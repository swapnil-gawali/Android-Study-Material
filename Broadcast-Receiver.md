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

    private lateinit var binding: ActivityMainBinding
    private val TAG = MainActivity::class.java.simpleName

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        IntentFilter("my.custom.action").also { intentFilter ->
            registerReceiver(firstReceiver, intentFilter)
        }

        IntentFilter("my.custom.action").also { intentFilter ->
            registerReceiver(secondReceiver, intentFilter)
        }

        binding.btnSendBroadcast.setOnClickListener {
            sendOrderedBroadcast(Intent("my.custom.action"), null) // here null is the string permission
        }
    }

    private val firstReceiver: BroadcastReceiver = object : BroadcastReceiver() {
        override fun onReceive(p0: Context?, p1: Intent?) {
            Log.wtf(TAG, "onReceive: first receiver")
        }
    }

    private val secondReceiver: BroadcastReceiver = object : BroadcastReceiver() {
        override fun onReceive(p0: Context?, p1: Intent?) {
            Log.wtf(TAG, "onReceive: second receiver")
        }
    }
}
```

<br/>

### Ordered Broadcasts:

Ordered Broadcasts are sent to the receivers in order based on priority set in Intent.

For Ordered Broadcasts, we need to use `sendOrderedBroadcast()` method.

```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding
    private val TAG = MainActivity::class.java.simpleName

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        IntentFilter("my.custom.action").also { intentFilter ->
            intentFilter.priority = 40
            registerReceiver(firstReceiver, intentFilter)
        }

        IntentFilter("my.custom.action").also { intentFilter ->
            intentFilter.priority = 50
            registerReceiver(secondReceiver, intentFilter)
        }

        binding.btnSendBroadcast.setOnClickListener {
            sendOrderedBroadcast(Intent("my.custom.action"), null) // here null is the string permission
        }
    }

    private val firstReceiver: BroadcastReceiver = object : BroadcastReceiver() {
        override fun onReceive(p0: Context?, p1: Intent?) {
            Log.wtf(TAG, "onReceive: first receiver")
        }
    }

    private val secondReceiver: BroadcastReceiver = object : BroadcastReceiver() {
        override fun onReceive(p0: Context?, p1: Intent?) {
            Log.wtf(TAG, "onReceive: second receiver")
        }
    }
}
```

> In the above example, `secondReceiver` will receive broadcast first due to its high priority
> than `firstReceiver`.

<br/>

**Send ResultCode and ResultData in Ordered Broadcasts:**

In Ordered Broadcast Receiver, we can set ResultCode, Bundle, ResultData while sending broadcasts. When first receiver
receives the data, then it can modify it and forward to the next receiver.

```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding
    private val TAG = MainActivity::class.java.simpleName

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        IntentFilter("my.custom.action").also { intentFilter ->
            intentFilter.priority = 40
            registerReceiver(firstReceiver, intentFilter)
        }

        IntentFilter("my.custom.action").also { intentFilter ->
            intentFilter.priority = 50
            registerReceiver(secondReceiver, intentFilter)
        }

        binding.btnSendBroadcast.setOnClickListener {
            Intent("my.custom.action").also { intent ->
                val bundle = Bundle()
                bundle.putString("key", "Hello World!")
                sendOrderedBroadcast(intent, null, null, null, 100, "Initial Data", bundle)
            }
        }
    }

    private val firstReceiver: BroadcastReceiver = object : BroadcastReceiver() {
        override fun onReceive(p0: Context?, p1: Intent?) {
            Log.wtf(TAG, "onReceive: firstReceiver")

            if (isOrderedBroadcast) {
                val initialCode = resultCode
                val initialData = resultData
                val initialBundle = getResultExtras(true)
                val message = initialBundle.getString("key")

                Log.wtf(TAG, "onReceive: firstReceiver $initialCode")
                Log.wtf(TAG, "onReceive: firstReceiver $initialData")
                Log.wtf(TAG, "onReceive: firstReceiver $message")
            }
        }
    }

    private val secondReceiver: BroadcastReceiver = object : BroadcastReceiver() {
        override fun onReceive(p0: Context?, p1: Intent?) {
            Log.wtf(TAG, "onReceive: secondReceiver")

            // execute this if only this broadcast is ordered, otherwise functionality will break
            if (isOrderedBroadcast) {
                val initialCode = resultCode
                val initialData = resultData
                val initialBundle = getResultExtras(true)
                val message = initialBundle.getString("key")

                Log.wtf(TAG, "onReceive: secondReceiver $initialCode")
                Log.wtf(TAG, "onReceive: secondReceiver $initialData")
                Log.wtf(TAG, "onReceive: secondReceiver $message")

                // now modify the data and forward it to the firstReceiver
                val bundle = Bundle()
                bundle.putString("key", "Modified message from secondReceiver")
                setResult(200, "hello from secondReceiver", bundle)
            }
        }
    }
}
```

> In the above example, `secondReceiver` will execute first, then it will get the data,
> modify it and forward to the `firstReceiver`. After executing `firstReceiver`, the execution
> will stop. But, in case we want to send data to the last `ResultReceiver` we need to create a
> separate receiver that will execute at the end of all receiver.

```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding
    private val TAG = MainActivity::class.java.simpleName

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        IntentFilter("my.custom.action").also { intentFilter ->
            intentFilter.priority = 40
            registerReceiver(firstReceiver, intentFilter)
        }

        IntentFilter("my.custom.action").also { intentFilter ->
            intentFilter.priority = 50
            registerReceiver(secondReceiver, intentFilter)
        }

        // registering resultReceiver
        IntentFilter("my.custom.action").also { intentFilter ->
            registerReceiver(resultReceiver, intentFilter)
        }

        binding.btnSendBroadcast.setOnClickListener {
            Intent("my.custom.action").also { intent ->
                val bundle = Bundle()
                bundle.putString("key", "Hello World!")
                sendOrderedBroadcast(intent, null, resultReceiver, null, 100, "Initial Data", bundle)
            }
        }
    }

    private val firstReceiver: BroadcastReceiver = object : BroadcastReceiver() {
        override fun onReceive(p0: Context?, p1: Intent?) {
            Log.wtf(TAG, "onReceive: firstReceiver")

            if (isOrderedBroadcast) {
                val initialCode = resultCode
                val initialData = resultData
                val initialBundle = getResultExtras(true)
                val message = initialBundle.getString("key")

                Log.wtf(TAG, "onReceive: firstReceiver $initialCode")
                Log.wtf(TAG, "onReceive: firstReceiver $initialData")
                Log.wtf(TAG, "onReceive: firstReceiver $message")
            }
        }
    }

    private val secondReceiver: BroadcastReceiver = object : BroadcastReceiver() {
        override fun onReceive(p0: Context?, p1: Intent?) {
            Log.wtf(TAG, "onReceive: secondReceiver")

            // execute this if only this broadcast is ordered, otherwise functionality will break
            if (isOrderedBroadcast) {
                val initialCode = resultCode
                val initialData = resultData
                val initialBundle = getResultExtras(true)
                val message = initialBundle.getString("key")

                Log.wtf(TAG, "onReceive: secondReceiver $initialCode")
                Log.wtf(TAG, "onReceive: secondReceiver $initialData")
                Log.wtf(TAG, "onReceive: secondReceiver $message")

                // now modify the data and forward it to the firstReceiver
                val bundle = Bundle()
                bundle.putString("key", "Modified message from second receiver")
                setResult(200, "hello from second receiver", bundle)
            }
        }
    }

    private val resultReceiver = object : BroadcastReceiver() {
        override fun onReceive(p0: Context?, p1: Intent?) {
            // execute this if only this broadcast is ordered, otherwise functionality will break
            if (isOrderedBroadcast) {
                val initialCode = resultCode
                val initialData = resultData
                val initialBundle = getResultExtras(true)
                val message = initialBundle.getString("key")

                Log.wtf(TAG, "onReceive: resultReceiver $initialCode")
                Log.wtf(TAG, "onReceive: resultReceiver $initialData")
                Log.wtf(TAG, "onReceive: resultReceiver $message")
            }
        }
    }
}
```

> In the above example, we do not need to specify priority for the `resultReceiver`.
> `resultReceiver` are made to get execute at the end of the all ordered broadcast receivers.

<br/>

**Aborting Ordered Broadcast:**

If we do not wish to execute the next Ordered Broadcast Receivers then after executing first receiver we can abort the
broadcast using `abortBroadcast()` method. In this case, all Ordered Broadcasts are aborted.

```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding
    private val TAG = MainActivity::class.java.simpleName

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        IntentFilter("my.custom.action").also { intentFilter ->
            intentFilter.priority = 40
            registerReceiver(firstReceiver, intentFilter)
        }

        IntentFilter("my.custom.action").also { intentFilter ->
            intentFilter.priority = 50
            registerReceiver(secondReceiver, intentFilter)
        }

        IntentFilter("my.custom.action").also { intentFilter ->
            registerReceiver(resultReceiver, intentFilter)
        }

        binding.btnSendBroadcast.setOnClickListener {
            Intent("my.custom.action").also { intent ->
                val bundle = Bundle()
                bundle.putString("key", "Hello World!")
                sendOrderedBroadcast(
                    intent,
                    null,
                    resultReceiver,
                    null,
                    100,
                    "Initial Data",
                    bundle
                )
            }
        }
    }

    private val firstReceiver: BroadcastReceiver = object : BroadcastReceiver() {
        override fun onReceive(p0: Context?, p1: Intent?) {
            Log.wtf(TAG, "onReceive: firstReceiver")

            if (isOrderedBroadcast) {
                val initialCode = resultCode
                val initialData = resultData
                val initialBundle = getResultExtras(true)
                val message = initialBundle.getString("key")

                Log.wtf(TAG, "onReceive: firstReceiver $initialCode")
                Log.wtf(TAG, "onReceive: firstReceiver $initialData")
                Log.wtf(TAG, "onReceive: firstReceiver $message")
            }
        }
    }

    private val secondReceiver: BroadcastReceiver = object : BroadcastReceiver() {
        override fun onReceive(p0: Context?, p1: Intent?) {
            Log.wtf(TAG, "onReceive: secondReceiver")

            // execute this if only this broadcast is ordered, otherwise functionality will break
            if (isOrderedBroadcast) {
                val initialCode = resultCode
                val initialData = resultData
                val initialBundle = getResultExtras(true)
                val message = initialBundle.getString("key")

                Log.wtf(TAG, "onReceive: secondReceiver $initialCode")
                Log.wtf(TAG, "onReceive: secondReceiver $initialData")
                Log.wtf(TAG, "onReceive: secondReceiver $message")

                abortBroadcast() // aborting all next broadcasts
            }
        }
    }

    private val resultReceiver = object : BroadcastReceiver() {
        override fun onReceive(p0: Context?, p1: Intent?) {
            // execute this if only this broadcast is ordered, otherwise functionality will break
            if (isOrderedBroadcast) {
                val initialCode = resultCode
                val initialData = resultData
                val initialBundle = getResultExtras(true)
                val message = initialBundle.getString("key")

                Log.wtf(TAG, "onReceive: resultReceiver $initialCode")
                Log.wtf(TAG, "onReceive: resultReceiver $initialData")
                Log.wtf(TAG, "onReceive: resultReceiver $message")
            }
        }
    }
}
```

> In the above example, we have called `abortBroadcast()` method inside `secondBroadcast`. In this case,
> `firstReceiver` and `resultReceiver` will not get executed.

<br/>

**Register and Un-Register Broadcast Receiver:**

To avoid memory leaks, its necessary to un-register receiver. We usually registers a receiver in
`onResume()` method and un-register in `onPause()` method. If we forgot to un-register a receiver, then it will throw **
Leaked Intent Receiver** error.

```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding
    private val TAG = MainActivity::class.java.simpleName

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
    }

    override fun onResume() {
        super.onResume()
        // registering receiver to listen for the event
        IntentFilter(Intent.ACTION_AIRPLANE_MODE_CHANGED).also { intentFilter ->
            registerReceiver(myReceiver, intentFilter)
        }
    }

    override fun onPause() {
        // un-register receiver to avoid memory leaks
        unregisterReceiver(myReceiver)
        super.onPause()
    }

    private val myReceiver: BroadcastReceiver = object : BroadcastReceiver() {
        override fun onReceive(p0: Context?, p1: Intent?) {
            Log.wtf(TAG, "onReceive: myReceiver")
        }
    }
}
```

> **Note:** We should always use dynamic receiver (kotlin code) to listen to the events whenever app is running.
> manifest declared (static) broadcasts receives a result even when app is not running and hence it may
> leak memory.

<br/>

### Sticky Broadcast (Deprecated in Android 21 Lollipop):

Whenever we send a broadcast, it gets processed and dies. In Sticky Broadcast, even when app is not in foreground and
broadcast event is already occurred then it will also notify for the previous occurred events.

Sticky Broadcasts are only applicable for the dynamic (kotlin code) receivers. We can send Sticky Broadcast
using `sendStickyBroadcast()` method.

For using Sticky Broadcast, we need to define manifest
permission `<uses-permission android:name="android.permission.BROADCAST_STICKY" />`

> **Note:** Due to the poor security and poor protection level, Sticky Broadcasts are
> deprecate from the Android 21 (Lollipop).

<br/>

**Enhancing App Security using Broadcast Receivers:**

We can enhance Broadcast Receiver security by using `<permission>` or `<uses-permission>` tag in manifest. Also, we can
narrow down the Broadcast Receiver scope using `android:exported` property.

<br/>

**Send / Receive Broadcast from One app to Another:**

In the following example, we can register to receive a broadcast as follows:

```kotlin
IntentFilter("my.custom.action").also { intentFilter ->
    registerReceiver(myReceiver, intentFilter)
}
```

And then, we can send the broadcast from the another app as follows:

```kotlin
Intent("my.custom.action").also { intent ->
    sendBroadcast(intent)
}
```

<br/>

### Local Broadcast Receiver:

Local Broadcast Receiver is used send broadcasts with the same application. It is useful for communication between two
android components like Service and Activity etc.














