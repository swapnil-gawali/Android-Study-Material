### Class:

Class is the blueprint or template to create objects.

```java
public class User {
    String name;

    public User(String name) {
        this.name = name;
    }
}
```

<br/>

### Object:

Objects are instance of the class. Each object has an identity, a behavior and a state. The state of an object is stored
in fields (variables), while methods (functions) display the object's behavior.

```java
public class Main {

    public static void main(String[] args) {
        User user = new User("Swapnil Gawali");
    }
}
```

> In Java, `this` keyword is used to reference current object or class.

<br/>

### Object Comparison:

In java `==` used to compare referential equality of the object and `.equals()` used to compare structural equality of
the object.

<br/>

### Memory Allocation:

There are two memory available in java to store objects as well as variables.

1. Heap : is used to store objects
2. Stack : is used to store variables, short-lived variables and object references

**Example:**

```java
User user=new User();
```

In the above example, `new User()` will get stored in the heap memory and user variable will get stored inside the stack
memory.

> Garbage collector will automatically remove object from heap as well as variables from the stack whenever method/class
> finishes its execution.

<br/>

### Constructors:

In Java, a constructor is a block of codes similar to the method. It is called when an instance of the class is created.
At the time of calling constructor, memory for the object is allocated in the memory.

It is a special type of method which is used to initialize the object.

Every time an object is created using the new() keyword, at least one constructor is called.

It calls a default constructor if there is no constructor available in the class. In such case, Java compiler provides a
default constructor by default.

There are two types of constructors in Java: no-arg constructor, and parameterized constructor.

> Note: It is called constructor because it constructs the values at the time of object creation. It is not necessary to write a constructor for a class. It is because java compiler creates a default constructor if your class doesn't have any.
> Constructor name and class name must be same.

```java
public class Employee {
    private int baseSalary;
    private int hourlyRate;
    private int extraHours;

    // default empty constructor, if not defined, JVM will provide one for free
    public Employee() {
    }

    // Constructor with arguments
    public Employee(int baseSalary, int hourlyRate, int extraHours) {
        this.baseSalary = baseSalary;
        this.hourlyRate = hourlyRate;
        this.extraHours = extraHours;
    }
}
```

```java
public class Main {

    public static void main(String[] args) {
        // this will create Employee object
        // and initialize variables in the class with
        // default values
        Employee employee = new Employee();

        // this will create Employee object and initialize
        // variables at the time of object creation        
        Employee employee1 = new Employee(50000, 20, 10);
    }
}
```

<br/>

#### Constructor Overloading:

Constructor overloading in Java is a technique of having more than one constructor with different parameter lists.

```java
public class Employee {
    private final int baseSalary;
    private int hourlyRate;
    private int extraHours;

    // single parameter constructor
    public Employee(int baseSalary) {
        this.baseSalary = baseSalary;
    }

    // multiple parameter constructor
    public Employee(int baseSalary, int hourlyRate, int extraHours) {
        this(baseSalary); // Call to another constructor
        this.hourlyRate = hourlyRate;
        this.extraHours = extraHours;
    }
}
```

<br/>

### Static Variables & Methods:

In OOP, class can have two types of members, static and instance. Instance members belongs to the instances or objects,
and they can be accessed using object name.

When a member is declared static, it can be accessed before any objects of its class are created, and without reference
to any object.

```java
public class Calculator {

    public static double PI = 3.14;

    static int add(int a, int b) {
        return a + b;
    }
}
```

```java
public class Main {

    public static void main(String[] args) {
        System.out.println(Calculator.PI);

        int addNumbers = Calculator.add(10, 20);
        System.out.println(addNumbers);
    }
}
```

> In java, `main()` method is always declared as static, because it enables java runtime to call this method
> without creating an object.

<br/>

### Inheritance

Inheritance is a mechanism in which child class acquires all the properties and behaviours (variables and methods) of
the parent class.

```java
public class Parent {

    String someRandomString = "Hello This is some string";

    void printClassName(String className) {
        System.out.println("In parent class: " + className);
    }
}
```

```java
public class Child extends Parent {

    @Override
    void printClassName(String className) {
        // this method is overridden from the parent class
        super.printClassName(className); // this will call super class method

        System.out.println("In child class: " + className);
    }

    void accessParentClassVariable() {
        System.out.println(someRandomString); // someRandomString is declared inside Parent Class
    }
}
```

> `private` variables and methods cannot be inherited, and they cannot be accessible from the outside of class.

> `protected` variables and methods can be accessed within same package name or child class with different package name.

> `public` variables and methods can be accessed from anywhere.

> variables and methods without access modifiers called as `package-private` which can only be accessed within same package.

<br/>

### Object Class

Any class we declare will automatically inherit from the `Object` class. `Object` is the special class declared in the
`java.lang` package which allow us to work with additional methods like `equals()`, `hashCode()`, `toString()` methods.

<br/>

### Method Overriding

When we want to change the implementation of the parent class method then we override this method using `@override`
annotation.

```java
public class Child extends Parent {

    @Override
    void printClassName(String className) {
        // this method is overridden from the parent class
    }
}
```

> overridden method must have same signature and return type as declared in parent class. `@override` annotation is used
> to check this compatibility.

<br/>

### Upcasting and Downcasting:

Upcasting is casting an object to one of its parent type

Downcasting is casting object to one of its child type.

```java
public class Main {

    public static void main(String[] args) {
        Child child = new Child();
        printHashCode(child);
    }

    private static void printHashCode(Parent parent) { // this is Upcasting
        if (parent instanceof Child) {
            Child child = (Child) parent; // this is Downcasting
        }
    }
}
```

> `instanceof` keyword used to check whether the object is child of object or not.

<br/>

### Encapsulation:

Encapsulation is a mechanism of wrapping variables and methods together as a single unit. In encapsulation, the
variables of a class will be hidden from other classes, and can be accessed only through the methods of their current
class.

```java
public class Employee {
    private int baseSalary;
    private int hourlyRate;
    private int extraHours;

    public int getBaseSalary() {
        return baseSalary;
    }

    public void setBaseSalary(int baseSalary) {
        this.baseSalary = baseSalary;
    }

    public int getHourlyRate() {
        return hourlyRate;
    }

    public void setHourlyRate(int hourlyRate) {
        this.hourlyRate = hourlyRate;
    }

    public int getExtraHours() {
        return extraHours;
    }

    public void setExtraHours(int extraHours) {
        this.extraHours = extraHours;
    }

    int calculateWage() {
        return this.baseSalary + (this.extraHours * this.hourlyRate);
    }
}
```

```java
public class Main {

    public static void main(String[] args) {
        Employee employee = new Employee();
        employee.setBaseSalary(50000);
        employee.setHourlyRate(20);
        employee.setExtraHours(10);
        System.out.println("Salary is " + employee.calculateWage());
    }
}
```

In the above example, we have encapsulated the employee related variables and methods inside the Employee class as a
single unit, and it can be only accessed using the `Employee` object using getters and setters.

<br/>

### Abstraction:

Abstraction is a process of hiding the implementation details and showing only functionality to the user and this
reduces complexity by hiding unnecessary details.

In Java, Abstraction is the hiding implementation details of the class and expose only necessary methods/variables
outside.

Another way, it shows only essential things to the user and hides the internal details, for example, sending SMS where
you type the text and send the message. You don't know the internal processing about the message delivery.

Abstraction lets you focus on what the object does instead of how it does it.

#### Ways to achieve Abstraction

There are two ways to achieve abstraction in java

1. Abstract class (0 to 100%)
2. Interface (100%)

In java, abstraction is achieved by `interfaces` and `abstract` classes. We can achieve 100% abstraction
using `interfaces`.

<br/>

#### Abstract Class & Methods

A class which is declared as abstract is known as an abstract class. It can have abstract and non-abstract methods. It
needs to be extended and its method implemented. It cannot be instantiated, or we cannot create object of abstract
class.

**Points to Remember :**

1. An abstract class must be declared with an abstract keyword.
2. It can have abstract and non-abstract methods.
3. It cannot be instantiated.
4. It can have constructors and static methods also.
5. It can have final methods which will force the subclass not to change the body of the method.
6. Abstract methods do not have body, its child class responsibility to extend this class and override abstract method.

```java
public abstract class Bike {

    abstract void run();
}
```

```java
public class Splendor extends Bike {

    // overriding abstract method from parent class
    @Override
    void run() {
        System.out.println("Splendor is running");
    }
}
```

<br/>

#### Interfaces

An interface is a blueprint of a class. It has static constants and abstract methods. It cannot be instantiated just
like the abstract class.

Since Java 8, we can have default and static methods in an interface.

Since Java 9, we can have private methods in an interface.

**Why use Java interface?**

There are mainly three reasons to use interface. They are given below.

1. It is used to achieve abstraction.
2. By interface, we can support the functionality of multiple inheritance.
3. It can be used to achieve loose coupling.

<br/>

#### Declaring Interface

An interface is declared by using the `interface` keyword. It provides total abstraction; means all the methods in an
interface are declared with the empty body, and all the fields are public, static and final by default. A class that
implements an interface must implement all the methods declared in the interface.

```java
public interface MyInterface {
    double PI = 3.14;

    void doSomething();
}
```

<br/>

#### Interface Method Overloading

Just like classes, interfaces can also declare multiple methods with same name but different signature.

```java
public interface MyInterface {
    void doSomething();

    void doSomething(String key);
}
```

<br/>

#### Interface Inheritance

An interface can extend another interface using `extends` keyword.

```java
public interface MyBaseInterface {
    void baseMethod();
}

public interface MyInterface extends MyBaseInterface {
    void doSomething();
}
```

<br/>

### Final Classes & Methods

When we declare any class as final, then we cannot extend this class, but we can create object of final class.

When we declare any method as final, we cannot override it. This prevents child class to accidentally override method
from parent class.

```java
public final class Parent {
}
```

```java
public class Parent {

    // we cannot override following method inside of child class.
    final void printSomething() {
        System.out.println("Hello from Parent class.");
    }
}
```

<br/>

### Method Overloading:

If a class has multiple methods having same name but different in parameters, it is known as Method Overloading.

```java
public class Printer {

    private void print(String message) {
        System.out.println(message);
    }

    private void print(String message, String secondMessage) {
        System.out.println(message + " " + secondMessage);
    }
}
```

In the above example, both method names are same but the method parameters are different.

<br/>

### Polymorphism:

Polymorphism in Java is a concept by which we can perform a single action in different ways. Polymorphism is derived
from 2 Greek words: poly and morphs. The word "poly" means many and "morphs" means forms. So polymorphism means many
forms.

There are two types of polymorphism in Java: compile-time polymorphism and runtime polymorphism. We can perform
polymorphism in java by method overloading and method overriding.

If you overload a static method in Java, it is the example of compile time polymorphism.

```java
public class Bike {

    void run() {
        System.out.println("Bike is running");
    }
}
```

```java
public class Splendor extends Bike {

    @Override
    void run() {
        System.out.println("Splendor is running");
    }
}
```

```java
public class Activa extends Bike {
    @Override
    void run() {
        System.out.println("Activa is running");
    }
}
```

```java
public class Main {

    public static void main(String[] args) {
        Bike[] bikes = {new Splendor(), new Activa()};

        for (Bike b : bikes) {
            b.run(); // child class method calls at runtime, and it's a polymorphism
        }
    }
}
```

In the above example, we have created `Bike`, `Splendor` and `Activa` class. Splendor and Activa both are child of Bike
class and overrides `run()` method. We are calling the `run()` method by the reference variable of Parent class. Since
it refers to the child object and child method overrides the Parent class method, the child class method is invoked at
runtime.

<br/>

## Exceptions

---

### Handling Exceptions

The Exception Handling is one of the powerful mechanism to handle the runtime errors so that the normal flow of the
application can be maintained.

#### StackTrace

Stacktrace is error/crash output printed on console, and it contains information of exception along with line number
where exception is occurred in reverse order.

#### Types of Exception

In java there are three types of exceptions:

1. Checked
2. Unchecked or Runtime
3. Error

![](images\hierarchy-of-exception-handling.png)

<br/>

#### Checked Exceptions

Checked Exceptions are the exceptions where developer is responsible to handle properly. Checked exceptions are checked
at compile-time.

For e.g., we are reading data from file, there might be case the file is not exist, or it gets deleted or corrupted. In
this case it is developers responsibility to handle those exceptions. In this scenario, java compiler will enforce us to
handle these exceptions that is why it is called checked exceptions.

```java
public class Main {
    public static void main(String[] args) {
        try {
            FileReader fileReader = new FileReader("test.txt");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

In the above example, java compiler knows that `FileReader` class constructor `throws FileNotFoundException`, so the
compiler enforce us to handle this exception.

<br/>

#### Unchecked Exceptions Or Runtime Exceptions

These exceptions are not checked at the compile-time, and they appear at the runtime because of the programming mistake
or error.

```java
public class Main {
    public static void main(String[] args) {
        Main.sayHello(null);
    }

    public static void sayHello(String name) {
        System.out.println(name.toUpperCase());
    }
}
```

In the above example, we are trying to uppercase `null` value which is a programming error and the `sayHello` method
will throw exception when it will receive null value.

> Unchecked or Runtime Exceptions can be prevented using good coding practices and testing.

**Examples of Unchecked or Runtime Exceptions:**

1. NullPointerException
2. ArithmeticException
3. ArrayIndexOutOfBoundsException
4. NumberFormatException
5. IllegalStateException

<br/>

#### Error

This Error is external to the application and optimizing code for better performance.

**Examples of Error:**

1. StackOverflowError
2. VirtualMachineError
3. OutOfMemoryError+

<br/>

#### `try` `catch` & `finally`

`try`: The `try` keyword is used to specify a block where we code can throw an exception. It means we can't use try
block alone. The try block must be followed by either catch or finally.

`catch`: The `catch` block is used to handle the exception. It must be preceded by try block which means we can't use
catch block alone. It can be followed by finally block later.

`finally`: The `finally` block is used to execute the necessary code of the program. It is executed whether an exception
is handled or not.

```java
public class Main {
    public static void main(String[] args) {
        FileReader fileReader = null;
        try {
            fileReader = new FileReader("test.txt"); // exception will be thrown here

            // in case of exception, the following code won't be executed
            System.out.println("Reading file");
        } catch (FileNotFoundException e) {
            // in case of exception, these block will be executed
            e.printStackTrace();
        } finally {
            // this block will execute whether there is exception occurred or not
            fileReader.close();
        }
    }
}
```

<br/>

#### try-with-resource

This is enhanced version of the `try`, `catch` and `finally`. In this version, we do not need to explicitly
declare `finally`
block and java compiler will be responsible to release the resources.

```java
public class Main {
    public static void main(String[] args) {
        try (FileReader fileReader = new FileReader("test.txt")) {
            int data = fileReader.read();
            System.out.println("Reading file " + data);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

<br/>

### Throwing Exceptions

#### `throw` & `throws`

1. `throw`: The `throw` keyword is used to throw an exception.
2. `throws`: It states that there might be chance that calling method throws an exception. Caller method will be
   responsible to handle that exception. It is always used with method signature.

```java
public class Main {
    public static void main(String[] args) {
        Main.validateAge(15);
    }

    public static void validateAge(int age) {
        if (age < 18) {
            throw new IllegalArgumentException();
        }
    }
}
```

In the above example, we are throwing unchecked or runtime exception, in case user has provided wrong age. To throw
checked exception manually, we must `throws` it from the method signature, in this case caller will be responsible to
handle that exception.

```java
public class Main {
    public static void main(String[] args) {
        try {
            Main.validateAge(15);
        } catch (IOException e) {
            System.out.println("invalid age");
        }
    }

    public static void validateAge(int age) throws IOException {
        if (age < 18) {
            throw new IOException();
        }
    }
}
```

<br/>

## Generics

Before generics, we can store any type of objects in the collection, i.e., non-generic. Now generics force the java
programmer to store a specific type of objects. It makes the code stable by detecting the bugs at compile time.

#### Advantage of Java Generics:

1. **Type-safety:** We can hold only a single type of objects in generics. It doesn't allow storing other objects.

```java
public class Main {
    public static void main(String[] args) {
        // without generics
        List list = new ArrayList();
        list.add(10);
        list.add("10");

        // With Generics,it is required to specify the type of object we need to store.
        List<Integer> list2 = new ArrayList<Integer>();
        list2.add(10);
        list2.add("10");// compile-time error  
    }
}
```

2. **Type casting is not required:** There is no need to typecast the object.

```java
public class Main {
    public static void main(String[] args) {
        // without generics
        List list1 = new ArrayList();
        list1.add("hello");
        String s1 = (String) list1.get(0); //typecasting    

        //After Generics, we don't need to typecast the object.  
        List<String> list2 = new ArrayList<String>();
        list2.add("hello");
        String s2 = list2.get(0);
    }
}
```

3. **Compile-Time Checking:** It is checked at compile time so problem will not occur at runtime. The good programming
   strategy says it is far better to handle the problem at compile time than runtime.

```java
public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        list.add("hello");
        list.add(32); //Compile Time Error    
    }
}
```

<br/>

### Generic Class

**Example:**

`CustomGenericClass.java`

```java
public class CustomGenericClass<T> {
    private final List<T> list = new ArrayList<>();

    public void addValue(T value) {
        list.add(value);
    }

    public T getValue(int index) {
        return list.get(index);
    }
}
```

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        CustomGenericClass<Integer> test = new CustomGenericClass<>();
        test.addValue(10);
        test.addValue(20);
        test.addValue(30);

        test.addValue("10"); // compile time error, only accepting integers

        // getting value
        System.out.println(test.getValue(1)); // 20
    }
}
```

<br/>

### Generic Methods

```java
public class Main {
    public static void main(String[] args) {
        Integer[] intArray = {10, 20, 30, 40, 50};
        printArray(intArray);
    }

    public static <E> void printArray(E[] elements) {
        for (E element : elements) {
            System.out.println(element);
        }
    }
}
```

<br/>

### Multiple Type Parameters

**Method Example:**

```java
public class Main {
    public static void main(String[] args) {
        printKeyValue("name", "Swapnil Gawali");
        printKeyValue("Age", 25);
    }

    public static <K, V> void printKeyValue(K key, V value) {
        System.out.println("Key : " + key + " Value: " + value);
    }
}
```

**Class Example:**

```java
public class CustomKeyValue<K, V> {
    private final K key;
    private final V value;

    public CustomKeyValue(K key, V value) {
        this.key = key;
        this.value = value;
    }
}
```

<br/>

### Boxing & Unboxing

1. **Boxing:** The automatic conversion of primitive data types into its equivalent Wrapper type is known as boxing.

```java
public class Main {
    public static void main(String[] args) {
        int a = 50;
        Integer b = new Integer(a); // Boxing
    }
}
```

2. **Unboxing:** The automatic conversion of object data types into its equivalent primitive type is known as Unboxing.

```java
public class Main {
    public static void main(String[] args) {
        Integer a = new Integer(50);
        int b = a; // Unboxing
    }
}
```

<br/>

## Collection

The Collection in Java is a framework that provides an architecture to store and manipulate the group of objects.

Java Collections can achieve all the operations that you perform on a data such as searching, sorting, insertion,
manipulation, and deletion.

> In simple words, Collection means a single unit of objects.

<br/>

### Collection Hierarchy

![Collection Hierarchy](images\java-collection-hierarchy.png)

<br/>

### Iterable Interface

The Iterable interface is the root interface for all the collection classes which allows us to iterate over items in
collections.

#### Methods of Iterator interface

There are only three methods in the Iterator interface. They are:

| Method                   | Description                                                                  |
|--------------------------|------------------------------------------------------------------------------|
| public boolean hasNext() | It returns true if the iterator has more elements otherwise it returns false |
| public Object next()     | It returns the element and moves the cursor pointer to the next element.     |
| public void remove()     | It removes the last element returned by the iterator. It is less used.       |

<br/>

### Collection Interface

The Collection interface is the interface which is implemented by all the classes in the collection framework. It
declares the methods that every collection will have. In other words, we can say that the Collection interface builds
the foundation on which the collection framework depends.

Some methods of Collection interface are `Boolean add ( Object obj)`, `Boolean addAll ( Collection c)`, `void clear()`,
etc. which are implemented by all the subclasses of Collection interface.

<br/>

### List Interface

List interface is the child interface of Collection interface. It inhibits a list type data structure in which we can
store the ordered collection of objects. It can have duplicate values.

List interface is implemented by the classes `ArrayList`, `LinkedList`, `Vector`, and `Stack`.

To instantiate the List interface, we must use :

```java
List<data-type>list1=new ArrayList();
```

```java
List<data-type>list1=new LinkedList();
```

```java
List<data-type>list1=new Vector();
```

```java
List<data-type>list1=new Stack();
```





