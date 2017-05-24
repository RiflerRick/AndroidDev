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

### Elements in an Android Intent

Elements in an android intent are the following: name, action, data, category, extras, flags.
The action, data and extras are of interest to the component receiving the intent whereas the others are of interest to the android framework itself.

There are 2 types of intents: explicit intent and implicit intent. Explicit intents are the ones in which the intent has the name of the recipient service or activity. These are tightly coupled intents whereas the intent may not have the name of the recipient in which case they become the loosely coupled intent and it is upon the android activity manager to figure out which services or activities have registered for that particular intent. 

#### Intent API

The intent api in android has many methods to work with. For example an intent can be used in the following manner:

```
Intent intent=new Intent(ACTION_VIEW, data);//data here is the Uri possibly
intent.putExtra("Today","22ndMay2017");
intent.putExtra("Pi",3.14);
intent. putExtra("isHot",true);
intent.putExtra("answer",42);

startActivity(intent);
```
obviously as it was said intents are like key value pairs and the final startActivity() method is used to start the activity with the intent. senders can use setter methods. 

The component that actually gets the intents have getter methods to get the intent

```
Intent i=getIntent();
Uri data=i.getData();
String s=i.getStringExtra("Today");
Float f=i.getFloatExtra("Pi");
```

We can get the various fields using getter methods however it is upto us to get the proper types right.

### Android intent elements in detail:

- Name: Identifies the name element that can receive an intent. It is obviously optional as is clear from the discussion of implicit and explicit intents. A named or explicit intent is sent to the instance of a designated class. 

Creating an explicit intent:

```
Intent intent=new Intent(this, C.class);
```

`this` is basically referring to the object being called which is generally an activity and `C.class` indicates the class that you are making an explicit intent for.

Another way of doing this same thing would be the following:

```
Intent intent=new Intent().setComponent(name).setClass(this, C.class);
```

the methods setClass and setComponent return an intent and hence it is possible to do this. **Explicit intents are typically used to bind components inside the same application. Explicit intent is particularly important when we are binding services. In recent versions of android it is not possible to bind a service with an implicit intent.**
 
 An implicit intent as had been said earlier is the one where the component to be used to figured out at runtime.
 
 Creating an implicit intent:
 
 ```
 Intent i=new Intent(Intent.ACTION_VIEW, uri);
 
 ```
 
 There is also another way to do this:
 
 ```
 Intent i=new Intent().setAction(Intent.ACTION_VIEW).setData(uri);
 
 ```
 
 implicit intents are typically used to interact with components that reside in other apps.
 
 **Implicit intents are typically used to bind activity to activity relationships and activity to boradcast receiver component relationships. As said earlier in case of activity to service relationships the explicit intents are used.**
 
 - Action: An action generally describes an operation to perform or an event that has already occurred. An operation to perform represents and activity whereas an event that has occurred represents a broadcast receiver. An action is similar to a method name with a set of arguments. An action is designated with a java string. An action should be as specific as possible. For example these are sets of actions typically used: ACTION_CALL, ACTION_EDIT, ACTION_BATTERY_LOW, ACTION_HEADSET_PLUG and so on.
    
    An action is generally defined as a protocol. For example the ACTION_VIEW action(predefined by the Intent class) is used to simply display data to the user. The input is the uri from which to retrieve the data and there is no output at all. Similarly ACTION_PICK is the action which is used to pick an item from a list. The input is the URI of the directory of the data from which to pick an item and the ouput is the URI of the item that was picked.
    
  - Data: This elment defines the data that the action element should act upon. For example the ACTION_CALL action, the data column would be tel:<uri>
  
  ```
  Intent i=new Intent(Intent.ACTION_CALL, Uri.parse("tel:+911"));
  
  ```
  The Uri.parse is important as the data needs to be formatted as a uri.
  
  Also,
  
  ```
  Intent i=new Intent(Intent.ACTION_CALL).setData(Uri.parse("tel:+911"));
  
  ```
  
  Similarly:
  
  ```
  Intent i=new Intent(Intent.ACTION_VIEW, Uri.parse("geo:"+"36.1486"+","+"86.4152"));
  
  ```
  
  The type of data can also be explicitly set in the Intent object. 
  ```
  Intent makeGalleryIntent(String pathToFile){
  return new Intent(Intent.ACTION_VIEW).setDataAndType(Uri.parse("file://"+pathToFile),"image/*");
  }
  ```
  Here `image/*` is the type of data 
     
  - Extras: Extras provide additional information along with an intent. Simply key value pairs. 
  
  ```
  Intent i=new Intent(Intent.ACTION_SEND).putExtra(android.content.Intent.EXTRA_EMAIL, new String[]{"rajdeep.mukherjee295@gmail.com","rajdeepmukhrj@gmail.com"});
  
  ```
    
  - Category: A category is simply the element that provides the intent with the type of component that can handle an intent. It provides this information to the android activity manager service. There are various types of predefined categories:
  
  CATEGORY_DEFAULT: Allows activity to be started by an implicit Intent when no specific category is assigned to it. Similarly others.
    
  ```
  Intent i=new Intent(Intent.ACTION_VIEW).addCategory(Intent.CATEGORY_BROWSABLE);
  ```
  - Flags: Again used to set different types of flags. Is of interest to the android activity manager
  
  ```
  Intent i =new Intent(Intent.ACTION_SEND).setFlags(Intent.FLAG_ACTIVITY_NO_HISTORY| Intent.FLAG_DEBUG_LOG_RESOLUTION);
  ```
  
  The new activity is not kept in the history stack. The FLAG_DEBUG_LOG_RESOLUTION is used to print debug messages during the resolution of an intent to show whats been found to create the final resolved list.
  
### Android Manifest file

The AndroidManifest.xml file is used to provide certain types of information to the app. Actually a lot of information. 
- app java package
- components, classes and capabilities- which components are exported, component names and which processes will be used to host the components, what intents are handled by the components.
- permissions the app must have
- minimum and target api levels. gradle build file

Key elements of the android manifest file:

<application></application> tag: it is a container for specifying various app components. 
<activity></> tag: implements part of apps visual interface. 
<service></> tag: implement long duration background operations. In general services should not have intent filters.
<receiver></> tag: Broadcast receivers can be used by apps to receive broadcast intents even when other app components are not running.
<provider></> tag: Supply structured access to data managed by apps.
 
 In case of an explicit intent the filters in an androidManifest.xml file are not consulted. An implicit intent is delivered to a target component only if a filter matches. Intent filters describe which types of intents a component can handle. The activity manager service uses the intent filters to match proper intents and bind intents to specific activities. The extra and flag components play no part isn resolving which intent is to be bound to which target activity. An implicit intent is tested in all 3 areas ie. There can be multiple intent filters assigned to a particular activity.
 
 Statically declared intents are specified and defined declaratively in the android manifest.xml file.
  
 It is also possible however to dynamically configure an intent. This means that we define intents through java code.
   
   ```
   final BroadcastReceiver mReceiver=new SendBroadcastReceiver();
   
   IntentFilter intentFilter=new IntentFilter(Intent.ACTION_SEND);
   intentFilter.setType("*/*");
   registerReceiver(mReceiver, intentFilter);
   ```
   
   
   
   
 
  
   