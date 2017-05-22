## Android Intents

Android Intents are simply messages that one component uses to interact with and request functionality from other components. Beware that we are saying components not apps themselves. One app itself can have a number of components and each component may interact with each other using intents and this is an important idea. For example lets say we have 3 apps. The first app itself has 2 activities each of which communicates using intents. Similarly activity 2 in app1 uses an intent to communicate with a receiver activity in app3 and activity 1 in app1 uses an intent to interact with a service in app 2. App development can therefore be simplified as existing components can be reused as black boxes. Simply speaking we can reuse components such that we do not need to worry about the implementation, rather we can simply focus on the interface of the component

### Example:

Consider an image downloading app that downloads an image from a remote web server and displays it to the user. One way to write the app would be to write the whole app from scratch but that would be tedious and error prone. We can rather create it using a model having 3 components- A MainActivity, a download service and a gallery activity. So we can create an image downloading component that simply downloads an image from a remote server and stores it in the device. Then the URI or path can be given to the main activity using an intent and again another intent can pass the URI to the gallery activity.

An android intent is considered to be a passive data structure. It just consists of fields and field accessor and mutator methods and is not object oriented. The data in the fields can be passed from one component to another.

Apps can use intents to describe 2 different kinds of things.

- It can define an operation to perform. For example the ACTION_VIEW intent is used to display data to the user. Similarly the ACTION_CALL intent can be used to call (as in phone call) someone specified by the user. Android delivers the intent to the activity or service that can best handle it.

- An intent can also be used to describe an event that has already occurred. For example the ACTION_BATTERY_LOW intent can be used to indicate that the battery is low on an android device. Similarly we have ACTION_AIRPLANE_MODE_CHANGED. Once again android delivers the intent to broadcast receivers that can best handle it.

### A little more in detail

A component invokes a method to send an intent to 0 or more components. These methods are obviously provided by android itself.
The `startActivity()` and `startActivityForResult()` methods can be used to launch an activity with an action intent. There may be more than one activity that matches an intent. The method `sendBroadcast()` method and other related methods can be used to send event intents to broadcast receivers. The `startService()` method and the `bindService()` method can be used to launch and communicate with a service via an intent. Unlike activities or Receivers where there may be multiple recipients of intents, an intent can only be sent to one service. All these method calls are asynchronous and hence do not block the caller.

### Late Runtime Binding with Intents

Intents offer late runtime binding to components which simply means that components can be used at runtime rather than at compile time. These decisions can be made dynamically. So if there are lets say 3 apps and each app has a service or an activity that it is running then the connections via intents happen at runtime rather than at compile time. An app can also have multiple processes running inside them. 

**The mapping of components to processes is done in the AndroidManifest.xml file.**

In general there are tradeoffs between performance and robustness. If there are many components in the same process it may improve performance but hamper robustness. Runtime discovery of components is in general very powerful and helpful. The components don't have to know about each other and can be extended and integrated dynamically. An app choose can be used for instance to select which app to select for running a specific process.
