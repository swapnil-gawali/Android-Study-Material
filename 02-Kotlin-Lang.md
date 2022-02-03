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





