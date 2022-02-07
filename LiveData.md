# LiveData

LiveData is an observable data holder class. LiveData is lifecycle aware, and it only emits events when
Activity/Fragment is active lifecycle state like `onResume()` etc.

Whenever LiveData updates its data, it sends updates to the observer.

#### The advantages of using LiveData:

* **Ensures your UI matches your data state:** <br/><br/>LiveData follows the observer pattern. LiveData notifies
  Observer objects when underlying data changes. You can consolidate your code to update the UI in these Observer
  objects. That way, you don't need to update the UI every time the app data changes because the observer does it for
  you.
  <br/><br/>
* **No memory leaks:** <br/><br/>Observers are bound to Lifecycle objects and clean up after themselves when their
  associated lifecycle is destroyed.<br/><br/>
* **No crashes due to stopped activities:** <br/><br/>If the observer's lifecycle is inactive, such as in the case of an
  activity in the back stack, then it doesn't receive any LiveData events.<br/><br/>
* **No more manual lifecycle handling:** <br/><br/>UI components just observe relevant data and don’t stop or resume
  observation. LiveData automatically manages all of this since it’s aware of the relevant lifecycle status changes
  while observing.<br/><br/>
* **Always up-to-date data:** <br/><br/>If a lifecycle becomes inactive, it receives the latest data upon becoming
  active again. For example, an activity that was in the background receives the latest data right after it returns to
  the foreground.<br/><br/>
* **Proper configuration changes:** <br/><br/>If an activity or fragment is recreated due to a configuration change,
  like device rotation, it immediately receives the latest available data.<br/><br/>
* **Sharing resources:** <br/><br/>You can extend a LiveData object using the singleton pattern to wrap system services
  so that they can be shared in your app. The LiveData object connects to the system service once, and then any observer
  that needs the resource can just watch the LiveData object.

<br/>

### Create LiveData objects

```kotlin
class SomeViewModel : ViewModel() {

    // Create a LiveData with a String
    val currentName: MutableLiveData<String> by lazy {
        MutableLiveData<String>()
    }

    // Rest of the ViewModel...
}
```

<br/>

### Observe LiveData objects

```kotlin
class NameActivity : AppCompatActivity() {

    // Use the 'by viewModels()' Kotlin property delegate
    // from the activity-ktx artifact
    private val viewModel: SomeViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Other code to setup the activity...

        // Create the observer which updates the UI.
        // Observe the LiveData, passing in this activity as the LifecycleOwner and the observer.
        viewModel.currentName.observe(this) { newName ->
            // Update the UI, in this case, a TextView.
            nameTextView.text = newName
        }
    }
}
```

> Get a `LifecycleOwner` that represents the Fragment's or Activity's View lifecycle.

<br/>

### MediatorLiveData

MediatorLiveData is a subclass of LiveData that allows you to merge multiple LiveData sources. Observers of
MediatorLiveData objects are then triggered whenever any of the original LiveData source objects change.

