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

### Starting an Activity:

An activity can be started on demand via an intent passed as a arguement to the `startActivity(<intent>)` method. The intent is an implicit intent typically although it can also be an exlicit intent except that explicit intents are generally used in case of services rather than activities. startActivity method passes this intent to the ActivityManagerService, the ActivityManagerService then does the necessary intent filtering and resolution to that intent and sees which are the activities that can handle the intent. Mind you here that the intents are not resolved programmatically in any way, these intents are resolved by the android framework's activity manager service and hence the programmer does not need to know or care of what activities are going to be called here.
 
 Then the control passes on target activity's onCreate() method and the intent is passed to this activity.
 
 Following are the methods that are required to start an Activity:
 - `void startActivity(Intent intent)`- This will simply launch a new activity
 - `void startActivityForResult(Intent intent, int requestCode)`- starts an activity from which you would want a result when finished. 
 
 It is obviously clear that the method to choose depends on the return result needed from the activity if any.
 
 Now understand that the `startActivity(Intent intent)` method is pretty straightforward in that it is a callback function, hence does not block other operations, moreover since this function simply calls the activity and does not request for any result, its implementation is less complex and easy to make however when it comes to the other method, `startActivityForResult(Intent intent, inr requestCode)`, its implementation is little more complicated, however we can do that slow and steady.
 
 **`startActivityForResult(Intent intent, int requestCode)`**
 
 
 
 
 
 
 
