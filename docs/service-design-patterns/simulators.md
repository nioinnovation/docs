# Simulators

Most of the time the first service you build with n.io involves simulating signals using a `Simulator` block and then logging that data using a `Logger` block. That is the "Hello, World" service of n.io. However, simulators are much more powerful and do much more than generate random data. This document attempts to define and explain other use cases for simulators.

## Understanding Simulators

Before we get into too many use cases, you must understand how simulators work and the naming conventions. There are many types of simulator blocks, but once you understand the naming you'll be a pro at using them. The basic idea is that a simulator block consists of a "trigger" and a "generator." Each simulator block will be named `<Generator><Trigger>Simulator`. For example, if you want to use the `Counter` generator and the `Interval` trigger, then you would want to use the `CounterIntervalSimulator` simulator block.

### Triggers

Triggers define when a simulator should notify signals. Triggers do not care what the signals look like, they are only concerned with the timing of simulators.

Examples of triggers include:

* `Interval` - Notify signals at a fixed time interval \(for example, every 5 minutes\)
* `Cron` - Notify signals at certain times of the day, similar to how you would configure a [cron job](https://en.wikipedia.org/wiki/Cron)

### Generators

Generators define the shape and contents of the signals that are notified.

Examples of generators include:

* `Counter` - Each new signal generated will have an attribute that is incremented by some value
* `File` - Each new signal generated is pulled from a file of signals
* `Identity` - Each signal generated is an empty signal `{}`

## Driving Another Block

The general design pattern of n.io is that **signals** represent data and information and **blocks** represent functions and transformations. However, blocks need to know when and how to perform these functions. Some blocks may need to do an action periodically \(for example, every hour\) while some may need to wait for some external action \(for example, a button press\). Rather than try and define that logic in the block's configuration, we will use incoming signals to trigger the action instead. This also allows the block to "tap in to" the incoming signals and perhaps change its behavior based on the content of the signals. For example, rather than making a fixed HTTP request every interval, a block could send the contents of the incoming signal as part of the HTTP payload instead.

In some cases, you don't care about the contents of the incoming signal, but want to perform an action. For example, if you wanted to poll the Twitter API every five minutes for new tweets, you would need an incoming signal to the Twitter block when you wanted to poll. Since you don't care about the contents of the signal, any signal, even an empty signal, would suffice. You could use the `IdentityIntervalSimulator` to create such a signal and "drive" the polling of the Twitter block. The `Identity` generator could be used since we only need an empty signal. The `Interval` trigger could be used to poll the API periodically.

Because of the way we use simulators to kick off the actions of a service, almost every service will either begin with a `Subscriber` block \(to subscribe to signals from another service\) or a `Simulator` block \(to generate the driving signals on its own\).

## Testing and Custom Simulators

To test a service, you may want data that looks like what an actual signal would look like, but don't want to connect to a real data source. To do that, you could create a custom generator for your data format and then use a simulator block with that generator. The process of creating a custom generator requires writing python code and is documented in more detail on the [simulator repo's README](https://github.com/nio-blocks/simulator).

It is also possible, although not as flexible, to create custom simulated data without writing any code. By pairing a simulator with the `Identity` trigger with a `DynamicFields` block, as service builder can generate empty signals on a schedule of their choosing and then route that empty signal to a `DynamicFields` block where they could fill in the information that the signal should consist of. A service building expert would make use of the `random` module inside of a n.io expression to generate random data. For example, to generate a signal with a random number from 1 to 10 each time, connect the output of an `IdentityIntervalSimulator` block to a `DynamicFields` block as in the following example:

```
{
  "name": "Random Number",
  "type": "DynamicFields",
  "fields": [{
    "title": "num",
    "formula": "{{ __import__('random').randint(1,10) }}"
  }]
}
```


