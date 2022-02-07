# Coroutines

Coroutine allows us to write non-blocking asynchronous kotlin code.

### Threads vs Coroutines

Threads are used to execute task concurrently.

Creating too many threads can actually make an application underperform in some situations; threads are objects which
impose overhead during object allocation and garbage collection.

To overcome these issues, Kotlin introduced a new way of writing asynchronous, non-blocking code; the Coroutine.

Similar to threads, coroutines can run in concurrently, wait for, and communicate with each other with the difference
that creating them is way cheaper than threads.

Coroutines are the basically lightweight threads which spawns thousands of coroutines inside a single thread.

<br>

### Suspend Function

Notes about `suspend` function:

1. This is a function marked with the `suspend` modifier.
2. Only adding `suspend` modifier doesn't change the behaviour of the function. i.e. this `suspend` modifier doesn't
   make function asynchronously. However, adding the modifier does change the compiled code.
3. `suspend` function may be long-running, and it never blocks the calling thread.
4. suspend function can only be called/run from the other suspend function or withing the coroutine.

For calling `suspend` function from the function which can't be marked as `suspend` for example, `onCreate()`
, `onPause()` etc., we need to create coroutine using coroutine builder.

```kotlin
suspend fun doSomeCalculations() {
    // doing some heavy calculations
}
```

### Coroutine Builder

Coroutine builder is bridge between non-suspending and suspending world.

Coroutine builders can run suspend function as well as normal function.

#### `GlobalScope.launch {}`

This coroutine builder use to launch coroutines in the global context and which is not recommended. Use this only for
testing purpose otherwise it will leak memory.

```kotlin
GlobalScope.launch {
    delay(1000)
    // do something here
}
```

#### `runBlocking {}`

This coroutine builder use to launch coroutines in the blocking manner. It will block the further execution util the
code inside this block will finish its execution.

If we call this function inside main thread, then it will block that thread until it finishes its execution and if we
call from other coroutine, then it will simply block that coroutine execution.

```kotlin
runBlocking {
    delay(1000)
    // do something here
}
```

<br/>

### `delay()` function

This method suspends coroutines for specific period of time but do not block a thread. It is also equivalent to the
postDelayed java.

```kotlin
GlobalScope.launch {
    delay(5000) // delay executions for the 5 seconds
}
```

<br/>

### Job

Whenever we launch any coroutines, it returns job which can be joined as wll as cancelled.

```kotlin
val job = GlobalScope.launch {
    delay(5000)
    print("finished execution")
}
```

<br/>

### Joining Coroutine

`join` is similar to joining a thread and the calling code blocks until the coroutine has finished.

```kotlin
 runBlocking {
    val job = GlobalScope.launch {
        delay(5000)
        print("finished execution")
    }

    job.join()
}
```

<br/>

### Cancelling Coroutine

```kotlin
job.cance()
```

This will cancel any running coroutines job.

<br/>

### Timeout

If we want certain coroutines cancel within a specific timout, then we can use withTimeout(millis) builder. It will
automatically cancel the coroutine after the specified time.

```kotlin
val job = withTimeout(1000) {
    // some heavy calculations which will take more than 1 seconds
    // if not finished within 1 second, it will cancel this coroutine
}
```

<br/>

### Scopes

Scopes are mechanism to manage the coroutines.

Scopes are used to define lifetime of the coroutine. This prevents any leaked resources, memory leaks etc.

### GlobalScope

Coroutines launch within this scope with tied to the lifetime of the application. For e.g., If we use GlobalScope inside
of MainActivity to execute some code, and later this activity gets destroyed then execution inside this scope will lead
to memory leaks.

We should never use this scope.

### coroutineScope

Using this scope we can manage the lifecycle of the coroutine. Coroutines or suspend function launched withing this
block is executed in sequential manner by asynchronously without blocking thread.

```kotlin
class MainActivity : AppCompatActivity() {

    private val TAG = MainActivity::class.java.simpleName

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        lifecycleScope.launch {
            doWork()
            Log.wtf(TAG, "Coroutine returns")
        }
    }

    private suspend fun doWork() = coroutineScope {
        doDatabaseWork()
        doAPICall()
        Log.wtf(TAG, "all work done")
    }

    private suspend fun doDatabaseWork() {
        delay(1000)
        Log.wtf(TAG, "inserting data into db")
    }

    private suspend fun doAPICall() {
        delay(2000)
        Log.wtf(TAG, "fetching data from server")
    }
}
```

In the above example, `doWork()` method executes its code inside coroutineScope. Once `doDatabaseWork()`
and `doAPICall()`suspend functions finishes their execution, `doWork()` method will return. So it is basically similar to
the `lifecycleScope`, `viewModelScope` in android.

