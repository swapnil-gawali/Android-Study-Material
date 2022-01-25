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
    // doing calculations
}
```

### Coroutine Builder

Coroutine builder is bridge between non-suspending and suspending world.

Coroutine builders can run 