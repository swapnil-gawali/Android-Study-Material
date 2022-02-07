# Lifecycle Aware Components

Lifecycle Aware Components used to observe lifecycle state of the Activity and Fragments.

<br/>

Lifecycle Aware Components has two methodology:

1. **Lifecycle Owner** : Activity, Fragment etc. We can register observer using `lifecycle.addObserver` method.
2. **Lifecycle Observer** : Custom classes or listeners who observe lifecycle events of the observer. Observer must
   implement `DefaultLifecycleObserver` interface which contains default methods for observing Activity/Fragment
   lifecycle events.

<br/>


`MainActivity.kt`

```kotlin
class MainActivity : AppCompatActivity() {

    private val TAG = MainActivity::class.java.simpleName

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        Log.wtf(TAG, "Activity onCreate")
        lifecycle.addObserver(MyObserver())
    }

    override fun onStart() {
        super.onStart()
        Log.wtf(TAG, "Activity onStart")
    }

    override fun onResume() {
        super.onResume()
        Log.wtf(TAG, "Activity onResume")
    }

    override fun onPause() {
        super.onPause()
        Log.wtf(TAG, "Activity onPause")
    }

    override fun onStop() {
        super.onStop()
        Log.wtf(TAG, "Activity onStop")
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.wtf(TAG, "Activity onDestroy")
    }
}
```

`MyObserver.kt`

```kotlin
class MyObserver : DefaultLifecycleObserver {

    private val TAG = MyObserver::class.java.simpleName

    override fun onCreate(owner: LifecycleOwner) {
        Log.wtf(TAG, "Observer onCreate")
    }

    override fun onStart(owner: LifecycleOwner) {
        Log.wtf(TAG, "Observer onStart")
    }

    override fun onResume(owner: LifecycleOwner) {
        Log.wtf(TAG, "Observer onResume")
    }

    override fun onPause(owner: LifecycleOwner) {
        Log.wtf(TAG, "Observer onPause")
    }

    override fun onStop(owner: LifecycleOwner) {
        Log.wtf(TAG, "Observer onStop")
    }

    override fun onDestroy(owner: LifecycleOwner) {
        Log.wtf(TAG, "Observer onDestroy")
    }
}
```

```text
Activity onCreate
Observer onCreate
Activity onStart
Observer onStart
Activity onResume
Observer onResume
Observer onPause
Activity onPause
Observer onStop
Activity onStop
Observer onDestroy
Activity onDestroy
```

In the above example, `MyObserver` is observing `MainActivity`'s lifecycle event. For adding observer, we have
used `lifecycle.addObserver(MyObserver())` in MainActivity.

For observing changes in `MyObserver` class, we have implemented `DefaultLifecycleObserver` interface.

> On Construction phase, Activity lifecycle method will before observers methods. On Destruction phase, Observer methods will execute then
> Activity lifecycle methods will get executed.

**Construction Methods Output:**

```text
Activity onCreate
Observer onCreate
Activity onStart
Observer onStart
Activity onResume
Observer onResume
```

**Destruction Methods Output:**

```text
Observer onPause
Activity onPause
Observer onStop
Activity onStop
Observer onDestroy
Activity onDestroy
```
