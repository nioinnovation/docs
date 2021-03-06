# User interfaces
In the nio Platform, user interfaces (UIs) are treated as first-class citizens within a [system](/systems/README.md), capable of both subscribing to and producing signals. Traditional application architectures normally route all signals through a central server, which increases network traffic and load on that centralized infrastructure.

[Pubkeeper](/pubkeeper/README.md), niolabs’s patent-pending peer-to-peer pub/sub communication product, allows UIs to subscribe to signals by leveraging the Pubkeeper browser client. The UI then secures the requested data directly from any node within a nio system that is able to publish it. The data may also be sent to or picked up from a central location like a websocket server or a Kafka store.

At niolabs, we use React for UI development. We have created a themed collection of React components we call the [UI Kit](https://uikit.niolabs.com), as well as a turn-key UI Scaffold that makes use of the UI Kit to get you started visualizing your system with nio and Pubkeeper.

Check out our [quickstart guide](/ui/build-a-ui.md) to building a UI for a nio system. Please note that [Node.js](https://nodejs.org/en/) is required in order to build with the UI Kit

---

## Related links

> _The following resources are designed to provide helpful information to nio users regarding the development and integration of user interfaces within a nio system. **They are not official niolabs products and are not supported as such**._

* You can clone the nio UI Scaffold here: https://github.com/niolabs/ui-scaffold.git
* You can learn about how to use the nio UI Kit here: https://uikit.niolabs.com
* And if you need additional help, you can learn more about React here: https://reactjs.org/
