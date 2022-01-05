### Big O Notation:

Big O used to describe the performance of the algorithm. It helps to describe that the given algorithm is going to scale
or not.

#### `O(1)`:

`O(1)` means that, no matter how much data, it will execute in constant time.

```kotlin
fun main(args: Array<String>) {
    val numberArray = arrayOf(1, 2, 3, 4, 5)

    // the following line will take constant time for execution
    println(numberArray[0]) // O(1)
}
```

<br/>

#### `O(n)`:

`O(n)` means that it is proportional to the amount of data. i.e. if there are 5 items in the input there will be 5
operation, if there are a million items in the input, there will be a million operations.

```kotlin
fun main(args: Array<String>) {
    val numberArray = arrayOf(1, 2, 3, 4, 5)

    println() // O(1)

    numberArray.forEach { number ->
        println(number) // O(n)
    }

    println() // O(1)
}
```

