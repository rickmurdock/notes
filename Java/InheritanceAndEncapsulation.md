# Inheritance

In programming, "inheritance" is the practice of creating a class based on another class. The idea is as follows:

* Create a "base" class. This base class has fields and methods.

 * Create a "child" class that inherits all of the base class's fields and methods.

 * Differentiate the child class from the base class by adding additional fields and methods it.
 
To demonstrate this concept, let's talk through an analogy.

## Inheritance Example - Computers

All computers have some things in common. They have processors, memory, and storage. They can compute data and return values. All computers share these features (more or less).

However, some computers can *also do a lot more*. Some computers have a screen, and others do not. Laptops have touchpads while desktop computers do not. Laptop computers have a built-in webcam and microphone, while desktop computers do not.

If we think about modeling computers as classes with inheritance, we can think of the class `Computer` as the "base class". It will have all of the generic fields and methods shared by *all* computers. Then, we can define some classes that inherit from `Computer`. `Laptop` will inherit all the fields and methods from `Computer` and then we can add more methods and fields.

## Authoring a Class From a Super Class

Here's the Computer class:

```java
public class Computer {
    int ramInGigabytes;
    int storageInGigabytes;
    String brand;
    boolean isOn;

    public void turnOn() {
      isOn = true;
    }

    public void turnOff() {
      isOn = false;
    }
}
```

The `Laptop` class that `extends` the `Computer` class to inherit from it.

```java
public class Laptop extends Computer {

    // The Laptop Class will automatically inherit the
    // fields and methods of the Computer class
    // extends is the keyword that allows this to happen

    // I can now define fields and methods here that are unique to the Laptop class
    Touchpad myTouchpad;
    Webcam myWebCam;
    Microphone myMic;

    // I can also define methods that didn't exist on the default computer

    public void startWebCamSession() {
        // start the web cam

        // start recording
    }
}
```

More than one class can extend a base class. `Tablet` inherits all the fields and methods from `Computer`.

```java
public class Tablet extends Computer {

  TouchScreen myTouchScreen;
  Camera myCamera;
  FingerPrintSensor myFingerPrintSensor;

  public void unlockHomeScreen() {
    // scan fingerprint
  }
}
```

`Laptop` and `Tablet` both have everything `Computer` has and more. `Laptop` and `Tablet` extend the `Computer` class to inherit the properties `ramInGigabytes`, `storageInGigabytes`, `brand`, and `isOn`. They also inherits the methods `turnOn()` and `turnOff()`.

In their class definitions, they also add new properties and methods that are related to the specific types of computers they represent.

Using inheritance may seem insignificant, but imagine a more detailed Computer class with 100 properties and 70 methods. The time that would be saved by inheriting that big Computer class is well worth it.

* Formal Terminology:

  * `Computer` is the **superclass** of `Laptop` and `Tablet`

  * `Laptop` and `Tablet` are **subclasses** of `Computer`

* Less formally, people will say:

  * `Computer` is the **parent class** of `Laptop` and `Tablet`

  * `Laptop` and `Tablet` are **children** of `Computer`

# super Keyword

Just like the `this` keyword provides a reference to the current object, sometimes we need to reference our superclass. The `super` keyword is that reference. A common use of this keyword is to call the constructor of the superclass. In this case, the syntax is `super(arguments)`. The call to the super constructor must be the first line of the constructor. Take a look at the code below - in particular, the constructors - and run the code.

```java
class Animal {

    String name;
    int age;

    public Animal (String name, int age) {
        System.out.println("In the Animal constructor");
        this.name = name;
        this.age = age;
    }

    public String toString () {
        return "Name: " + name + "\nAge: " + age;
    }
}
class Lion extends Animal {

    String roar;

    public Lion (String roar, String name, int age) {
        super(name, age);
        System.out.println("In Lion constructor");
        this.roar = roar;
    }

    public void workOnRoar () {
        System.out.println(roar);
        System.out.println("Working on my roar!");
        roar += "!!!";
        System.out.println(roar);
    }

    public String toString () {
        return super.toString() + "\nRoar: " + roar;
    }
}

public class AnimalApplication{
    public static void main (String[] args) {
        Lion lion = new Lion ("Roarrr", "Alfred", 3);
        lion.workOnRoar();

        System.out.println(lion);
    }
}
```
  
The example above contains another use of `super`: to call the `toString()` method of `Animal` to print out the variables of that class.

## Conclusion

We've discussed the concept of **inheritance**, a key part of Object Oriented Programming. We can `extend` classes and inherit their data and behavior, which can save a lot of time in building objects. The superclass is extended by the subclass. Lion is the subclass of Animal, and Animal is the superclass of Lion. We also discussed the `super` keyword. Inheritance is a core concept of OOP, and you will be seeing a good amount of it moving forward.

---

# Access Modifiers - Public, Private, Protected

One of the important aspects of object oriented programming is the concept of controlling access to properties and methods. OOP languages accomplish this by adding an **access modifier** to the class, data, and methods. Three access modifiers provide different levels of access: `public`, `private`, and `protected`.

* `public` - accessible anywhere

* `private` - accessible from within the class

* `protected` - access is the same as `private` except it provides access to classes that inherit from this class.

* `(default)` - There's no "default" keyword, but if you don't use one of the above keywords to specify an access level, variables can be accessed by any files in the same package

It is good programming practice to have fields `private` or `protected`. To provide access to the data from outside the class, we create "getters" and "setters" for private fields. Getters are methods that return the value of a private field. Setters are methods that change the value of a private field.

## Access Modifiers Example - Banking Application

Assume we're creating a banking application. We'll start by creating the `Account` class.

```java
public class Account {
    //Private variables
    private int id;
    private String description;
    private double balance;
    //Getters and setters
    public int getId() {
        return this.id;
    }
    public String getDescription() {
        return this.description;
    }
    public void setDescription(String description) {
        this.description = description;
    }
    public double getBalance() {
        return this.balance;
    }
    protected void setBalance(double balance) {
        this.balance = balance;
    }
}
```

In our `Account` class, the three fields (data), `id`, `description`, and `balance` all have the `private` access modifiers and cannot be accessed outside of the `Account` class. An instance of `Account` won't provide access to those fields. For example, say I've instantiated `Account` as `myAccount`. `myAccount.id` will throw an error. Run the following code sample to see the error.

```java
class Account {

    //Private variables
    private int id;
    private String description;
    private double balance;

    //Getters and setters
    public int getId() {
        return this.id;
    }

    public String getDescription() {
        return this.description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public double getBalance() {
        return this.balance;
    }

    protected void setBalance(double balance) {
        this.balance = balance;
    }
}

public class BankingApplication{
    public static void main (String[] args) {
        Account myAccount = new Account();
        System.out.println(myAccount.id);
    }
}
```

Four of the five methods are marked `public` meaning they can be called from any class. The `setBalance()` method is marked `protected` meaning it can only be called from within the Account class and from any class that inherits Account.

It's also worth noting that we did not choose to include a `setId()` method at all. This is because (presumably) within the context of our program, an Account's ID does not need to change. Remember, when authoring a class, **you** decide what fields users of the class will need access to and how they can access them.

## Java Bean/POJO

A Java Bean, also sometimes referred to as a Plain Old Java Object ("POJO"). These terms refer to classes that meet the following qualifications:

* Must have a public default constructor that takes no arguments (may have additional constructors)

* Must have getters and setters for all variables which follow appropriate naming conventions (`int numTigers -> getNumTigers()`)

* Must be **serializable**. This is a term we'll discuss more later, but essentially it means that your class can be saved into a database or other format.
