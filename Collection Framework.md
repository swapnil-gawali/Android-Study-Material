# Collection Framework

Collection framework is set of classes and interfaces which used to store and structure our data.

![Collection Hierarchy](images/java-collection-hierarchy.png)

<br/>

### Advantages of Collection Framework

1. It reduces programming efforts
2. Provides inbuilt methods and classes to structure our data
3. Optimized for performance
4. Increases productivity
   <br/>

## ArrayList

### Difference between Array and ArrayList

Arrays are used to store fixed length of objects, on the other hand ArrayList can is growable and resizable.
<br/>

### Virtual Capacity of ArrayList

When we create an object of ArrayList, it creates default 10 virtual capacity.

We can change virtual capacity of ArrayList using:

```kotlin
val arr = ArrayList<String>(10)
```

> Note: When we try to access index position which is not exist in Array or ArrayList then it will
> throw `ArrayOutOfBoundsException`. For e.g. If Array/ArrayList has only 3 items, so it contains 0,1,2 indexes, but
> if we try to access index 3 then it will throw an exception.

| Method                                                 | Description                                                                                                                                                               |
|--------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| void add(int index, E element)                         | It is used to insert the specified element at the specified position in a list.                                                                                           |
| boolean add(E e)                                       | It is used to append the specified element at the end of a list.                                                                                                          |
| boolean addAll(Collection<? extends E> c)              | It is used to append all of the elements in the specified collection to the end of this list, in the order that they are returned by the specified collection's iterator. |
| boolean addAll(int index, Collection<? extends E> c)   | It is used to append all the elements in the specified collection, starting at the specified position of the list.                                                        |
| void clear()                                           | It is used to remove all of the elements from this list.                                                                                                                  |
| void ensureCapacity(int requiredCapacity)              | It is used to enhance the capacity of an ArrayList instance.                                                                                                              |
| E get(int index)                                       | It is used to fetch the element from the particular position of the list.                                                                                                 |
| boolean isEmpty()                                      | It returns true if the list is empty, otherwise false.                                                                                                                    |
| Iterator()                                             ||
| listIterator()                                         ||
| int lastIndexOf(Object o)                              | It is used to return the index in this list of the last occurrence of the specified element, or -1 if the list does not contain this element.                             |
| Object[] toArray()                                     | It is used to return an array containing all of the elements in this list in the correct order.                                                                           |
| <T> T[]toArray(T[] a)                                  | It is used to return an array containing all of the elements in this list in the correct order.                                                                           |
| Object clone()                                         | It is used to return a shallow copy of an ArrayList.                                                                                                                      |
| boolean contains(Object o)                             | It returns true if the list contains the specified element.                                                                                                               |
| int indexOf(Object o)                                  | It is used to return the index in this list of the first occurrence of the specified element, or -1 if the List does not contain this element.                            |
| E remove(int index)                                    | It is used to remove the element present at the specified position in the list.                                                                                           |
| boolean remove(Object o)                               | It is used to remove the first occurrence of the specified element.                                                                                                       |
| boolean removeAll(Collection<?> c)                     | It is used to remove all the elements from the list.                                                                                                                      |
| boolean removeIf(Predicate<? super E> filter)          | It is used to remove all the elements from the list that satisfies the given predicate.                                                                                   |
| protected void removeRange(int fromIndex, int toIndex) | It is used to remove all the elements lies within the given range.                                                                                                        |
| void replaceAll(UnaryOperator<E> operator)             | It is used to replace all the elements from the list with the specified element.                                                                                          |
| void retainAll(Collection<?> c)                        | It is used to retain all the elements in the list that are present in the specified collection.                                                                           |
| E set(int index, E element)                            | It is used to replace the specified element in the list, present at the specified position.                                                                               |
| void sort(Comparator<? super E> c)                     | It is used to sort the elements of the list on the basis of the specified comparator.                                                                                     |
| Spliterator<E> spliterator()                           | It is used to create a spliterator over the elements in a list.                                                                                                           |
| List<E> subList(int fromIndex, int toIndex)            | It is used to fetch all the elements that lies within the given range.                                                                                                    |
| int size()                                             | It is used to return the number of elements present in the list.                                                                                                          |
| void trimToSize()                                      | It is used to trim the capacity of this ArrayList instance to be the list's current size.                                                                                 |

<br/>

### Synchronized Arraylist

By default, ArrayList is non synchronized, for achieve synchronization we need to convert ArrayList into synchronized
version of it using `Collections.synchronizedList()` method, or we can use `CopyOnWriteArrayList`.

#### `CopyOnWriteArrayList`

```kotlin
val list = CopyOnWriteArrayList<Int>()
```

<br/>

### Interview Questions

#### Remove Duplicates from ArrayList

For eliminate duplicate elements from the ArrayList we need to convert that list into `LinkedHashSet`

```kotlin
val list = arrayListOf(9, 1, 2, 3, 6, 4, 8, 5, 4, 1, 5, 6, 7, 8, 9)
val linkedHashSet = LinkedHashSet(list)
val listWithoutDuplicates = ArrayList(linkedHashSet)
```

Or we can simplify it in Kotlin way using `distinct()` method:

```kotlin
val list = arrayListOf(9, 1, 2, 3, 6, 4, 8, 5, 4, 1, 5, 6, 7, 8, 9).distinct()
println(list)
```

<br/>

#### Check two lists are same or not

For compare two list are same or not, first we need to sort them:

```kotlin
fun main() {
    val l1 = arrayListOf("A", "B", "C")
    val l2 = arrayListOf("B", "A", "C")

    println(l1 == l2) // false

    l1.sort()
    l2.sort()

    println(l1 == l2) // true
}
```

<br/>

## Hashmap

Hashmap used to store values in `Key` and `Value` pair.

Hashmap do not store data in order and does not maintain indexing.

Hashmap can contain multiple null values but only one null key is allowed.

Hashmap is not thread safe.

```kotlin
val map = mapOf("One" to 1, "two" to 2, "Three" to 3, "five" to 5, "four" to 4)

map.forEach {
    println(it)
}
```

<br/>

### Hashtable vs SynchronizedMap vs ConcurrentHashMap

Hashtable and ConcurrentHashMap both are thread safe and do not allow null keys or value. On other hand, SynchronizedMap
allows null key and multiple null values.

The difference is Hashtable lock entire hashmap when multiple threads trying to access that hashmap. In other hand,
ConcurrentHashMap do not lock entire Hashmap, it only locks single segment of that hashmap, so the performance will be
good.

```kotlin
val ht = Hashtable<String?, String?>()
ht.put("null", "hello")
ht.put("one", null) // null pointer
```

```kotlin
val map = hashMapOf(1 to "one", 2 to "Two")

val synchronizedMap = Collections.synchronizedMap(map)

println(synchronizedMap[1])
```

```kotlin
val ch = ConcurrentHashMap<String?, String>()
ch.put("null", null) // null not allowed
```

<br/>

### Converting HashMap to ArrayList

```kotlin
val map = HashMap<Int, String>()
map.put(1, "a")
map.put(2, "b")
map.put(3, "c")
map.put(4, "d")
map.put(5, "e")

val keyList = ArrayList(map.map {
    "${it.key} = ${it.value}"
})

println("Key List: $keyList")
```

<br/>

### LinkedHashMap vs HashMap

Both has same characteristics excepts LinkedHashMap maintains insertion order.

Both can have one null key and multiple null values.

<br/>

### TreeMap

TreeMap sort its key by ascending and it implements `SortedMap` interface.

```kotlin
val map = TreeMap<Int, String>()
map.put(5000, "5000")
map.put(3500, "3500")
map.put(7000, "7000")
map.put(400, "400")
map.put(105, "105")
map.put(15000, "15000")

map.forEach {
    println("${it.key} = ${it.value}")
}

//op
105 = 105
400 = 400
3500 = 3500
5000 = 5000
7000 = 7000
15000 = 15000
```

In the above example, keys are automatically sorted by ascending.

> Note: We can store data in descending order by using `Comparator.reverseOrder()` in the map `Constructor`.

<br/>

## LinkedList






