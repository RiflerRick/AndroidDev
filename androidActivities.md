## Android Activities

Android activities are at the heart of all android applications. An activity defines a user facing operation that is displayed on a device screen. For example showing a login prompt, reading an email message, composing an email message and so on. An app can obviously have more than one activity and these activities are described in the AndroidManifest.xml file. 

Steps in implementing activities:

- extend the `Activity` class. 
- Next we override selective lifecycle methods in the subclass.
- We can obviously define our own methods for implementing various other things and finally we need to update the AndroidManifest.xml file to include the activity so that android knows about it.

One way of implementing the basic structure of an application would be to extend the Activity class and make an abstract class called LifeCycleLoggingActivity and override a few basic lifecyclelogging methods in that abstract class and then make another MainActivityClass that would extend this LifeCycleLoggingActivity class and implement other functionalities.

The logging class simply means that when the logging methods are called back, all that class is doing is simply logging stuff into the console.  
 
 In the main.xml file which is basically used for writing the xml for the front-end of the application or the look and feel of the application, there is an event called onclick which takes a string which is the name of the method in the custom class that implements the activity.
 
 ### Android Activity Life Cycle
 
 Android activities are implemented as an object oriented framework which basically means they implement inversion of control via callbacks and various hook methods. **Inversion of control** is sometimes referred to as the hollywood principle which means, don't call us we will call u back when necessary. Objects such as specific activities are registered with the Android activity framework and various hook methods are automatically called back when certain events occur. Hook methods customize reusable framework classes to run app specific logic.  Hook methods are virtual methods that can be overriden my sub classes and sometimes be also implemented as lambda expressions.
   
  Examples of suck hook methods are the following:
  
  - onCreate() and onDestroy()- called when activities are created and destroyed.
  - onRestart(), onStart(), onResume(), onPause(), onStop() and so on.
  
The best way to understand the activity lifecycle is through a UML diagram:

![](https://raw.githubusercontent.com/RiflerRick/AndroidDev/master/assets/starting.PNG)
![](https://raw.githubusercontent.com/RiflerRick/AndroidDev/master/assets/running.PNG)
![](https://raw.githubusercontent.com/RiflerRick/AndroidDev/master/assets/paused.PNG)
![](https://raw.githubusercontent.com/RiflerRick/AndroidDev/master/assets/stopped.PNG)
![](https://raw.githubusercontent.com/RiflerRick/AndroidDev/master/assets/destroyed.PNG)


 
 
 
 
