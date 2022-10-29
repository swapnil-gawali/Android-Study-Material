# Process, Threads & MultiThreading

### Process

Process is a stance of a programme or application. When we launch an application, operating system create process and
launch that application into that process.

<br/>

### MultiThreading

When multiple threads are executed in parallel at the same time, this process is known as Multithreading.

> In simple words, if application is using multiple threads is called as MultiThreaded application.

MultiThreading takes advantages of multiple cores available in our CPU. If application is not using threads then it is
basically using single core of the machine.

<br/>

### Thread Safety

When multiple threads working on same data and the value is changing, that scenario is not thread-safe, and we will get
inconsistent result.

When a thread is already working on an object and preventing another thread on working on the same object, this process
is called thread-safety.

<br/>

### Thread

A Thread is a lightweight process which allow us to perform complex tasks in the background.

<br/>

#### Runnable Interface

Java runnable is an interface used to execute code on a concurrent thread. It is an interface which is implemented by
any class if we want that the instances of that class should be executed by a thread.

The runnable interface has an undefined method run() with void as return type, and it takes in no arguments. The method
summary of the `run()` method.

> In simple words, Runnable used to start a new thread and any class that implements `Runnable` interface runs on a
> different thread.

<br/>

#### Creating and Starting Thread

For creating a Thread, we need to create object of it and pass a class which implements `Runnable` interface.

We can start thread using `thread.start()` method.

##### Using Runnable interface

```java
public class ExampleThread implements Runnable {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
        System.out.println("Executing task on thread");
    }
}

public class Main {
    public static void main(String[] args) {
        // creating thread
        Thread thread = new Thread(new ExampleThread());
        // starting thread
        thread.start();
    }
}
```

```text
// output
Thread-0
Executing task on thread
```

> We can check on which thread out task is running using `Thread.currentThread().getName()`

<br/>

##### Using `Thread` class

```java
public class ExampleThread extends Thread {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
        System.out.println("Executing task on thread");
    }
}

public class Main {
    public static void main(String[] args) {
        // creating thread
        Thread thread = new ExampleThread();
        // starting thread
        thread.start();
    }
}
```

<br/>

#### `Thread` class vs `Runnable` interface

Both used to perform operation on background thread but when we `extend` a `Thread` class then we cant extend any
other class but on other hand if we implement `Runnable` interface, then we can extend any other class if needed.

> `Thread` class implements `Runnable` interface

<br/>

#### Pausing a Thread

We can pause execution of the Thread using `Thread.sleep(timeInMillis)` method.

```java
public class ExampleThread implements Runnable {
    @Override
    public void run() {
        try {
            Thread.sleep(5000); // pausing execution for 5 seconds
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("Task finished.");
    }
}
```

<br/>

#### Joining Threads

Let's say we have to execute a task on Thread or Main Thread but after finishing of another Thread.

For e.g., we are downloading file from server using thread, after download complete we want to perform read/write
operation on that file on another background thread.

```java
public class DownloadFileThread implements Runnable {
    @Override
    public void run() {
        try {
            Thread.sleep(5000); // waiting for 5 seconds
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("File download finished.");
    }
}

public class ReadFileThread implements Runnable {
    @Override
    public void run() {
        System.out.println("File read successfully.");
    }
}

public class Main {
    public static void main(String[] args) {
        Thread downloadFileThread = new Thread(new DownloadFileThread());
        downloadFileThread.start();

        try {
            downloadFileThread.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        Thread readFileThread = new Thread(new ReadFileThread());
        readFileThread.start();
    }
}
```

In the above example, ReadFileThread will wait until DownloadFileThread execution finishes.

> In simple words, when we have two threads and we need to execute them sequentially by one after another, then we need
> to `join()` thread.

<br/>

#### Interrupting a Thread

We can interrupt a thread when we want to cancel an operation using `Thread.interrupt()` method.

For e.g., lets say downloading a file taking a very long time, and we want to cancel that operation then we can
interrupt that download file thread.

```java
public class DownloadFileThread implements Runnable {
    @Override
    public void run() {
        System.out.println("File download started");

        for (int i = 0; i < Integer.MAX_VALUE; i++) {
            // check if current thread is interrupted
            // if interrupted then exit from the for loop
            if (Thread.currentThread().isInterrupted()) {
                return;
            }

            System.out.println("Downloading byte : " + i); // simulating fake download operation
        }

        System.out.println("File download finished.");
    }
}

public class Main {
    public static void main(String[] args) {
        Thread downloadFileThread = new Thread(new DownloadFileThread());
        downloadFileThread.start();

        try {
            Thread.sleep(1000); // sleeping main thread for 1 seconds
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // interrupting a thread
        downloadFileThread.interrupt();
    }
}
```

> Interrupting a thread means cancelling a thread.

<br/>

#### Concurrency Issues & `synchronized` keyword

When multiple threads try to access or update object it causes concurrency issue and application may crash or behave
abnormally. This is also called as **Race Condition** i.e. multiple threads are competing or racing to modify the data.
To avoid this, we use `synchronized` keyword.

> Java Synchronization is better option where we want to allow only one thread to access the shared resource.


When we use `synchronized` keyword, it automatically acquires the lock for that object and releases it when the thread
completes its task.

#### `synchronized` method

```java
public class Main {
    int counter = 0;

    synchronized void incrementObject() {
        System.out.println(counter);
        counter++;
    }
}
```

#### `synchronized` block

```java
public class Main {
    int counter = 0;

    void incrementObject() {
        synchronized (this) {
            System.out.println(counter);
            counter++;
        }
    }
}
```

<br/>

#### `volatile` keyword

For performance point of view, jvm caches an object and stored them inside a cpu and changes made to the cached object
are only local to the cpu, and not visible to the other thread. When we mark an object as a `volatile` keyword, it does
not cache that object.

When a thread changes a data, another thread cannot see those changes, and it is called visibility issue. `volatile`
keyword make sure the changed data is visible to the another thread. It does not prevent race condition, but only
prevent visibility issue.

---

# Coroutines

Coroutine allows us to write non-blocking asynchronous kotlin code without blocking main thread.

### Threads vs Coroutines

Threads are used to execute task concurrently.

Creating too many threads can actually make an application underperformed in some situations and can lead to
the `Out Of Memory` errors. To overcome these issues, Kotlin introduced a new way of writing asynchronous, non-blocking
code; the Coroutine.

Similar to threads, coroutines can run in concurrently, wait for, and communicate with each other with the difference
that creating them is way cheaper than threads.

Coroutines are the basically lightweight threads which spawns thousands or millions of coroutines inside a single thread
without impacting applications performance.

> **Note:** When we create coroutine, it basically spawns a single background thread and launch coroutine inside it.

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

> `runBlocking {}` can also be calling from main thread.

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
job.cancel()
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

### Structured Concurrency

Structured Concurrency is the combination of language features and best practices.

#### Ideas behind the Structured Concurrency:

- Cancel work when no longer needed (so we don't waste resources)
- Keep track of work while it's running
- Signal errors on failure

<br/>

### Scopes

Scopes are mechanism to manage the coroutines.

Scopes are used to define lifetime of the coroutine. This prevents any leaked resources, memory leaks etc.

### GlobalScope

Coroutines launch within this scope with tied to the lifetime of the application. For e.g., If we use GlobalScope inside
MainActivity to execute some code, and later this activity gets destroyed then execution inside this scope will lead to
memory leaks.

We should never use this scope.

### coroutineScope

Using this scope we can manage the lifecycle of the coroutine. Coroutines or suspend function launched withing this
block is executed in sequential manner by asynchronously without blocking thread.

```kotlin
fun main(args: Array<String>) {
    runBlocking {
        doWork()
        println("Coroutine returns")
    }
}

suspend fun doWork() = coroutineScope {
    doDatabaseWork()
    doAPICall()
    println("all work done")
}

suspend fun doDatabaseWork() {
    delay(1000)
    println("inserting data into db")
}

suspend fun doAPICall() {
    delay(2000)
    println("fetching data from server")
}
```

In the above example, `doWork()` method executes its code inside coroutineScope. Once `doDatabaseWork()`
and `doAPICall()`suspend functions finishes their execution, `doWork()` method will return. So it is basically similar
to the `lifecycleScope`, `viewModelScope` in android.


<br/>

### Custom Coroutine Scope

When we want a scope which lifecycle is tied to specific element or view for e.g. Fragment lifecycle then we can use our
own scope. Coroutines running inside this scope will automatically get destroyed when the lifecycle of the fragment is
destroyed.

Android provides its own scopes like `viewModelScope`, `lifecycleScope` etc. When we use these scopes Android manages
cancellation of these scopes. Foe e.g. `ViewModel` gets destroyed then its scopes also gets destroyed.

<br/>

### Dispatchers

Dispatcher determines which thread coroutine is run on.

#### Coroutines can run on following Dispatchers:

- Default - CPU bound work, will exhaust CPU thread pool if we create multiple coroutines.
- Main - Runs on the Main thread of the application
- IO - Used for network call, file read/write, DB operations etc. and uses expandable thread pool
- Other - Custom dispatchers

The difference is that Dispatchers.Default is limited to the number of CPU cores (with a minimum of 2) so only N (where
N == cpu cores) tasks can run in parallel in this dispatcher.

On the IO dispatcher there are by default 64 threads, so there could be up to 64 parallel tasks running on that
dispatcher.

The idea is that the IO dispatcher spends a lot of time waiting (IO blocked), while the Default dispatcher is intended
for CPU intensive tasks, where there is little or no sleep.

<br/>

#### Coroutine Context

The context determines on which thread the coroutines will run. There are four options:

1. `Dispatchers.Default` - for CPU intense work (e.g. sorting a big list)
2. `Dispatchers.Main` - what this will be depending on what you've added to your programs runtime dependencies (e.g.
   kotlinx-coroutines-android, for the UI thread in Android)
3. `Dispatchers.Unconfined` - runs coroutines unconfined on no specific thread
4. `Dispatchers.IO` - for heavy IO work (e.g. long-running database queries)

<br/>

#### `withContext()`

`withContext()` is used to switch context i.e. `withContext()` will switch execution into a different thread and get
back to the original dispatcher when it completes. This suspending function is cancellable by default, and it will throw
cancellable exception.

> `withContext()` is `suspend` function and it can only be called with `CoroutineScope`

For e.g. if we're doing database operation inside `CoroutineScope` with `IO` dispatcher, and we need to assign that
result to the `TextView`. So in this case we can switch context from `IO` to `Main` using `withContext()`.

```kotlin
lifecycleScope.launch(Dispatchers.IO) {
    val result = fetchDataFromDatabase() // fetchDataFromDatabase() is suspend function which returns string

    withContext(Dispatchers.Main) {
        button.text = result
    }
}
```

<br/>

#### Async Await

When we want to call multiple suspend method simultaneously then we can use `async` & `await`. `async` `await` returns
Deferred object which similar to the promise in javascript.

When we call multiple suspend function inside coroutines, then it basically execute them in sequentially i.e. call first
suspend function wait for finishing its work and after finishing first task it will execute second suspend function. So
the async await solves this issue.

```kotlin
suspend fun fetchTwoDocs() =
    coroutineScope {
        val deferredOne = async { fetchDoc(1) }
        val deferredTwo = async { fetchDoc(2) }
        deferredOne.await()
        deferredTwo.await()
    }

// OR

suspend fun fetchTwoDocs() =        // called on any Dispatcher (any thread, possibly Main)
    coroutineScope {
        val deferreds = listOf(     // fetch two docs at the same time
            async { fetchDoc(1) },  // async returns a result for the first doc
            async { fetchDoc(2) }   // async returns a result for the second doc
        )
        deferreds.awaitAll()        // use awaitAll to wait for both network requests
    }
```


