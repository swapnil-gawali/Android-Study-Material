# Work Manager

Work Manager is used to schedule tasks based on specific constraints.

For e.g. Sync data with server with only if device is connected to internet Or show local notification only if device
battery is greater than 50% etc.

Work Manager can schedule on-time or repeating tasks. Work manager tasks executes, even if app is not running or device
rebooted.

<br/>

### Defining Work

To create some work for WorkManager to run, extend the `Worker` class and override the `doWork()` method. The `doWork()`
method runs asynchronously on a background thread provided by WorkManager.

```kotlin
class UploadWorker(context: Context, workerParameters: WorkerParameters) :
    Worker(context, workerParameters) {

    override fun doWork(): Result {
        // here we're running some background tasks
        uploadImageToServer()
        // Indicate whether the work finished successfully with the Result
        return Result.success()
    }
}
```

In the above example, the Result returned from `doWork()` informs the WorkManager service whether the work succeeded
and, in the case of failure, whether the work should be retried or not.

* `Result.success()`: The work finished successfully.
* `Result.failure()`: The work failed.
* `Result.retry()`: The work failed and should be tried at another time according to its retry policy.

<br/>

### Creating & Scheduling Task

We can schedule task to run periodically or one-time.

#### One-Time Request

For scheduling one-time request, we need to use `OneTimeWorkRequest` which is child class of `WorkRequest` class.

```kotlin
 val uploadWorkRequest = OneTimeWorkRequestBuilder<UploadWorker>()
    .build()
```

<br/>

#### Periodic(Repeating) Request

```kotlin
val saveRequest = PeriodicWorkRequestBuilder<SaveImageToFileWorker>(1, TimeUnit.HOURS)
    .build()
```

In this example, the work is scheduled with a one-hour interval.

<br/>

### Constraints

Constraints are the conditions set to the scheduled tasks. For example, you could add a constraint to your work request
such that the work only runs when the user’s device is charging. We need to use `Constraints.Builder()` to create
constraints.

```kotlin

val constraints = Constraints.Builder()
    .setRequiredNetworkType(NetworkType.UNMETERED)
    .setRequiresCharging(true)
    .build()

val myWorkRequest: WorkRequest =
    OneTimeWorkRequestBuilder<MyWork>()
        .setConstraints(constraints)
        .build()
```

In the above example, task will schedule if device is connected to internet.

When multiple constraints are specified, your work will run only when all the constraints are met.

<br/>

### Delayed Work

When we schedule a task, it runs immediately. If you do not want the task runs immediately, you can specify your work to
start after a minimum initial delay.

```kotlin
val myWorkRequest = OneTimeWorkRequestBuilder<MyWork>()
    .setInitialDelay(10, TimeUnit.MINUTES)
    .build()
```

<br/>

### Tag work

Every work request has a unique identifier, which can be used to identify that work later in order to cancel the work or
observe its progress.

```kotlin
val myWorkRequest = OneTimeWorkRequestBuilder<MyWork>()
    .addTag("cleanup")
    .build()
```

<br/>

### Setting Input Data

We can pass data to work manager while scheduling task.

We can set input data as follows:

```kotlin
val input: Data = workDataOf(
    "URL" to "https://www.google.com"
)

val uploadWorkRequest =
    OneTimeWorkRequestBuilder<UploadWorker>()
        .setInputData(input)
        .build()
```

In the `Worker` class, we can access the data using `inputdata` property.

```kotlin
val url = inputData.getString("URL")
```

<br/>

### Returning Result From Worker Class

After completion of work, we can set result in `Result` object.

```kotlin
// Set Result
val result: Data = workDataOf(KEY_RESULT to result)

return Result.success(result)
```

The returned value would be available in the task's `WorkInfo`:

```kotlin
WorkManager.getInstance(myContext).getWorkInfoByIdLiveData(uploadWorkRequest.id)
    .observe(this, Observer { info ->
        if (info != null && info.state.isFinished) {
            val myResult = info.outputData.getInt(
                KEY_RESULT,
                myDefaultValue
            )
            // ... do something with the result ...
        }
    })
```

<br/>

### Unique Work

Unique Work guarantees that you only have one instance of work with a particular name at a time.

**Usage:**

* `WorkManager.enqueueUniqueWork()` for one time work
* `WorkManager.enqueueUniquePeriodicWork()` for periodic work

```kotlin
WorkManager.getInstance(this).enqueueUniquePeriodicWork(
    "sendLogs",
    ExistingPeriodicWorkPolicy.KEEP,
    uploadWorkRequest
)
```

<br/>

### Cancelling and stopping work

If you no longer need your previously enqueued work to run, you can ask for it to be cancelled. Work can be cancelled by
its name, id or by a tag associated with it.

```kotlin
// by id
workManager.cancelWorkById(syncWorker.id)

// by name
workManager.cancelUniqueWork("sync")

// by tag
workManager.cancelAllWorkByTag("syncTag")
```

Under the hood, WorkManager checks the State of the work. If the work is already finished, nothing happens.