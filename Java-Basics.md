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

Abstraction means reduce complexity by hiding unnecessary details.

In Java, Abstraction is the hiding implementation details of the class and expose only necessary methods/variables
outside.

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



