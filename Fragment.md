# Fragment

Fragment is a reusable portion of UI.

Fragment is always hosted in Activity and has its own lifecycle.

<br/>

### Creating Fragment

```kotlin
class ExampleFragment : Fragment(R.layout.example_fragment)
```

<br/>

#### Add a fragment via XML

```xml
<!-- res/layout/example_activity.xml -->
<androidx.fragment.app.FragmentContainerView
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/fragment_container_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:name="com.example.ExampleFragment"/>
```

`android:name` attribute used to instantiate fragment when the activity is created for the first time.

> `FragmentContainerView` is used to host fragments instead of the previously common way of using FrameLayouts. In
> addition to correctly handling Fragment transactions under the hood, it has some additional features that coordinate
> with Fragment behavior.
>
> Under the hood, `FragmentContainerView` extends `FrameLayout`.

<br/>

#### Add a fragment programmatically

```xml
<!-- res/layout/example_activity.xml -->
<androidx.fragment.app.FragmentContainerView
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/fragment_container_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
```

```kotlin
class ExampleActivity : AppCompatActivity(R.layout.example_activity) {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        if (savedInstanceState == null) {
            supportFragmentManager.commit {
                setReorderingAllowed(true)
                add<ExampleFragment>(R.id.fragment_container_view)
            }
        }
    }
}
```

Unlike the XML approach, the `android:name` attribute isn't used on the `FragmentContainerView` here, so no specific
fragment is automatically instantiated. Instead, we use FragmentTransaction to instantiate Fragment.

<br/>

### Sending Data From One Fragment To Another

```kotlin
class ExampleActivity : AppCompatActivity(R.layout.example_activity) {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        if (savedInstanceState == null) {
            val bundle = bundleOf("some_int" to 0) // sending data
            supportFragmentManager.commit {
                setReorderingAllowed(true)
                add<ExampleFragment>(R.id.fragment_container_view, args = bundle)
            }
        }
    }
}
```

The arguments `Bundle` can then be retrieved from within your fragment by calling `requireArguments()`, and the
appropriate
`Bundle` getter methods can be used to retrieve each argument.

```kotlin
class ExampleFragment : Fragment(R.layout.example_fragment) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        val someInt = requireArguments().getInt("some_int") // retrieving data
        //...
    }
}
```

<br/>

### Fragment Manager

`FragmentManager` is the class responsible for performing actions on your app's fragments, such as adding, removing, or
replacing them, and adding them to the back stack.

<br/>

#### Access the FragmentManager

Every `FragmentActivity` and subclasses thereof, such as `AppCompatActivity`, have access to the FragmentManager through
the `getSupportFragmentManager()` method.

Fragments are also capable of hosting one or more child fragments. Inside a fragment, you can get a reference to the
`FragmentManager` that manages the fragment's children through `getChildFragmentManager()`. If you need to access its
host
`FragmentManager`, you can use `getParentFragmentManager()`.

<br/>

#### Child fragments

When you add fragment inside another fragment then it is called as Child Fragment. Child Fragment Manager can be
accessed using `getChildFragmentManager()`.

<br/>

### Fragment Transactions

At runtime, a `FragmentManager` can add, remove, replace, and perform other actions with fragments in response to user
interaction. Each set of fragment changes that you commit is called a transaction.

<br/>

#### Adding Fragments

```kotlin
val fragmentManager = ...
val fragmentTransaction = fragmentManager.beginTransaction()

fragmentManager.commit {
    setReorderingAllowed(true)
    // Add ExampleFragment in the fragment_container view
    add<ExampleFragment>(R.id.fragment_container)
}
```

> ****Important Note****: The `fragment-ktx` module provides a commit block that automatically calls beginTransaction
> and
> commit for you. So we do not need to create transaction object like `beginTransaction()`. For that, instead of
> creating FragmentManager object, we need to use `supportFragmentManager`, example below.

```kotlin
supportFragmentManager.commit {
    ...
}
```

<br/>

#### Replacing Fragment

```kotlin
// Create and commit a new transaction
supportFragmentManager.commit {
    setReorderingAllowed(true)
    // Replace whatever is in the fragment_container view with this fragment
    replace<ExampleFragment>(R.id.fragment_container)
}
```

<br/>

#### Removing Fragment

For removing a fragment from host, first we need to retrieve fragment object using `findFragmentById()`
or `findFragmentByTag()`.

#### Finding fragment by Id

```kotlin
supportFragmentManager.commit {
    replace<ExampleFragment>(R.id.fragment_container)
    setReorderingAllowed(true)
    addToBackStack(null)
}

val fragment = supportFragmentManager.findFragmentById(R.id.fragment_container) as ExampleFragment
```

#### Finding fragment by tag

```kotlin
supportFragmentManager.commit {
    replace<ExampleFragment>(R.id.fragment_container, "tag")
    setReorderingAllowed(true)
    addToBackStack(null)
}

val fragment = supportFragmentManager.findFragmentByTag("tag") as ExampleFragment
```

#### Removing fragment

```kotlin
supportFragmentManager.commit {
    setReorderingAllowed(true)
    val fragment = supportFragmentManager.findFragmentById(R.id.fragment_container) as ExampleFragment

    // OR

    val fragment = supportFragmentManager.findFragmentByTag("tag") as ExampleFragment
    remove(fragment)
}
```

<br/>

#### Hide Fragment

```kotlin
supportFragmentManager.commit {
    setReorderingAllowed(true)
    val fragment = supportFragmentManager.findFragmentById(R.id.fragment_container) as ExampleFragment
    hide(fragment)
}
```

<br/>

#### Show fragment

```kotlin
supportFragmentManager.commit {
    setReorderingAllowed(true)
    val fragment = supportFragmentManager.findFragmentById(R.id.fragment_container) as ExampleFragment
    show(fragment)
}
```

<br/>

### Fragment Lifecycle

![Android Activity Lifecycle](images/fragment-view-lifecycle.png)

### `onCreate()`

Initialize essential components and variables of the Fragment in this callback. The system calls this method when the
Fragment is created. Anything initialized in `onCreate()` is preserved if the Fragment is paused and resumed.

### `onCreateView()`

Inflate the XML layout for the Fragment in this callback. The system calls this method to draw the Fragment UI for the
first time. As a result, the Fragment is visible in the Activity. To draw a UI for your Fragment, you must return the
root `View` of your Fragment layout. Return `null` if the Fragment does not have a UI.

In simple words, this method renders UI.

### `onViewCreated()`

This method calls when fragment finishes UI creation. It is good place to initialize views like button, register
broadcast receiver etc.

### `onViewStateRestored()`

This method used to restore state of the fragment.

### `onStart()`

The system invokes this method to make the fragment visible on the user’s device.

### `onResume()`

This method is called to make the visible fragment interactive. i.e. User can interact with UI like Button, EditText
etc.

### `onPause()`

It indicates that the user is leaving the fragment. System call this method to commit the changes made to the fragment.

### `onStop()`

Method to terminate the functioning and visibility of fragment from the user’s screen.

### `onDestroyView()`

This call can occur if the host Activity has stopped, or the Activity has removed the Fragment.

### `onDestroy()`

It is called to perform the final clean up of fragment’s state and its lifecycle.

<br/>

### Fragment Lifecycle Associate with Activity Lifecycle

| Activity    | Fragment                                                             |
|-------------|----------------------------------------------------------------------|
| onCreate()  | onAttach()<br/>onCreate()<br/>onCreateView()<br/>onActivityCreated() |
| onStart()   | onStart()                                                            |
| onResume()  | onResume()                                                           |
| onPause()   | onPause()                                                            |
| onStop()    | onStop()                                                             |
| onDestroy() | onDestroyView()<br/>onDestroy()<br/>onDetach()                       |

> Note: While construction, Activity lifecycle gets called first. In destruction, fragment methods gets called first.

#### When User Rotates screen:

| Activity    | Fragment                                                             |
|-------------|----------------------------------------------------------------------|
| onPause()   | onPause()                                                            |
| onStop()    | onStop()                                                             |
| onDestroy() | onDestroyView()<br/>onDestroy()<br/>onDetach()                       |
| onCreate()  | onAttach()<br/>onCreate()<br/>onCreateView()<br/>onActivityCreated() |
| onStart()   | onStart()                                                            |
| onResume()  | onResume()                                                           |

#### When User Press Home Button:

| Activity   | Fragment   |
|------------|------------|
| onResume() | onResume() |
| onStop()   | onStop()   |

#### When User Resume App from Home:

| Activity   | Fragment   |
|------------|------------|
| onStart()  | onStart()  |
| onResume() | onResume() |

> Note: When popup appears before fragment, NO lifecycle method gets called.

<br/>

### Fragment Communication

#### Fragment to Fragment communication:

There are two ways to perform fragment to fragment communication

1. Using shared `ViewModel`
2. Fragment Result API

<br/>

##### 1. Using Shared `ViewModel`

In this case host activity has ViewModel and in this ViewModel contains LiveData which can be observed in child
fragments.

##### 2. Using Fragment Result API

```kotlin
// sending data
setFragmentResult("requestKey", bundleOf("bundleKey" to result))
```

```kotlin
// receiving data
setFragmentResultListener("requestKey") { requestKey, bundle ->
    // We use a String here, but any type that can be put in a Bundle is supported
    val result = bundle.getString("bundleKey")
    // Do something with the result
}
```

<br>

### Fragment Back Press Listener

We can intercept back press using `onBackPressedDispatcher` callbacks.

```kotlin
requireActivity().onBackPressedDispatcher.addCallback(requireActivity(), object : OnBackPressedCallback(false) {
    override fun handleOnBackPressed() {
        // do some logic on back press

        if (gotoBackScreen) {
            isEnabled = false
            requireActivity().onBackPressed()
        }
    }
})
```

<br/>

### `getContext()`, `requireContext()`, `getActivity()`, `requireActivity()`

`getContext()` and `getActivity()` returns nullable activity or context if activity and our app will crash.

But, with `requireContext()` and `requireActivity()` always returns non-null activity or context or
throws `IllegalStateException` if activity is null.

<br>

### Fragment Backstack