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

We can perfoem constructor injection using `@Inject` annotation on the constructor of a class.

```kotlin
class AnalyticsAdapter @Inject constructor(
    private val service: AnalyticsService
) { ... }
```

<br/>

### Hilt Modules

Sometimes a type cannot be constructor-injected. This can happen for multiple reasons. For example, you cannot
constructor-inject an interface. You also cannot constructor-inject a type that you do not own, such as a class from an
external library. In these cases, you can provide Hilt with binding information by using Hilt modules.

A Hilt module is a class that is annotated with @Module. Like a Dagger module, it informs Hilt how to provide instances
of certain types. Unlike Dagger modules, you must annotate Hilt modules with `@InstallIn` to tell Hilt which Android
class each module will be used or installed in.

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object MyCustomModule 
```

<br/>

### Inject interface instances with `@Binds`

We cannot directly inject interfaces in android, for that we need to use `@Binds`.

```kotlin
interface AnalyticsService {
    fun analyticsMethods()
}

// Constructor-injected, because Hilt needs to know how to
// provide instances of AnalyticsServiceImpl, too.
class AnalyticsServiceImpl @Inject constructor(
    ...
) : AnalyticsService { ... }

@Module
@InstallIn(ActivityComponent::class)
abstract class AnalyticsModule {

    @Binds
    abstract fun bindAnalyticsService(
        analyticsServiceImpl: AnalyticsServiceImpl
    ): AnalyticsService
}
```

<br/>

### Inject instances with `@Provides`

Constructor injection is also not possible if you don't own the class because it comes from an external library (classes
like Retrofit, OkHttpClient, or Room databases) for thar we need to use `@Provides`.

```kotlin
@Module
@InstallIn(ActivityComponent::class)
object AnalyticsModule {

    @Provides
    fun provideAnalyticsService(
        // Potential dependencies of this type
    ): AnalyticsService {
        return Retrofit.Builder()
            .baseUrl("https://example.com")
            .build()
            .create(AnalyticsService::class.java)
    }
}
```

<br/>

### Provide multiple bindings for the same type using `@Qualifiers`

When we want two objects of same type in this case we need to use `@Qualifiers`.

For e.g. if we need to create two instances of OkHttp client object, then we can do in following way:

```kotlin
@Qualifier
@Retention(AnnotationRetention.BINARY)
annotation class AuthInterceptorOkHttpClient

@Qualifier
@Retention(AnnotationRetention.BINARY)
annotation class OtherInterceptorOkHttpClient
```

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    @AuthInterceptorOkHttpClient
    @Provides
    fun provideAuthInterceptorOkHttpClient(
        authInterceptor: AuthInterceptor
    ): OkHttpClient {
        return OkHttpClient.Builder()
            .addInterceptor(authInterceptor)
            .build()
    }

    @OtherInterceptorOkHttpClient
    @Provides
    fun provideOtherInterceptorOkHttpClient(
        otherInterceptor: OtherInterceptor
    ): OkHttpClient {
        return OkHttpClient.Builder()
            .addInterceptor(otherInterceptor)
            .build()
    }
}
```

We can also inject this specific object using constructor and field injection

```kotlin
// As a dependency of a constructor-injected class.
class ExampleServiceImpl @Inject constructor(
    @AuthInterceptorOkHttpClient private val okHttpClient: OkHttpClient
) : ...

// At field injection.
@AndroidEntryPoint
class ExampleActivity : AppCompatActivity() {

    @AuthInterceptorOkHttpClient
    @Inject
    lateinit var okHttpClient: OkHttpClient
}
```

<br/>

### Predefined qualifiers in Hilt

Hilt provides some predefined qualifiers. For example, as you might need the Context class from either the application
or the activity, Hilt provides the `@ApplicationContext` and `@ActivityContext` qualifiers.

```kotlin
class AnalyticsAdapter @Inject constructor(
    @ActivityContext private val context: Context,
    private val service: AnalyticsService
) { ... }
```

<br/>

### Generated components for Android classes

For each Android class in which you can perform field injection, there's an associated Hilt component that you can refer
to in the `@InstallIn` annotation. Each Hilt component is responsible for injecting its bindings into the corresponding
Android class.

The previous examples demonstrated the use of ActivityComponent in Hilt modules.

<br/>

#### Hilt provides the following components:

| Hilt component             | Injector for                              |
|----------------------------|-------------------------------------------|
| SingletonComponent         | Application                               |
| ActivityRetainedComponent	 | NA                                        |
| ViewModelComponent         | ViewModel                                 |
| ActivityComponent          | Activity                                  |
| FragmentComponent          | Fragment                                  |
| ViewComponent              | View                                      |
| ViewWithFragmentComponent	 | View annotated with @WithFragmentBindings |
| ServiceComponent           | Service                                   |

<br/>

#### Component lifetimes

Hilt automatically creates and destroys instances of generated component classes following the lifecycle of the
corresponding Android classes.

| Generated component	       | Created at	             | Destroyed at          |
|----------------------------|-------------------------|-----------------------|
| SingletonComponent	        | Application#onCreate()	 | Application destroyed |
| ActivityRetainedComponent  | Activity#onCreate()	    | Activity#onDestroy()  |
| ViewModelComponent         | ViewModel created	      | ViewModel destroyed   |
| ActivityComponent          | Activity#onCreate()	    | Activity#onDestroy()  |
| FragmentComponent	         | Fragment#onAttach()	    | Fragment#onDestroy()  |
| ViewComponent	             | View#super()	           | View destroyed        |
| ViewWithFragmentComponent	 | View#super()	           | View destroyed        |
| ServiceComponent           | Service#onCreate()	     | Service#onDestroy()   |

<br/>

#### Component scopes

| Android class	                             | Generated component	       | Scope                   |
|--------------------------------------------|----------------------------|-------------------------|
| Application                                | SingletonComponent         | @Singleton              |
| Activity                                   | ActivityRetainedComponent	 | @ActivityRetainedScoped |
| ViewModel                                  | ViewModelComponent         | @ViewModelScoped        |
| Activity                                   | ActivityComponent	         | @ActivityScoped         |
| Fragment                                   | FragmentComponent          | @FragmentScoped         |
| View                                       | ViewComponent              | @ViewScoped             |
| View annotated with @WithFragmentBindings	 | ViewWithFragmentComponent	 | @ViewScoped             |
| Service                                    | ServiceComponent           | @ServiceScoped          |

In the example, if you scope AnalyticsAdapter to the ActivityComponent using @ActivityScoped, Hilt provides the same
instance of AnalyticsAdapter throughout the life of the corresponding activity:

```kotlin
@ActivityScoped
class AnalyticsAdapter @Inject constructor(
    private val service: AnalyticsService
) { ... }
```

Or we can create singleton object using modules:

```kotlin
// If AnalyticsService is an interface.
@Module
@InstallIn(SingletonComponent::class)
abstract class AnalyticsModule {

    @Singleton
    @Binds
    abstract fun bindAnalyticsService(
        analyticsServiceImpl: AnalyticsServiceImpl
    ): AnalyticsService
}

// If you don't own AnalyticsService.
@Module
@InstallIn(SingletonComponent::class)
object AnalyticsModule {

    @Singleton
    @Provides
    fun provideAnalyticsService(): AnalyticsService {
        return Retrofit.Builder()
            .baseUrl("https://example.com")
            .build()
            .create(AnalyticsService::class.java)
    }
}
```

<br/>

### Inject dependencies in classes not supported by Hilt

Hilt comes with support for the most common Android classes. However, you might need to perform field injection in
classes that Hilt doesn't support. For that we need to use `@EntryPoint` annotation.

<br/>

### Use Hilt with other Jetpack libraries

Hilt includes extensions for providing classes from other Jetpack libraries. Hilt currently supports the following
Jetpack components:

1. ViewModel
2. Navigation
3. Compose
4. WorkManager

<br/>

#### Inject ViewModel objects with Hilt

Provide a `ViewModel` by annotating it with `@HiltViewModel` and using the `@Inject` annotation in the ViewModel
object's
constructor.

```kotlin
@HiltViewModel
class ExampleViewModel @Inject constructor(
    private val savedStateHandle: SavedStateHandle,
    private val repository: ExampleRepository
) : ViewModel() {
    ...
}
```

Then, an activity or a fragment that is annotated with `@AndroidEntryPoint` can get the ViewModel instance as normal
using
ViewModelProvider or the by `viewModels()` KTX extensions:

```kotlin
@AndroidEntryPoint
class ExampleActivity : AppCompatActivity() {
    private val exampleViewModel: ExampleViewModel by viewModels()
    ...
}
```

<br/>

#### Hilt in Navigation Components

```kotlin
val viewModel: ExampleViewModel by hiltNavGraphViewModels(R.id.my_graph)
```

<br/>

#### Work Manager

Inject a Worker using the `@HiltWorker` annotation in the class and `@AssistedInject` in the Worker object's constructor.
You can use only `@Singleton` or unscoped bindings in Worker objects. You must also annotate the Context and
WorkerParameters dependencies with `@Assisted`:

```kotlin
@HiltWorker
class ExampleWorker @AssistedInject constructor(
  @Assisted appContext: Context,
  @Assisted workerParams: WorkerParameters,
  workerDependency: WorkerDependency
) : Worker(appContext, workerParams) { ... }



@HiltAndroidApp
class ExampleApplication : Application(), Configuration.Provider {

    @Inject lateinit var workerFactory: HiltWorkerFactory

    override fun getWorkManagerConfiguration() =
        Configuration.Builder()
            .setWorkerFactory(workerFactory)
            .build()
}
N
```