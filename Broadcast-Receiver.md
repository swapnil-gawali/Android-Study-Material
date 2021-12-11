# Broadcast Receivers

Broadcast receiver allows you to send or receive Android system or application events. nt which allows you to send or
receive Android system or application events.

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

> **Note:** In Broadcast Receiver there is `onReceive()` method which receives broadcasts

---

### Normal Broadcast

Normal broadcasts are **unordered and asynchronous**. The broadcasts don't have any priority and follow a random order.
You can run all the broadcasts together at once or run each of them randomly. These broadcasts are sent by using
the `Context:sendBroadcast`.

<br />

**Registering Broadcast in manifest.xml:**

```xml

<receiver android:name=".MyReceiver"/>
```

<br />

**Broadcast Receiver as a separate class**

```kotlin
class MyReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context?, intent: Intent?) {
        Toast.makeText(context, "Inside broadcast onReceive() method", Toast.LENGTH_LONG).show()
    }
}
```

**Broadcast Receiver as inner class**

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

```kotlin
Intent("my.custom.action").also {
    sendBroadcast(it)
}
```

> **Note:** From Android 8 (API 26) you cannot use the manifest to declare a receiver for implicit broadcasts. In the above solution
> we won't receive any Broadcast in `onReceive()` method. Also, To receive the broadcast using the
> "action name" we need to use the implicit `Intent` and set action to it as follows:

```kotlin
Intent(this, MyReceiver::class.java).setAction("my.custom.action").also {
    sendBroadcast(it)
}
```

<br/>

> **Note**: Broadcasts are sent Asynchronously and receive inside `OnReceive()`
> method which executed on the Main thread.






