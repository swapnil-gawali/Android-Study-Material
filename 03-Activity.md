# Activity

---

**There are four building blocks of Android:**

1. Activity
2. Services
3. Broadcast Providers
4. Content Providers

### Activity:

Activity is a visual representation of an app where user can interact with the UI elements.

### Intents:

Intents are intention to perform an action like start/launch Activity, start a Service, deliver broadcasts etc.

<br />

**Passing data from one Activity to another:**

```kotlin
Intent(this, SecondActivity::class.java).also {
    it.putExtra("key", "Hello World!")
}
```

**Retrieving data in Second Activity:**

```kotlin
intent.getStringExtra("key")
```

<br />

**Passing `Object` data between to Activities:**

passing `Object` between two activities between two Activities require `Object` to implement `Serializable` interface
or `Parcelable` interface.

<br/>

**Using Serializable:**

```kotlin
// class must implements Serializable interface

import java.io.Serializable

data class SomeObject(val message: String) : Serializable
```

**Passing Object from one Activity to another using Serializable:**

```kotlin
Intent(this, SecondActivity::class.java).also {
    it.putExtra("obj", SomeObject("Hello World!"))
}
```

**Retrieving Object in Second Activity:**

```kotlin
val someObj = intent.getSerializableExtra("obj") as SomeObject
Log.wtf(TAG, "onCreate: " + someObj.message)
```

<br />

**Using Parcelable:**

Kotlin has simplified the way to use Parcelable, and we just need to annotate class with `@Parcelize`, and it will
automatically make that class available for the data passing.

To use Kotlin's Parcelable, we need to add `kotlin-parcelize` extension on the app `build.gradle`

```kotlin
import android.os.Parcelable
import kotlinx.parcelize.Parcelize


@Parcelize
data class SomeObject(val message: String) : Parcelable
```

**Passing Object from one Activity to another using Parcelable:**

```kotlin
Intent(this, SecondActivity::class.java).also {
    it.putExtra("obj", SomeObject("Hello World!"))
}
```

**Retrieving Object in Second Activity:**

```kotlin
val someObj: SomeObject? = intent.getParcelableExtra("obj")
Log.wtf(TAG, "onCreate: " + someObj?.message)
```

<br />

**Passing data using Bundle object:**

When we directly pass data from one `Activity` to another without using `Bundle`
then under the hood it wraps that data inside the `Bundle` object.

**Passing data from one Activity to another using Bundle:**

```kotlin
Intent(this, SecondActivity::class.java).also {
    val bundle = Bundle()
    bundle.putString("key", "Hello World!")
    it.putExtras(bundle)
}
```

**Retrieving data in Second Activity:**

```kotlin
intent.getStringExtra("key")
```

<br/>

### AppCompatActivity:

When we need to use material design in our app then we can use `AppCompatActivity`.

<br/>

**Activity Lifecycle:**

![Android Activity Lifecycle](Android-Activity-Lifecycle.png)

**onCreate :**

Called when Activity is first time created. In this method we can set our layout, initialize views etc. This method also
provides Bundle object containing the Activity's previous state, if available.

**onRestart :**

Called when Activity has been stopped. For Eg. user has navigates to the second Activity and navigates back to the first
Activity then `onRestart()`
will be called.

**onStart :**

Called when the Activity becoming visible to the user.

**onResume :**

Called when the Activity will start interacting with the user.

**onPause :**

Called when user navigates to the other Activity or press back button or press home button.

**onStop :**

Called when Activity no longer visible to the user. This may happen either because of Activity is being destroyed or
user is navigating to the other Activity. Also, when user press home button this callback may trigger.

**onDestroy :**

Called when Activity is about to be destroyed. This can happen either because of the Activity is finishing
by `Activity.finish()` method or user press back button or system decided to destroy Activity to save memory for other
tasks. We can check if Activity is finishing or not using `isFinishing()` method.


> **Note:** On change of orientation, Activity is destroyed first &
> then recreated.

<br/>

**Activity Stack:**

Activities use "Last in First Out" (LIFO) to clear the stack of the activities.

<br/>

**Screen Orientation & Lifecycle of Activity:**

When screen orientation happens then Activity is recreated from scratch.

If we define unique id to the views in xml layout then some views can retain their previous state.

1. Views that by default retain their state on rotation:
    * Edittext
    * Checkbox
    * Switch
    * Radiobutton
2. Views that do not retain their state on screen rotation even when they have assigned unique id in their layout
    * Textview
    * Button

> **Note:** If we do not assign unique id to the views like Edittext, Checkbox, Switch, Radiobutton
> then they do not retain their state after orientation change.

<br/>

**Possible solutions to handle screen orientation:**

**1. Restoring and retrieving Activity state:**

Whenever orientation happens, there is a method called `onSaveInstanceState()` and it's gets called soon after
the `onPause()` callback. in this method we can store Activity state in `Bundle`.

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    // storing a value which can be retrieve after orientation change
    outState.putString("key", "Hello World!")
}
```

```kotlin
override fun onRestoreInstanceState(savedInstanceState: Bundle) {
    super.onRestoreInstanceState(savedInstanceState)
    val data = savedInstanceState.getString("key")
    Log.wtf(TAG, "onRestoreInstanceState: $data")
}
```

> **Note:** In `onCreate()` method we also receive `Bundle` object and this Bundle object
> also be used for restoring the state of the application.

<br />

**2. Restoring Activity state without recreating Activity:**

For achieve this scenario, we need to do the following changes:

* Declare `android:configChanges` attribute in `manifest.xml` file under the `<activity>` tag.
* override `onConfigurationChanged()` method in Activity class.

```xml
<?xml version="1.0" encoding="utf-8"?>
<activity
        android:name=".MainActivity"
        android:configChanges="orientation|screenSize">
</activity>
```

```kotlin

```

> **Note:** This is **NOT** recommended solution to handle configuration changes.

<br/>

**Start Activity & Get Result:**

Sometimes we want to get a result back from an activity when it ends. To do this, we can
call `startActivityForResult(intent: Intent, requestCode: Int)`

When the second Activity finishes, we can call `setResult(resultCode: Int, intent: Intent)`
to return data back to the previous Activity.

<br/>

**MainActivity.kt:**

```kotlin
class MainActivity : AppCompatActivity() {

    private val TAG = MainActivity::class.java.simpleName

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding.btnSecondActivity.setOnClickListener {
            // start another activity
            startActivityForResult(Intent(this, SecondActivity::class.java), 100)
        }
    }

    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        // here we get the result back
        Log.wtf(TAG, "onActivityResult: $resultCode")

        data?.let { intent ->
            Log.wtf(TAG, "onActivityResult: " + intent.getStringExtra("key"))
        }
    }
}
```

**SecondActivity.kt:**

```kotlin
class SecondActivity : AppCompatActivity() {

    private val TAG = SecondActivity::class.java.simpleName

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding.sendResult.setOnClickListener {
            // set result and finish the current Activity
            val intent = Intent()
            intent.putExtra("key", "Some data from SecondActivity")
            setResult(100, intent)
            finish()
        }
    }
}
```

> **Note:** From Android 12, `startActivityForResult()` method has been deprecated &
> we need to use `registerForActivityResult()` instead. Check following example:

```kotlin
// here app will receive result from Second Activity
private val startForResult =
    registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result: ActivityResult ->
        result.data?.let { intent ->
            Log.wtf(TAG, intent.getStringExtra("key"))
        }
    }
```

```kotlin
// start another activity for result
startForResult.launch(Intent(this, SecondActivity::class.java))
```

<br/>

**Tasks and the back stack:**

A task is a collection of activities that users interact with when trying to do something in your app. These activities
are arranged in a stack—the back stack—in the order in which each activity is opened. For example, an email app might
have one activity to show a list of new messages. When the user selects a message, a new activity opens to view that
message. This new activity is added to the back stack. Then, if the user presses or gestures Back, that new activity is
finished and popped off the stack.

> **Note:** Activities in the back stack are never rearranged.

<br/>

**Lifecycle Recap:**

* When Activity A starts Activity B, Activity A is stopped, but the system retains its state (such as scroll position
  and text entered into forms). If the user uses the Back or gesture while in Activity B, Activity A resumes with its
  state restored.
* When the user leaves a task using the Home button or gesture, the current activity is stopped and its task goes into
  the background. The system retains the state of every activity in the task. If the user later resumes the task by
  selecting the launcher icon that began the task, the task comes to the foreground and resumes the activity at the top
  of the stack.
* If the user presses or gestures Back, the current activity is popped from the stack and destroyed. The previous
  activity in the stack is resumed. When an activity is destroyed, the system does not retain the activity's state.
* Activities can be instantiated multiple times, even from other tasks.

<br/>

**Launch Modes:**

There are four launch modes for activity:

1. Standard
2. SingleTop
3. SingleTask
4. SingleInstance

**1. Standard:**

This is the default launch mode of activity. If you don’t set any launch mode to your activity, it will use the standard
mode by default. It creates a new instance of activity every time even if activity instance is already present.

Suppose we have A, B, C, and D activities and your activity B has standard launch mode. Now again launching activity B
State of Activity Stack before launch

B → A → B → C → D

State of Activity Stack after launch

B → A → B → C → D → B

We can see that new instance of B is created again.

**2. SingleTop:**

If an instance of activity already exists at the top of the current task, a new instance will not be created and the
Android system will route the intent information through `onNewIntent()`. If an instance is not present on top of the
task then a new instance will be created.

Suppose we have A, B, C, and D activities.

A → B → C → D

If we launch C then a new instance of C will be created as it is not on top. So it will look like

A → B → C → D → C

Now suppose we have

A → B → C → D → C

like this then we if again launch C activity then in this case new instance will not be created. Instead, we will
receive the callback on `onNewIntent()` method.

**3. SingleTask:**

An activity declared with launch mode as singleTask can have only one instance in the system (singleton). At a time only
one instance of activity will exist.

If activity instance is not present then the new instance will be created and if the instance is already present in the
system then the onNewIntent() method will receive the callback.

Suppose we have A, B, C activities(A → B → C) and we are launching D that has a singleTask launch mode. In that case,
the new instance of D will be created so the current state will look like this. (A → B → C → D)

Now let suppose if we launch B that also have has a singleTask launch mode then current state will look like this.

A → B

Here old instance gets called and intent data route through onNewIntent() callback. Also, notice that C and D activities
get destroyed here.

**SingleInstance:**

It is similar to singleTask except that no other activities will be created in the same task. If another Activity is
called from this kind of Activity, a new Task would be automatically created to place that new Activity.

_Case 1:_

Suppose you have A, B, and C activities(A → B → C) and your activity D has a singleInstance launch mode. In this case,
if we launch D then D will be launch in the different task. New task for D will be created.

Task1: A → B → C

Task2 : D (here D will be in the different task)

Now if you continue this and start E and D then Stack will look like

Task1: A → B → C → E

Task2: D

_Case 2:_

Suppose you have A, B, C activities in one task (A → B → C)and activity D is in another task with launch mode
singleInstance. In this case, if we launch D again then we will receive the callback in the `onNewIntent()` method of D
activity.

Task1: A → B → C

Task2: D (Here old instance gets called and intent data route through `onNewIntent()` callback)

**Launch Modes Using Intent Flags:**

**FLAG_ACTIVITY_NEW_TASK :**

Works same as `singleTask`

**FLAG_ACTIVITY_SINGLE_TOP :**

Works same as `singleTop`

**FLAG_ACTIVITY_CLEAR_TOP :**

If the activity being started is already running in the current task, then instead of launching a new instance of that
activity, all the other activities on top of it are destroyed and this intent is delivered to the resumed instance of
the activity (now on top), through `onNewIntent()`).

There is no value for the `launchMode` attribute that produces this behavior.

`FLAG_ACTIVITY_CLEAR_TOP` is most often used in conjunction with `FLAG_ACTIVITY_NEW_TASK`. When used together, these
flags are a way of locating an existing activity in another task and putting it in a position where it can respond to
the intent.

### Loaders:

The Loader API used to load data from content providers or other data source for display in an `FragmentActivity`
or `Fragment`.

**Some problems you might encounter without loaders:**

1. If you fetch the data directly in the activity or fragment, your users will suffer from lack of responsiveness due to
   performing potentially slow queries from the UI thread.


2. If you fetch the data from another thread, perhaps with `AsyncTask`, then you're responsible for managing both the
   thread and the UI thread through various activity or fragment lifecycle events, such as `onDestroy()` and
   configurations changes.

**Loaders solve these problems and includes other benefits. For example:**

1. Loaders run on separate threads to prevent janky or unresponsive UI.


2. Loaders simplify thread management by providing callback methods when events occur.


3. Loaders persist and cache results across configuration changes to prevent duplicate queries.


4. Loaders can implement an observer to monitor for changes in the underlying data source. For example, CursorLoader
   automatically registers a ContentObserver to trigger a reload when data changes.

<br/>

### Application Class

Base class for maintaining global application state. You can provide your own implementation by creating a subclass and
specifying the fully-qualified name of this subclass as the `android:name` attribute in your
AndroidManifest.xml's `<application>` tag. The Application class, or your subclass of the Application class, is
instantiated before any other class when the process for your application/package is created.

<br/>

### Context

The Context in Android is actually the context of what we are talking about and where we are currently present.

Few important points about the context:

1. It is the context of the current state of the application.
2. It can be used to get information regarding the activity and application.
3. It can be used to get access to resources, databases, and shared preferences, and etc.
4. Both the Activity and Application classes extend the Context class.

<br/>

#### Application Context:

It is an instance that is the singleton and can be accessed in activity via `getApplicationContext()`. This context is
tied to the lifecycle of an application. The application context can be used where you need a context whose lifecycle is
separate from the current context or when you are passing a context beyond the scope of activity.

**Example Use:** If you have to create a singleton object for your application and that object needs a context, always
pass the application context.

If you pass the activity context here, it will lead to the memory leak as it will keep the reference to the activity and
activity will not be garbage collected.

In case, when you have to initialize a library in an activity, always pass the application context, not the activity
context.

You only use `getApplicationContext()` when you know you need a Context for something that may live longer than any
other likely Context you have at your disposal.

<br/>

#### Activity Context:

This context is available in an activity. This context is tied to the lifecycle of an activity.

Whenever you are in Activity, for any UI operations like showing toast, dialogs etc., use the Activity Context.


