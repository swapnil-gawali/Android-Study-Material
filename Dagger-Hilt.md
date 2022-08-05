# Dependency injection in Android

Dependency Injection is passing dependency to the object.

Classes often require references to other classes. For example, a `Car` class might need a reference to an `Engine`
class. These required classes are called dependencies, and in this example the `Car` class is dependent on having an
instance of the `Engine` class to run.

Implementing dependency injection provides you with the following advantages:

1. Re-usability of code
2. Ease of refactoring
3. Ease of testing

<br/>

### Dagger Hilt

Hilt is Jetpack's recommended library for dependency injection in Android. Hilt defines a standard way to do DI in your
application by providing containers for every Android class in your project and managing their lifecycles automatically
for you.

Hilt is built on top of the popular DI library Dagger to benefit from the compile time correctness, runtime performance,
scalability, and Android Studio support that Dagger provides.

<br/>

### Hilt application class

All apps that use Hilt must contain an Application class that is annotated with `@HiltAndroidApp`.

`@HiltAndroidApp` triggers Hilt's code generation, including a base class for your application that serves as the
application-level dependency container.

```kotlin
@HiltAndroidApp
class MyApplication : Application()
```

This generated Hilt component is attached to the Application object's lifecycle and provides dependencies to it.
Additionally, it is the parent component of the app, which means that other components can access the dependencies that
it provides.

<br/>

### Inject dependencies into Android classes

Once Hilt is set up in your Application class and an application-level component is available, Hilt can provide
dependencies to other Android classes that have the `@AndroidEntryPoint` annotation:

```kotlin
@AndroidEntryPoint
class ExampleActivity : AppCompatActivity() { ... }
```

Hilt currently supports the following Android classes:

1. Application (by using @HiltAndroidApp)
2. ViewModel (by using @HiltViewModel)
3. Activity
4. Fragment
5. View
6. Service
7. BroadcastReceiver

If you annotate an Android class with `@AndroidEntryPoint`, then you also must annotate Android classes that depend on
it. For example, if you annotate a fragment, then you must also annotate any activities where you use that fragment.

<br/>

### Field Injection

To obtain dependency in our components, we can use field injection as follows.

```kotlin
@AndroidEntryPoint
class ExampleActivity : AppCompatActivity() {

    @Inject
    lateinit var analytics: AnalyticsAdapter
    ...
}
```

> **Note:** Fields injected by Hilt cannot be private. Attempting to inject a private field with Hilt results in a
> compilation error.

Classes that Hilt injects can have other base classes that also use injection. Those classes don't need the
`@AndroidEntryPoint` annotation if they're abstract.

<br/>

### Constructor Injection

