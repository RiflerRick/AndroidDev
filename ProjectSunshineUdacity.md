## Project Sunshine

In the process of building this project from udacity, there were a number of things learnt. This document focusses on those learnt things.

### layouts

**Components in the layout**
An activity creates views to show the user information, and to let the user interact with the activity. Views are a class in the Android UI framework. They occupy a rectangular area on the screen and are responsible for drawing and handling events. An activity determines what views to create (and where to put them), by reading an XML layout file. These XML files, as Dan mentioned, are stored in the res folder inside the folder labeled layouts. 

### Types of views

There are 2 major types of views. The first one is called UI components. These include components like TextView, EditText, ImageView etc. The `android.widget` package contains most of the UI view classes.

The second type are called the 'Layout' or 'Container' Views. They extend from a class called ViewGroup. They are generally responsible for containing a number of views and determining where they are placed on the screen. Basically views will be nested inside the tag of another view and so on. Examples of Container Views include LinearLayout, RelativeLayout and so on. 

### XML attributes

Views have xml attributes which control the properties of the view. Properties are stuff like textSize and padding. 
- Width and Height: 2 of the most important properties are the width and the height. For the sake of making the view responsive and adjust to various screen size 2 very common values are used in the case of width and height: 

```
android:layout_width='wrap-content'
android:layout_height='match-parent'
```
wrap content will shrink the view to wrap whatever is displayed within the view. For example if the text in a view is 'helloworld' then wrap-content will set the width of the view to be exact-width of the text 'helloworld' on screen.
match parent as the name suggests will make the view as large as the parent inside which this view is nested.

- Difference between padding and margin: 
This is very important to understand not only in terms of android material design paradigms but also when it comes to UI design in general. People often confuse between padding and margin and forget what each one does and how one differs from the other. 
    - Padding has to do with the view component itself and not its parent component. Lets say we have a TextView with a background color of blue that says 'hello'. If we add 8 dps of padding on all sides then the textview will be expanded in all directions by 8 dps. Instead if we had added margin of 8dps on all sides then the component would be shifted by 8 dps from all sides of the screen. 
    - Margins work on the parent component which is generally a viewgroup. The parent component kind of shifts its child component considering the margin. Mind you that if in the case of the TextView, there would be no background color then we would not be able to tell any difference in adding a padding of 8dps or a margin of 8dps on all sides.
    - We declare the padding and margin both on the child view(textview in this case). The padding gets handled by the child view itself whereas the margin gets handled by the parent component generally a viewgroup.
    
    `android:padding="8dps"` also `android:paddingLeft="8dps"`, `android:paddingTop="8dps"` and so on. The  default value of padding is zero.
    
    For margin it is: `android:layout_margin="8dps"`. Think of margin as a force-field around the textview inside which no component is allowed to come inside. Similarly we have `android:layout_marginLeft="8dps"` and so on. Notice the word 'layout' in the attribute name. This tells that it refers to the viewgroup element. 
    
    According to material design libraries, it is necessary to keep increments of 8dps always.
      
#### How xml attributes relates to activities:

This is an important section as it tells us, how xml components are bound with activities. Binding actually happens in the `onCreate()` method of Activity(or the class that extends the activity class). The `setContentView()` method is used to inflate the layout as necessary. You pass a reference to the layout file as R.layout.name_of_layout in the setContentView() function. For example

```
public class MainActivity extends AppCompatActivity{
    @Override
    protected void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);// mind you in our own onCreate() function, this super class function must always be called
        setContentView(R.layout.activity_main);//in this case the layout file name is activity_main.xml, we pass the filename without the extension
    }
    //other code
}

```
The setContentView(<instance name>) actually inflates the view given by the xml file.

#### The R class

When the application is compiled, the R class is generated.  It creates constants that allow you to dynamically identify the various contents of the res folder, including layouts. 

#### Accessing Resources

Once we provide a resource in our application, we can apply it by referencing it using what is called its resource id. All resource ids are present in the project's R class which the aapt tool automatically generates when our source code is compiled. These resources include all resources present in our res folder. For every 'type' of resource there is a subclass of R present and for every resource there is a static integer (for example R.drawable.icon).
 
- Resource id: A resource id is always composed of a resource type and a resource name. Resource types are specific types of resources. For each resource there is always a specific type, examples can be 'string','drawable' and 'layout'. The resource name is either the filename excluding the extension or the value in the xml attribute(`android:name`)

There are basically 2 ways of accessing a resource:

- In code: Using a static integer from a subclass of the R class such as R.string.hello, string being the resource type and hello being the resource name. For exmaple we can use a resource in code by passing the resource id as a method parameter.

```
ImageView imageView = (ImageView) findViewById(R.id.myimageview);
imageView.setImageResource(R.drawable.myimage);
```

The syntax to reference a resource would be:

`[<package_name>.]R.<resource_type>.<resource_name>`

`[<package_name>]`- This is optional and is only required if we are referencing a resource from another package.
`resource_type` is the R subclass and `resource_name` is the name of the resource or the static integer, either the filename without extension or the name in the xml file(basically the value for the xml attribute `android:name`)

- In xml: 
```
<Button
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:text="@string/submit" />
```
You can define values for some XML attributes and elements using a reference to an existing resource. You will often do this when creating layout files, to supply strings and images for your widgets. 

Syntax would be:
`@[<package_name>:]<resource_type>/<resource_name>`

#### Accessing Original Files

While uncommon, you might need access your original files and directories. If you do, then saving your files in res/ won't work for you, because the only way to read a resource from res/ is with the resource ID. Instead, you can save your resources in the assets/ directory.

Files saved in the assets/ directory are not given a resource ID, so you can't reference them through the R class or from XML resources. Instead, you can query files in the assets/ directory like a normal file system and read raw data using `AssetManager`.

However, if all you require is the ability to read raw data (such as a video or audio file), then save the file in the res/raw/ directory and read a stream of bytes using `openRawResource()`.

#### setContentView() method:
So what is the setContentView method doing? It inflates the layout. Essentially what happens is that Android reads your XML file and generates Java objects for each of the tags in your layout file. You can then edit these objects in the Java code by calling methods on the Java objects. We’ll go over what these methods look like and how to access view in your layout files later in the course.

### Accessing xml components using the xml 'id' attribute:(accessing resources using xml)

First in the xml page, the component is given an id in the following way: 

```
<TextView   
    android:id="@+id/toyName"
/>
```
Notice how the id is given. This is reminiscent of the way we access resources using xml. `@+id/toyName`
- `@` tells the tools not to treat whatever is written inside the quotes as a string literal and to instead look for the contents inside the android resources. 

- `+` tells the tools that we are dealing with a component that does not yet exists and should be created anew.

After it is just the type of resource and the name of the resource as discussed already. 

Now in the java code we create a TextView instance and use the method `findViewById()` method to assign a TextView instance to it.

```
TextView myTextView;
myTextView=(TextView)findViewById(R.id.toyNames);
```
Obviously the nomenclature of the resource instance passed as a parameter to findViewById() is the same. R.resource_type.resource_name.
 
 pro-tip: `\n\n\n` means a single `\n` in the view in android.
 
 