## Kotlin KTX

Android KTX is a set of Kotlin extensions that reduces boilerplate code.

There are plenty of KTX available:

1. Collection KTX
2. Fragment KTX
3. Room KTX
4. Lifecycle KTX
5. Livedata KTX
6. ViewModel KTX
7. WorkManager KTX

<br/>

## Memory Leak

Failure to release unused objects from the memory means that there are unused objects in the application that the
Garbage Collector cannot clear from memory.

Let’s say we have method X. Method X is performing a task in the background that will take five minutes to complete and
we’ve given it an activity context to retrieve some string. We decided to close the activity after one minute, but the
method is still running and it needs four more minutes to complete the task; during this time we are holding the
activity object, which is no longer in use, and the GC will be unable to free it from the memory; once the method
completes the task, the GC will run again and will be able to clear and reclaim them from memory.

### Few major causes for memory leak

1. Unregistered Listeners
2. Unregistered Broadcast Receivers
3. Inner class reference
4. Passing ApplicationContext instead of ActivityContext
5. Background thread is still running while View is being destroyed
   <br/>

### Weak Reference

A weakly referenced object is cleared by the Garbage Collector when it's weakly reachable.

Weak reachability means that an object has neither strong nor soft references pointing to it.

<br/>

## Singleton Class

A singleton class is a class that is defined in such a way that only one instance of the class can be created and used
everywhere.

It is used where we need only one instance of the class like NetworkService, DatabaseService, etc.

Generally, it is done because it takes the resource of the system to create these objects again and again. So it is
better to create only once and use again and again the same object.

> In kotlin, `Object` classes are singleton by default.
<br/>

## Scope Functions in Kotlin (let, run also, apply, with)

Scope functions creates a temporary block inside which you can access the object as `it` or `this`

### `let`

`let` is used check null properties.

```kotlin
number?.let { it ->
    print(number)
}
```

in the above example, it will only execute that block if number is not null.

> We can access field in block using `it` and return last line from the block

<br/>

### `also`

`also` is similar to `let` but it does not return last line from the block instead it returns the properties itself.

```kotlin
val hello = number.also {
    print(it)
    number++
}
```

> We can access field in block using `it`

<br/>

### `apply`

`apply` used to apply or modify changes to the objects. This is very useful it gives access to `this` inside the block,
and we do not have
used `it.something` each time.

```kotlin
val intent = Intent().apply {
    // do something with the intent
    putExtra("", "")
}
```

> We can access field in block using `this`

<br/>

### `run`

`run` is similar to the `apply` but it does not returns object, instead it returns last statement from block.

```kotlin
val intent = Intent().run {
    putExtra("", "")
    action = ""
}
```

> We can access field in block using `this`

<br/>

### `with`

`with` is similar to the `run` but with different signature, and it is rarely usable, and it returns last statement in
the block.

```kotlin
val hello = with(Intent()) {

}
```

> We can access field in block using `this`

<br/>

### `takeIf`

returns the result if condition is `true` else return `null`

```kotlin
println("swapnil".takeIf { it == "it is swapnil" } ?: "Not a person!")
```

<br/>

### `takeUnless`

it is similar to the `takeIf` but in reversed. if condition is `true` then returns `null` else returns result.

```kotlin
 println("swapnil".takeUnless { it == "it is swapnil" } ?: "Not a person!")
```

<br/>

### Backing field

Backing field is represented by the `field` keyword, and it stores value of own property.

For e.g. when we create custom getter and setter in kotlin, for accessing own value of that property we need to use
backing field.

```kotlin
 var a: Int = 1
    get() {
        return field * 2
    }
    set(value) {
        field = value + 1
    }
```

<br/>

### Backing property

backing property is way to restrict modifying data in the class from another class.

For e.g. we have livedata object inside view-model, and we do not accidentally modify its data from outside the class,
then we can make that livedata to private and make public accessible backing property using getter which cant be
modified.

```kotlin
private val _name = MutableLiveData<String>()
val selectedItemList: LiveData<String> get() = _name
```
