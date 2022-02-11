### Variable Declaration:

In Kotlin, variable can be declared using `val` or `var` keywords.

`val` is immutable and `var` is mutable.

<br/>

### Checking Equality:

In java `==` used to compare referential equality of the object and `.equals()` used to compare structural equality of
the object. But, in Kotlin `==` is used to compare referential as well as structural equality of the object.

To check referential equality in Kotlin, you need to use `===` (triple equals).

<br/>

### Type Checking

In Java, we use `instanceof` operator to check type of ab object.

In Kotlin, if we want to check type of object we can use `is` operator.

```kotlin
fun main(args: Array<String>) {
    val age = 20

    checkType(age)
}

fun checkType(value: Any) {
    if (value is String) {
        println("it is string")
    } else {
        println("it is not string")
    }
}
```

```text
// output
it is not string
```

<br/>

### Casting

In Kotlin, we use `as` operator to cast

```kotlin
fun main(args: Array<String>) {
    val name = "Swapnil Gawali"

    doCasting(name)
}

fun doCasting(value: Any) {
    val stringValue = value as String
    // do something with stringValue
    // ...
}
```

If we are checking type first and then casting the object then we do not need to explicitly cast because kotlin supports
smart casting.

```kotlin
fun main(args: Array<String>) {
    val name = "Swapnil Gawali"

    doCasting(name)
}

fun doCasting(value: Any) {
    if (value is String) {
        // no cast needed, since its already know that tha value is String
        // in this case we can assume value is type of String and perform operations on it

        println(value.uppercase())
    }
}
```

<br/>

### String Template

We can use `$` for templating string.

```kotlin
fun main(args: Array<String>) {
    val name = "Swapnil Gawali"

    println("Welcome $name!")

    // OR

    println("Welcome ${name.uppercase()}!")
}
```

<br/>

### Raw Strings (Triple Quoted String)

We can use Triple Quoted String if we do not want to escape characters, instead we want string to behave as raw string
or device string into multiple line.

```kotlin
fun main(args: Array<String>) {
    val filePath = """C:\somedir\text.txt"""

    println(filePath)

    val multilineString = """This is a very big
        |Multiline string, which can take
        |upto 3 lines.
    """.trimMargin()

    println(multilineString)
}
```

```text
// output

C:\somedir\text.txt
This is a very big
Multiline string, which can take
upto 3 lines.
```

<br/>

### Type Conversion

Unlike java, kotlin does not support auto widening of numbers. For example, converting `int` to `float` ot `int`
to `long` etc.

For converting types, kotlin has `.to()` methods as follows:

```kotlin
fun main(args: Array<String>) {
    val myInt: Int = 10
    val myFloat: Float = 1.35F
    val myDouble: Double = 2.653

    val newDouble: Double = myInt.toDouble()
    val newFloat: Float = myInt.toFloat()
}
```

<br/>

### Any class

In java, `Object` is a superclass of all the classes. But in Kotlin, `Any` is the superclass of all the classes.

Both `Object` and `Any` class contain `toString()`, `equals()` and `hashCode()` methods.

<br/>

### Arrays

We can declare arrays in following ways:

```kotlin
fun main(args: Array<String>) {
    val intArray = arrayOf(1, 2, 3, 4) // or intArrayOf(1, 2, 3, 4) or Array<Int>(1, 2, 3, 4)

    // OR

    val intArray2 = IntArray(4)
    intArray2[0] = 1
    intArray2[1] = 2
    intArray2[2] = 3
    intArray2[3] = 4
}
```

<br/>

### Null Safety

Following are the Null Safety operators in kotlin:

1. Safe Call (`?.`)
2. Safe Call with let (`?.let{}`)
3. Elvis Operator (`?:`)
4. Non Null Assertion Operator (`!!`)

#### Safe Call (`?.`)

This operator will do the operation on the value if not null otherwise will return `null`. Use it if you don't mind
getting null values.

```kotlin
fun main(args: Array<String>) {
    val name: String? = "Swapnil Gawali"
    val address: String? = null

    println("This will uppercase name: ${name?.uppercase()}")
    println("This will return null: ${address?.uppercase()}")
}
```

```text
// output
This will uppercase name: SWAPNIL GAWALI
This will return null: null
```

#### Safe Call with let (`?.let{}`)

This will execute the block only if value is not null.

```kotlin
fun main(args: Array<String>) {
    val name: String? = "Swapnil Gawali"
    val address: String? = null

    name?.let { str ->
        // will execute only if name is not null
        println(str.uppercase())
    }

    address?.let { str ->
        // will execute only if address is not null
        println(str.uppercase())
    }
}
```

```text
// Output
SWAPNIL GAWALI
```

#### Elvis Operator (`?:`)

This will assign the non-null value if the variable is null.

```kotlin
fun main(args: Array<String>) {
    val country: String? = "Nashik"

    // without elvis operator
    val lengthWithoutElvis = if (country != null) country.length else -1
    println("Without Elvis Operator : $lengthWithoutElvis")

    // With Elvis Operator
    val lengthWithElvis = country?.length ?: -1
    println("With Elvis Operator : $lengthWithElvis")
}
```

#### Non Null Assertion Operator (`!!`)

You can use it when you are sure the value is not null. it will throw `NullPointerException` if value found null.

```kotlin
fun main(args: Array<String>) {
    val name: String? = "Swapnil Gawali"
    val address: String? = null

    println("This will uppercase name: ${name?.uppercase()}")
    println("This will return null: ${address!!.uppercase()}") // this will throw NullPointerException
}
```

```text
// output
This will uppercase name: SWAPNIL GAWALI
Exception in thread "main" java.lang.NullPointerException
	at MainKt.main(Main.kt:9)
```

<br/>

### Ranges

Range is used to declare values based on range.

```kotlin
fun main(args: Array<String>) {
    val range1 = 1..5 // Or 1.rangeTo(5)
    // this range contains the number 1 2 3 4 5


    val range2 = 5 downTo 1 // Or 5.downTo(1)
    // this range contains the number 5 4 3 2 1

    val range3 = 5 downTo 1 step 2
    // this range contains the number 5, 3, 1
}
```

### If, When, and loops

#### If-else

```kotlin
fun main(args: Array<String>) {
    val name = "Swapnil"
    printName(name)
    println(getAge(name))
}

fun printName(name: String) {
    // normal if-else
    if (name == "Ravi") {
        println("Hello Ravi!")
    } else {
        println("You are unknown!")
    }
}

fun getAge(name: String): Int {
    // if-else as expression
    return if (name == "Swapnil") {
        // return age of ravi
        30
    } else {
        // unknown person, return 0
        0
    }
}
```

```text
// output
You are unknown!
30
```

#### When

When is replacement foe switch case in kotlin.

```kotlin
fun main(args: Array<String>) {
    val name = "Swapnil"
    printName(name)
}

fun printName(name: String) {
    // normal if-else
    when (name) {
        "Ravi" -> {
            println("Hello Ravi!")
        }
        "Swapnil" -> {
            println("Hello Swapnil!")
        }
        else -> {
            println("You are unknown!")
        }
    }
}
```

```text
// output
Hello Swapnil!
```

#### For and ForEach

```kotlin
fun main(args: Array<String>) {
    for (i in 1..5) {
        println(i)
        // output -> 1 2 3 4 5
    }
}
```

#### While

```kotlin
fun main(args: Array<String>) {
    var counter = 1

    while (counter <= 10) {
        println(counter)
        counter++
    }
}
```

#### Do-While

```kotlin
fun main(args: Array<String>) {
    var counter = 1

    do {
        println(counter)
        counter++
    } while (counter <= 5)
}
```

#### break

```kotlin
fun main(args: Array<String>) {
    val range = 1..5
    range.forEach { number ->
        println(number)
        if (number == 2) {
            return // break is not applicable here
        }
    }

    for (number in range) {
        if (number == 2) break
    }

    myForEach@ for (number in range) {
        if (number == 2) break@myForEach // named loop
    }
}
```

#### continue

```kotlin
fun main(args: Array<String>) {
    val range = 1..10

    // print even numbers
    for (number in range) {
        if (number % 2 == 0) continue

        println(number)
    }

    // print odd numbers
    myForEach@ for (number in range) {
        if (number % 2 == 1) continue@myForEach // named loop

        println(number)
    }
}
```

<br/>

### Extension Functions

Extension function adds new function to the classes and this function behaves like `static`.

```kotlin
fun main(args: Array<String>) {
    val name = "Swapnil Gawali"

    println(name.trimAndUppercase()) // this behaves like it is declared in String class
}

// extension function
fun String.trimAndUppercase(): String {
    return this.trim().uppercase()
}
```

<br/>

### Infix Functions

Infix functions can be member functions or extension functions. They must have single parameter and have prefixed
with `infix` keyword.

> All `infix` functions are extension functions.

```kotlin
fun main(args: Array<String>) {
    val add = 50 add 18
    println(add)
}

infix fun Int.add(value: Int): Int {
    return this + value
}
```

In the above example, infix function improves readability. Basically, in infix function, we do not need to use `.`
and `()` instead we can write it as plain english.

Kotlin standard library contains some infix functions like `for(i in 1..10)` etc.

<br/>

### Recursion

When a function calls itself then it is called recursion.

```kotlin
fun main(args: Array<String>) {
    printSomething()
}

fun printSomething() {
    println("hello")
    printSomething() // calling same function recursively
}
```

The above example will throw `StackOverflowError`. To avoid that error, kotlin has `tailrec` functions which calls
recursive functions optimized way.

<br/>

### `tailrec` function

`tailrec` functions are used to call recursive functions in optimized way and avoid `StackOverflowError`.

```kotlin
fun main(args: Array<String>) {
    printSomething()
}

tailrec fun printSomething() { // this is tailrec function
    println("hello")
    printSomething() // calling same function recursively
}
```

In the above example, `printSomething()` functions call itself infinite time without throwing `StackOverflowError`.

<br/>

### Class

We can define class in kotlin following ways:

```kotlin
class User {
    // ...
}
```

<br/>

### Primary Constructors

In kotlin, we can declare primary constructor in following ways

```kotlin
class User(name: String) { // this is primary constructor
}

// OR

class User constructor(name: String) { // this is primary constructor
}
```

#### Declaring property inside constructor

Using `var` or `val` we can declare property while initializing constructor as follows:

```kotlin
// we do not need to assign name property which we assign in java constructor
class User constructor(val name: String) { // this is primary constructor
}
```

#### `init` block

`init` block get executed when we create instance of the class. `init` block can be used to assign or execute some code
at the time of object initialization.

```kotlin
class User constructor(val name: String) { // this is primary constructor

    init {
        // this block will execute when we create User object
        println("init block executed")
    }
}
```

> `init` block is always part of primary constructor.

<br/>

### Secondary Constructors

In kotlin, primary constructor dont have its body instead we have init block. But secondary constructors has its own
body.

```kotlin
class User constructor(val name: String) { // this is primary constructor

    constructor(name: String, age: Int) : this(name) {
        // body of the constructor
    }
}
```

We cannot use `var` or `val` while declaring fields in secondary constructors, we have to initialize these variables
explicitly.

```kotlin
class User constructor(val name: String) { // this is primary constructor

    // secondary constructor
    constructor(name: String, age: Int) : this(name) { // must call primary or another secondary constructor
        // body of the constructor
    }

    // another secondary constructor
    constructor(name: String, age: Int, address: String) : this(
        name,
        age
    ) { // must call primary or another secondary constructor

    }
}
```

> When we declare secondary constructor, it is mandatory to call primary or another secondary constructor.

#### Declaring properties in secondary constructor

```kotlin
class User constructor(val name: String) { // this is primary constructor

    var age: Int = 0

    // secondary constructor
    constructor(name: String, age: Int) : this(name) { // must call primary or another secondary constructor
        // body of the constructor
        this.age = age
    }
}
```

In the above example, for we are calling primary constructor with the property `name` which will automatically assigned
because use of `val` keyword. But we need to explicitly assign `age` property.

<br/>

### Inheritance

By default, in kotlin all classes are `public` and `final` in nature. That's why we can't directly inherit from a class.

For inheritance kotlin has provided keyword `open`.

```kotlin
fun main(args: Array<String>) {
    val cat = Cat("White", 12)
    cat.meow()
    cat.eat() // will call Animal class method

    val dog = Dog("White", "Labra")
    dog.bark()
    dog.eat() // will call Animal class method
}

open class Animal(val color: String) { // to enable inheritance we must declare class as open

    fun eat() { // to enable method overriding we must declare method as open
        println("Animal eating")
    }
}

class Dog(color: String, val breed: String) : Animal(color) {

    fun bark() {
        println("Dog barking")
    }
}

class Cat(color: String, val age: Int) : Animal(color) {

    fun meow() {
        println("Cat meowing")
    }
}
```

<br/>

### Method Overriding

In kotlin, by default every function is final. To enable overriding, we need to mark method as `open`.

```kotlin
open class Animal(val color: String) {

    open fun eat() {
        println("Animal eating")
    }
}

class Cat(color: String, val age: Int) : Animal(color) {

    fun meow() {
        println("Cat meowing")
    }

    override fun eat() { // overridden method from parent class
        super.eat() // this will call Animal class eat() method
        println("Cat eating")
    }
}
```

<br/>

### Primary Constructor Inheritance

```kotlin
open class Animal(val color: String)

class Cat(color: String, val age: Int) : Animal(color) // calling primary constructor of parent class
```

<br/>

### Secondary Constructor Inheritance

To call secondary constructor of parent class from the secondary constructor of current class, we need to use `super`
keyword.

```kotlin
fun main(args: Array<String>) {
    val cat = Cat("White", 12)
    println(cat.color)
    println(cat.age)
}

open class Animal {
    var color: String = ""

    constructor(color: String) {
        this.color = color
    }
}

class Cat : Animal {
    var age: Int = 0

    constructor(color: String, age: Int) : super(color) { // calling secondary constructor of parent class
        this.age = age
    }
}
```

<br/>

### Access Modifiers

In kotlin, everything public by default.

#### Top Level Modifiers

Functions, properties, classes, objects, and interfaces can be declared at the "top-level" directly inside a package:

* If you don’t use a visibility modifier, `public` is used by default, which means that your declarations will be
  visible everywhere.

* If you mark a declaration as `private`, it will only be visible inside the file that contains the declaration.

* If you mark it as `internal`, it will be visible everywhere in the same module.

* The `protected` modifier is not available for top-level declarations.

```kotlin
// file name: example.kt
package foo

private fun foo() {
    ...
} // visible inside example.kt

var bar: Int = 5 // property is visible everywhere
    private set         // setter is visible only in example.kt

internal val baz = 6    // visible inside the same module
```

#### Class Level Modifiers

For members declared inside a class:

* `private` means that the member is visible inside this class only (including all its members).

* `protected` means that the member has the same visibility as one marked as `private`, but that it is also visible in
  subclasses.

* `internal` means that any client inside this module who sees the declaring class sees its `internal` members.

* `public` means that any client who sees the declaring class sees its `public` members.

> In Kotlin, an outer class does not see private members of its inner classes.

```kotlin
open class Outer {
    private val a = 1
    protected open val b = 2
    internal open val c = 3
    val d = 4  // public by default

    protected class Nested {
        val e: Int = 5
    }
}

class Subclass : Outer() {
    // a is not visible
    // b, c and d are visible
    // Nested and e are visible

    override val b = 5   // 'b' is protected
    override val c = 7   // 'c' is internal
}

class Unrelated(o: Outer) {
    // o.a, o.b are not visible
    // o.c and o.d are visible (same module)
    // Outer.Nested is not visible, and Nested::e is not visible either
}
```

<br/>

### Abstract Classes, functions and Properties

In Kotlin, every abstract class, function and properties `open` by default i.e. they are available to override.

```kotlin
abstract class Animal {

    abstract var habit: String

    abstract fun eat()
}

class Cat(override var habit: String) : Animal() {
    override fun eat() {
        println("eating")
    }
}
```

> We cannot create object of abstract class

<br/>

### Interfaces

All the functions declared inside interface are `abstract` by default.

```kotlin
interface OnClickListener {
    fun onClick()
}

class Button : OnClickListener {
    override fun onClick() {
        println("Button is clicked")
    }
}
```

<br/>

### Data Classes

Data classes generally used for POJO model and they meant to hold data. In case of Data Classes, we do not need to
override `.equals()`, `hashcode()`, `toString()` methods since default behaviour is already provided to us.

Data classes also provides `copy()` method.

```kotlin
data class User(val name: String, val age: Int)
```

#### Data Classes Requirements:

* The primary constructor needs to have at least one parameter.

* All primary constructor parameters need to be marked as val or var.

* Data classes cannot be abstract, open, sealed, or inner.

#### Copying

Use the copy() function to copy an object, allowing you to alter some of its properties while keeping the rest
unchanged.

```kotlin
val jack = User(name = "Jack", age = 1)
val olderJack = jack.copy(age = 2)
```

In the above example, we are creating object of named `jack`, and later we are copying all the data from the `jack`,
reassigning `age`storing into another object.

<br/>

### `lateinit`

`lateinit` used to initialized property later instead of initializing at the time of declaring.

```kotlin
class User {
    lateinit var name: String // this variable will initialize later

    fun getName() {
        if (this::name.isInitialized) { // checking if this field is initialized or not
            println("name is initialized")
        } else {
            println("name is not initialized")
        }
    }
}
```

> `lateinit` only allowed on mutable properties i.e. `var`.

<br/>

### Singleton

Singleton has only one instance per whole application.

### Object Class

When we declare class as `object`, methods and variables declared inside it behaves as a static.

Only one instance of `object` class created at the runtime, hence it is widely used for creating singleton classes.

```kotlin
fun main(args: Array<String>) {
    val addition = Calculator.calculate(10, 20)
    println(addition)
}

object Calculator {
    val PI = 3.14

    fun calculate(a: Int, b: Int): Int {
        return a + b
    }
}
```

#### Points to remember:

* We cannot create instance of `object` class.

* `object` class cannot have constructor.

* `object` class also has parent class and inherit method from it.

<br/>

### Companion Object

Companion Objects are same as `object` but declared within a class. Methods and variables declared inside it behaves
like static.

```kotlin
fun main(args: Array<String>) {
    val fetchData = ApiService.doApiCall()
}

class ApiService {
    companion object {
        val API_URL = "https://google.com" // behaves like static variable

        fun doApiCall() {
            println("Calling $API_URL")
        }
    }
}
```

<br/>

### Constants

We can declare constant variables using `const` keyword.

`const` can only be allowed to declare inside `object` or `companion object`.

```kotlin
class Calculator {
    companion object {
        const val PI = 3.14
    }
}
```

```kotlin
object Calculator {
    const val PI = 3.14
}
```

<br/>

### Anonymous Inner Class

In Kotlin, we can declare anonymous inner class using `object` keyword.

```kotlin
interface OnClickListener {
    fun onClick()
}

fun main(args: Array<String>) {
    val onClickListener = object : OnClickListener {
        override fun onClick() {
            println("Clicked")
        }
    }

    onClickListener.onClick()
}
```

<br/>

### Enums

Enums in kotlin is similar to java.

```kotlin
enum class Colors {
    Blue,
    Black,
    Red,
    Orange,
    Green
}
```

<br/>

### Sealed Classes

Sealed Classes are used to maintain state.

Class extends sealed classes can have different object states.

```kotlin
sealed class Result {
    data class Success(val data: Data) : Result()
    data class Error(val exception: Exception) : Result()
}

fun main(args: Array<String>) {
    val result: Result = Result.Success("Data fetched successfully") // mock data from API

    val message = when (result) {
        is Result.Error -> result.exception.message
        is Result.Success -> result.data
    }

    println(message)
}
```

<br/>

### High Order Functions and Lambdas

#### High Order Functions

High Order Functions are the functions which accepts functions as a parameters and can also return a function.

<br/>

### Exception Handling

Kotlin's exception works similarly like Java but Kotlin do not have check exceptions. Every exception ios treated as
Unchecked or Runtime exception in Kotlin.

```kotlin
fun main(args: Array<String>) {
    var str = "100"
    var number: Int

    try {
        number = str.toInt()
    } catch (e: NumberFormatException) {
        println("Invalid number string")
    }
}
```

#### try as an expression

```kotlin
fun main(args: Array<String>) {
    var str = "100"

    var number = try {
        str.toInt()
    } catch (e: NumberFormatException) {
        -1 // if exception, then return -1
    }

    println(number)
}
```