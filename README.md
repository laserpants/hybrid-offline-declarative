# Hybrid, offline and declarative

## Hybrid mobile web apps

Hybrid mobile web applications behave more like native apps, but are created using our everyday web technologies (HTML, CSS, and JavaScript). These applications run inside a native WebView container on the target device. From normal users' perspective, this means little or no difference in terms of overall experience. You find these applications in app stores online and install them to your device just like any other app. For application developers, these technologies offer some important advantages over vendor-proprietary SDKs. In particular, web technologies are

* already familiar to most developers, or otherwise easy to learn;
* inherently cross-platform; and
* founded on open standards.

UX designers can leverage their existing web design skills and rely on CSS toolkits (e.g. [Twitter Bootstrap](http://getbootstrap.com/)) to build sophisticated and highly interactive user interfaces with modern, responsive features. For businesses, the advantage is that their development team can target more than one platform, which means a single code base to maintain.

The overall trend today is that more and more applications run either in the browser, or in a browser-like environment. Language technologies are quickly adapting to keep up with this new reality. Compilers and other build tools are already capable of producing code that runs in legacy browsers and performs well even on low-end devices. In fact, modern JavaScript runtimes have performance characteristics comparable to native code execution.

Although there may be capabilities unique to a specific device which are unavailable using the hybrid app approach, plugins and native APIs can be utilized to target most platform-specific requirements.

Some questions to consider before making the decision to develop a hybrid app:

* What are the target devices?
* What does the development team's skill set look like?
* What capabilities of the device does the application need to utilize?

## Offline capabilities

Despite large-scale adaptation of smartphone technologies, bandwidth and network connectivity issues remain a barrier for wider acceptance of the browser as a service platform in the developing world. Slow and disruptive connections effectively render most web-based applications useless in these environments. Offline capabilities (an application's ability to continue working while disconnected) can therefore be crucial to a project's success in regions undergoing this transition from SMS and USSD based services to mobile web.

#### Ground computing as a catalyst to drive innovation

For a system to be able to operate effortlessly offline, the conventional client-server model is clearly inadequate. Since this aspect of system behavior is largely negelected in other parts of the world, modern cloud architecture is doing little more (if anything) to solve the problem. Instead we need to look beyond the cloud, to an architectural style sometimes referred to as *ground computing* -- applications that run on the device (i.e., on the ground) with robust data replication built in as an integral part of the software stack. 

The [Groundfork synchronization framework](https://github.com/johanneshilden/groundfork-js) was designed specifically for these environments. Important requirements were that:

* User experience is unaffected by network connectivity. (It shouldn't matter if the device is offline.)
* Syncing is effortless on the part of the user.
* The solution introduces no coupling between replication/synchronization and business logic. (App developer doesn't have to think about syncing.)
* No assumption is made w.r.t. replication topology.

Groundfork is open-source software and primarily intended to be used in browser-based applications, i.e., those using the hybrid app approach described earlier.

The solution implements a software design pattern known as the [Command pattern](https://en.wikipedia.org/wiki/Command_pattern). Access to application resources is relayed through a programming interface which enables the system to maintain a transaction log of changes while the device is offline. These changes are subsequently propagated and synchronized with other devices with the help of a web service, when connectivity is available. This service can run in the cloud or on a host system available to all network nodes. 

## Declarative programming techniques
