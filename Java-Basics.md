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

### Abstract Class & Methods

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

### Interfaces



<br/>

### Final Classes $ Methods

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

##### Coupling:

Coupling refers to the dependency between the classes.

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

### Object Comparison:

In java `==` used to compare referential equality of the object and `.equals()` used to compare structural equality of
the object.

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

