# ViewModel

ViewModel is used to **store** and **manage** UI related in data. Also, it works as a communication layer between UI and
repository.

In real world apps, its is best practice writing all the business logic in ViewModel and Activity is only responsible to
show or perform UI related changes.

**Few Notes About ViewModel:**

* ViewModels are lifecycle aware.
* ViewModel survives configuration changes.
* Destroy only if the owner activity/fragment is completely destroyed.
* `onCleared()` method will execute when ViewModel is about to destroy.

<br/>

### ViewModel Scope

![ViewModel Scope](images/viewmodel-lifecycle.png)

<br/>

### Example:

`MainViewModel.kt`

```kotlin
class MainViewModel : ViewModel() {

    fun getRandomNumber(): Int {
        // following code generates random number
        return (0 until 10).random()
    }
}
```

```kotlin
class MainActivity : AppCompatActivity() {
    private val TAG = MainActivity::class.java.simpleName

    private val viewModel: MainViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val randomNumber = viewModel.getRandomNumber()
        Log.wtf(TAG, "Random Number is : $randomNumber")
    }
}
```

In the above example, `MainViewModel` is generating random number between 0 and 10, and we are calling ViewModel method
from the activity and printing `randomNumber` variable.

<br/>

### Retrieving ViewModel in Activity/Fragment

We can get ViewModel in Activity using `by viewModels()` ktx method. If we do not use ktx, then we need to
use `ViewModelProvider.of()` to initialize ViewModel.

In Fragment, if we want to retrieve ViewModel associated with parent Activity, then we can use `by activityViewModels()`
. See example:

```kotlin
val viewModel: MainViewModel by activityViewModels()
```

<br/>

### `AndroidViewModel`

When we want to access Application Context inside ViewModel we can extend class using `AndroidViewModel`. It is child
class of `ViewModel`.

```kotlin
class MainViewModel(app: Application) : AndroidViewModel(app) {

    fun accessApplicationContext() {
        Log.wtf("MainViewModel", getApplication<Application>().getString(R.string.app_name))
    }
}
```

<br/>

### ViewModel Factory

ViewModelProviders can only instantiate ViewModels with no arg constructor. Hence, we cannot pass dependency into it. To
overcome this problem, we use ViewModelFactory.

But from Dagger-Hilt, all the dependency is automatically gets injected in the ViewModel. So we do not need to use
ViewModelFactory anymore.


