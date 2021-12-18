# Broadcast Receivers

Broadcast receiver allows you to send or receive Android system or application events.

For example, applications can register for various system events like boot complete or battery low, and Android system
sends broadcast when specific event occur.

There are four types of Broadcasts:

1. Normal Broadcasts
2. Ordered Broadcasts
3. Sticky Broadcasts
4. Local Broadcasts

There are two ways to register Broadcast Receivers:

1. Statically (Using manifest.xml)
2. Dynamically (Using Kotlin/Java Code)

---

### Normal Broadcast

Normal broadcasts are **unordered and asynchronous**. The broadcasts don't have any priority and follow a random order.
You can run all the broadcasts together at once or run each of them randomly. These broadcasts are sent by using
the `Context:sendBroadcast`.

<br />

**Registering Broadcast in manifest.xml :**

```xml

<receiver android:name=".MyReceiver"/>
```

<br />

**Broadcast Receiver as a separate class :**

```kotlin
class MyReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context?, intent: Intent?) {
        Toast.makeText(context, "Inside broadcast onReceive() method", Toast.LENGTH_LONG).show()
    }
}
```

**Broadcast Receiver as inner class :**

```kotlin
companion object {
    class MyReceiver : BroadcastReceiver() {
        private val TAG = MyInnerReceiver::class.java.simpleName

        override fun onReceive(context: Context?, intent: Intent?) {
            Log.wtf(TAG, "onReceive: Inner class")
        }
    }
}
```

<br />

> **Note:** Inner Broadcast Receiver class must be a static class and
> we should use $ symbol to define this class inside `manifest.xml` as follows

```xml

<receiver android:name=".MainActivity$Companion$MyReceiver"/>
```

<br />

**Send Broadcast:**

```kotlin
Intent(this, MyReceiver::class.java).also {
    sendBroadcast(it)
}
```

<br />

**Send Broadcast explicitly using action name:**

We can also send Broadcast using the custom action name as follows:

```xml

<receiver android:name=".MyReceiver"
          android:exported="false">
    <intent-filter>
        <action android:name="my.custom.action"/>
    </intent-filter>
</receiver>
```

> **Note:** From Android 8 (API 26) you cannot use the manifest to declare a receiver for implicit broadcasts(with `action:name`). In the above solution
> we won't receive any Broadcast in `onReceive()` method. Also, To receive the broadcast using the
> "action name" we need to register broadcast and use the explicit `Intent` and set action to it as follows:

```kotlin
class MainActivity : AppCompatActivity() {
    private val myReceiver: MyReceiver by lazy { MyReceiver() }
    private val myReceiver1: MyInnerReceiver by lazy { MyInnerReceiver() }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // register both the broadcast to listen for the event
        registerReceiver(myReceiver, IntentFilter("my.custom.action"))
        registerReceiver(myReceiver1, IntentFilter("my.custom.action"))

        btnSendBroadcast.setOnClickListener {
            sendBroadcast(Intent("action.hello")) // sending broadcasts
        }
    }
}
```

<br/>

**Communication between Activity and Broadcast Receiver to send and receive data :**

We can send data from Activity to Broadcast Receiver using `intent.putExtra()` or `Bundle` object.

<br/>

**Sending data from Activity to Broadcast Receiver :**

```kotlin
Intent(this, MyReceiver::class.java).also {
    it.putExtra("key", "Hello World!")
    sendBroadcast(it)
}
```

**Receiving data from Activity to Broadcast Receiver :**

```kotlin
class MyReceiver : BroadcastReceiver() {

    private val TAG = MyReceiver::class.java.simpleName

    override fun onReceive(context: Context?, intent: Intent?) {
        val data = intent?.getStringExtra("key")
        Log.wtf(TAG, "onReceive: data : $data")
    }
}
```

<br/>

> **Note**: Broadcasts are sent **Asynchronously** and receive inside `onReceive()`
> method which executed on the Main thread. As of Android 12, `android:exported` must be set; use true to make the activity available to other apps, and false otherwise.

<br/>

**Broadcast Receiver Real World Example:**

Whenever any system event occurred, android system notifies (Send Broadcast) to all apps which are registered to listen
that event. These events are like, Wi-Fi on/off, Flight-mode on/off, SMS receive, call receive/disconnect etc.

In android, there are already defined predefined actions which can be registered in manifest.xml `action:name`.

> **Note:** From Android 8, there are restrictions for the system defined broadcasts.
> For using system defined broadcasts, we need to register these broadcasts from
> the `kotlin/java` code. manifest declared broadcast won't work anymore.

```kotlin
class MainActivity : AppCompatActivity() {
    private val myReceiver: MyReceiver by lazy { MyReceiver() }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        registerReceiver(myReceiver, IntentFilter(Intent.ACTION_AIRPLANE_MODE_CHANGED))
    }
}
```

<br/>

**Sending Broadcasts to the multiple receiver with same action name:**

```kotlin
class MainActivity : AppCompatActivity() {

    private val myReceiver: MyReceiver by lazy { MyReceiver() }
    private val myInnerReceiver: MyInnerReceiver by lazy { MyInnerReceiver() }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        IntentFilter("my.custom.action").also { intentFilter ->
            registerReceiver(myReceiver, intentFilter)
        }

        IntentFilter("my.custom.action").also { intentFilter ->
            registerReceiver(myInnerReceiver, intentFilter)
        }

        binding.btnSendBroadcast.setOnClickListener {
            sendBroadcast(Intent("my.custom.action"))
        }
    }
}
```

<br/>

**Ordered Broadcasts:**

Ordered Broadcasts are sent to the receivers in order based on priority set in Intent.

For Ordered Broadcasts, we need to use `sendOrderedBroadcast()` method.

```kotlin
class MyReceiver : BroadcastReceiver() {

    private val TAG = MyReceiver::class.java.simpleName

    override fun onReceive(context: Context?, intent: Intent?) {
        Log.wtf(TAG, "onReceive: changed")
    }
}
```

```kotlin
class MainActivity : AppCompatActivity() {

    private val myReceiver: MyReceiver by lazy { MyReceiver() }
    private val myInnerReceiver: MyInnerReceiver by lazy { MyInnerReceiver() }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        IntentFilter("my.custom.action").also { intentFilter ->
            intentFilter.priority = 40
            registerReceiver(myReceiver, intentFilter)
        }

        IntentFilter("my.custom.action").also { intentFilter ->
            intentFilter.priority = 50
            registerReceiver(myInnerReceiver, intentFilter)
        }

        binding.btnSendBroadcast.setOnClickListener {
            sendOrderedBroadcast(Intent("my.custom.action"), null) // here null is the permission string
        }
    }

    companion object {
        class MyInnerReceiver : BroadcastReceiver() {
            private val TAG = MyInnerReceiver::class.java.simpleName

            override fun onReceive(context: Context?, intent: Intent?) {
                Log.wtf(TAG, "onReceive: Inner class")
            }
        }
    }
}
```

> In the above example, `MyInnerReceiver` will receive broadcast first due to its high priority
> than `MyReceiver`.







