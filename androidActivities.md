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
 This method is also asynchronous but is a 2 way method which means we need to use it when we wanna call another activity and also get some results from that activity. The second argument is basically used to identify the request to activity 2 so that when activity 2 is done and sends a response to activity 1 it can handle that request appropriately. 
  
 Inside activity 2 is a method called `setResult(RESULT_OK, data)` and this result code and data will be sent in turn to the activity 1 for making use of it in activity 1. Now in activity 1 is the method `onActivityResult(inr requestCode, inr resultCode, Intent data)`. This method is returned via a callback from the androidActivityFramework. One thing we get back is actually the request code which is used to see if the request code we required is the one we are actually getting from the activity that sent us the request.
   
 ### Finishing an Activity
 
 When an activity is done it can set its result using the `setResult()` method. It can take in as its parameter, stuff like the result code as well as the intent which would contain the data that would be used by the source activity for other purposes. A result code may be RESULT_OK, or RESULT_CANCELLED
 
 Finally we would call the method `finish()`
 
 LifeCycle hook methods are called in the context of UI threads:
 
 - onCreate()- called to initialize an activity when its first created. In a sense it plays the role of a virtual constructor. Typically it is used to initialize global activity state, stuff like inflating, configuring and caching UI views as needed as well as setting the activity's content view which is how the activity displays itself to the user.
 
 - onStart()- called when the activity is becoming visible to the user. This may be called back many times during the lifecycle of an activity. Typically used to reset app state and behaviour for example can be used to load data from persistent storage. 
 
 -onResume()- called when the activity will start interacting with the user. When onResume() returns the activity has focus and can respond to user input. Typically used for starting foreground behaviours like animations.
 
 -onPause()- called when the user leaves an activity but it is stil visible on the background. It has no longer got the focus of control however it has not disappeared from the screen. onPause() should be used to perform important cleanup operations such as committing unsaved state or perhaps releasing system resources such as broadcast receivers and so on.
 
 -onStop()- is called when the activity is no longer visible to the user. Typically used to perform non-essential operations such as caching which is good to perform but if not performed will not result in problems or the app behaving in a buggy fashion.
 
 -onRestart()- is called when the activity comes to foreground after being stopped.
 
 **One important thing to note is that when we create our own implementation of the onCreate() method it is the programmer's job to call the super class implementation of the onCreate() method also in order for the code to function properly because the super class method actually does a few more things important to the system.**
 
 ### Managing Multiple Activities in the Task BackStack
 
 It is a very basic algorithm which defines how android manages multiple activities using a structure called a backstack which is very similar in structure and function to the callstack used in case of functions in programming languages.
 
 Whenever a new activity is on the screen that activity gets loaded on top of the backstack and whenever the we press the backbutton or the activity gets paused, destroyed and so on, it is popped out of the stack.
  
 ### Design Constraints on Activity Concurrency
 
 First of all android apps have only one UI thread. All components in a process use the same UI thread. They use these thread for various things such as receive system notifications, broadcasts, interacting with users and preforming activity life cycle operations. The UI toolkit components must only be accessed by the UI thread. Because internally they are not synchronized and hence they can cause concurrency hazards like race conditions and so on if they were to come from separate threads. All activity methods run in the UI thread ad must be of short duration and they should be non-blocking otherwise the 'Application Non Responding' error or 'ANR' will arise. **Hence any long-running operations such as downloading files, getting content from remote databases, reading and writing to databases and so on must be done on separate background thread and concurrency frameworks should be used to handle such tasks.**
  
  ### Android Concurrency Frameworks:
  
  - HAMER framework: Handlers, Messages and Runnables - In this framework, operations can run in one or more threads and when they are ready to publish their results they can do so to the UI thread.
  
  - Async Task Framework: Here also operations run in one or more background threads and publish the result to the UI thread without directly using threads, handlers, messages and runnables. The Async task framework in a sense abstracts away most of the lower level details.
 
 
 
 
 
 
 
