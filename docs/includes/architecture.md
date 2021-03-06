The nio Platform is built for users like you to build real-world, innovative, and impactful solutions.

{% video start="3" %}https://www.youtube.com/watch?v=1d76U5_7riE{% endvideo %}

Every state, every change, and every interaction in our world is a signal—from the percentage of moisture in the soil beneath your feet, to the vibration of your car motor, to the individual characters within your last tweet. Every _thing_ is producing context that already has—or will soon—become digitally available in the Internet of Things era. But how can these trillions of disparate signals work harmoniously to produce any meaningful value?

nio was built to act as the nervous system for a world with trillions of unique [signals](/signals/README.md). It  allows you to create real-world, innovative, and impactful solutions with connected, distributed systems.

We have built solutions for [agriculture](https://niolabs.com/case-studies/agriculture), [machine learning](https://niolabs.com/case-studies/industrial), [enterprise IT](https://niolabs.com/case-studies/case-study-real-time-database-migration), [smart toys](https://niolabs.com/case-studies/raspberry-pi-car), and [industrial operations](https://niolabs.com/case-studies/case-study-industrial-operations-intelligence). We can’t wait to see what you build!

---
## Signals
[Signals](/signals/) make your nio systems go. They can be generated by hardware, software, or humans, and should be a fundamental consideration when designing with nio.

```json
{
  "datetime": "2017-09-02T10:00:00-04:00",
  "soil_moisture": "67",
  "relative_humidity": "33",
  "above_canopy_temp": "89",
  "below_canopy_temp": "78",
  "vine_id": "112n5os23520595136981387krh23"
}
```


---
## Blocks
A [block](/blocks/) is the component nio uses to apply logic to a signal. Blocks consume, alter, and publish signals. They are the basic units of functionality that allow unlimited interoperability and possibilities.

![](/img/intro-blocks.png)

---
## Service
Multiple nio blocks are intelligently connected to form a nio service. A [service](/services/) is a real-time process that can be connected with other services to create more complex processes.

![service](/img/intro-service.png)

---
## Instance
Services exist within a running version of nio, known as a nio [instance](/instances/). The nio instance can be installed on a chip, cloud, or anywhere in between to distribute processing and intelligence throughout an entire nio system.

![instance](/img/intro-instance.png)

---
## System
Just like your biological system is a group of organs working together to keep you alive, a nio [system](/systems/) is a group of distributed instances working together to achieve a common goal. Regardless of where the instances live within the system (chip, gateway, or cloud), they work in harmony on the same platform to listen, transform, share, and act upon signals with intelligence.

![system](/img/intro-system.png)

---
## <span class="allow-caps">System Designer</span>
The nio [System Designer](/system-designer) is the tool that allows you to arrange these blocks, services and instances to build your own nio systems. The only limitation is your imagination.

![system designer](/img/intro-systemdesigner.jpg)

## <span class="allow-caps">System Monitor</span>
The nio [System Monitor](/monitoring/system-monitor) is the tool that allows you to monitor the status of all deployed instances within nio systems. 
