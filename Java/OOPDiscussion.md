# Dot Notation  

Dot notation works similarly in Java and JavaScript where it is used to access fields and method.

The following code sample demonstrates an `Animal` class. The class has two fields `name` (String) and `friend` (Animal). The constructor function takes one argument, `name`. `Animal` also has a method `speak` which prints out the string "WOOF?"

`Application` contains the main method for our application where `Animal` is instantiated as `myAnimal`. Then, `myAnimal.speak()` gets called and `myAnimal.name` gets printed.

```java
class Animal {

    String name;
    Animal friend;

    public Animal (String name) {
        this.name = name;
    }

    public void speak () {
        System.out.println("WOOF?");
    }
}

public class Application{
    public static void main (String[] args) {
        
        //instantiate Animal
        Animal myAnimal = new Animal("Fred");
        
        //Accessing member method
        myAnimal.speak();

        //Accessing member variable
        System.out.println(myAnimal.name); 
    }
}
``` 

## Accessing Fields and Methods of Nested Objects  

The example above shows us using dot notation to access methods and variables of the `Animal` class. Dot notation can also be "nested." For example, to retrieve the name of our "friend," we would use `myAnimal.friend.name`.

```java
class Animal {
    String name;
    Animal friend;
    public Animal (String name) {
        this.name = name;
    }
    public void speak () {
        System.out.println("WOOF?");
    }
}

public class Application{
    public static void main (String[] args) {
        //Instantiate Animal
        Animal myAnimal = new Animal("Fred");
        // assign a new Animal to the
        // friend property of myAnimal
        myAnimal.friend = new Animal("Betty");
        //Accessing member method
        myAnimal.friend.speak();
        //Accessing member field
        System.out.println(myAnimal.friend.name); 
    }
}
```

## Adding Fields and Methods to Instances

Unlike in JavaScript, Java does *not allow* us to define new fields "on the fly." Only the fields defined in the class are available.

In the following example, the `main` method tries to print the value of `myAnimal.friend`. No value has been assigned to the `friend` field so `null` is returned. Next, it tries to print `myAnimal.friend.name`. `null` doesn't have a `name` field and so a `NullPointerException` is thrown.

```java
class Animal {
    String name;
    Animal friend;
    public Animal (String name) {
        this.name = name;
    }
    public void speak () {
        System.out.println("WOOF?");
    }
}

public class Application{
    public static void main (String[] args) {
        // Instantiate Animal
        Animal myAnimal = new Animal("Fred");
        // Accessing member variable
        // Returns null. The field exists, but has no value
        System.out.println(myAnimal.friend); 
        // Accessing member variable
        // friend is null and doesn't have a name property
        // ERROR!! NullPointerException
        System.out.println(myAnimal.friend.name); 
    }
}
```

## Conclusion  

Dot notation is very similar in most languages. In Java, you use the `.` operator to drill down into fields and methods (and nested objects). Fields that have been defined in the class, but given no value will return `null`. Attempting to access fields that don't exist will result in a `NullPointerException`.

---

# Reason for Object Oriented Programming

Most people will refer to four basic tenets of Object Oriented Programming: Encapsulation, Abstraction, Inheritance, and Polymorphism. This lesson will discuss those concepts in general terms.

## Without Object Oriented Programming

To understand why Object Oriented Programming is a useful approach, let's consider an example without it. Let's say we're writing a program to simulate a Car Garage. The user could have multiple cars with different properties that we need to represent: the car's speed, the brand (i.e. "Honda"), and whether it is "on" or not.

```java
public class NoObjectOrientedStuff {
    int firstCarSpeed;
    int secondCarSpeed;
    int thirdCarSpeed;
    String firstCarBrand;
    String secondCarBrand;
    String thirdCarBrand;
    boolean firstCarOn;
    boolean secondCarOn;
    boolean thirdCarOn;
    //This ^^ is already looking cumbersome and doesn't scale with the number
    //Of cars. We could use an array instead:
    int[] carSpeeds;
    String[] carBrands;
    boolean[] carOn;
    //Now if we want to know the first car's speed:
    carSpeeds[0];
    //Set the second car's speed to 5
    carSpeeds[1] = 5;
}
```

Problems:

* There's nothing to prevent someone from setting a car's speed to a bad value. `carSpeeds[0] = -1000;` for example

* What if we want to define behaviors for each car? For example, we want to write a function to print out all the information about each car:

```java
public static void printInfo (int index) {
    System.out.println("Speed: " + carSpeeds[index]);
    System.out.println("Brand": + carBrands[index]);
    System.out.println("On: " + carOn[index]);
}
```

* What if the index is out of the range of the array? (In Java, this will crash your program if you don't expect it)

The bigger problem is that there's no real association between these fields except in the mind of the programmer. We could have meant to change Car 2's speed but accidentally changed Car 3's instead. We'd like to be able to bundle up the information and behaviors of a car into one thing. This desire to package related information leads us to the concept of Encapsulation

## Encapsulation

Object Oriented Programming provides us with the tools to bundle up our various fields into a class (as we have discussed previously):

```java
public class Car {
    int speed;
    String brand;
    boolean isOn;
}
```

What we've done is to package up these three fields (`int Speed`, `String brand`, `boolean isOn`) into a class `Car`. This class also provides us with the opportunity to define shared behaviors of all cars.

In this case, we want the ability to print some information about a car.

```java
public class Car {
    int speed;
    String brand;
    boolean isOn;
    public void printInfo () {
        System.out.println("Speed: " + speed);
        System.out.println("Brand: " + brand);
        System.out.println("On: " + isOn);
    }
}
```

As the author of the `Car` class, I can now make it available for other people to use. They can create `new Car()` and access `printInfo()`. Even though there is not a lot going on with `Car`, someone using the class does not need to understand its inner workings. Similarly, Java provides us with a `Scanner` to easily read from files. You as the programmer making use of `Scanner` do not need to know how the `Scanner` works. It's effectively a "black box" - it requires some inputs (such as the name of the file you want to scan) and outputs (the data from that file). We don't need to concern ourselves with **how** it gets that data.

## Inheritance

In programming we try to stay DRY: Don't Repeat Yourself. Let's imagine we wanted our car garage above to have different types of cars with different default behaviors. Perhaps we have a `SportsCar` that will accelerate much more quickly or a `Minivan` that is slower. Without inheritance, we would need to build each of these classes from scratch with their own `speed`, `brand`, and `isOn`, as well as shared behaviors like accelerate(), brake(), and printInfo().

Inheritance allows new classes to take on the properties and behaviors of existing classes. Any class that inherits behaviors from a parent class is called a "subclass."

In the following example, the SportsCar class is "extending" the Car class. SportsCar inherits all the fields and methods of Car. Any new fields or methods in SportsCar are only for SportsCar. Here is what our subclass looks like:

```java
public class SportsCar extends Car {
    //Fields specific to a SportsCar only:
    boolean isConvertible;
    //Methods specific to SportsCar
    public void accelerate () {
        speed += 5; //<- We still have access to speed though
    }
}
public class MiniVan extends Car {
    //Fields specific to a MiniVan only:
    boolean hasSlidingDoors;
    //Methods specific to MiniVan
    public void accelerate () {
        speed += 1; //<- We still have access to speed though
    }
}
```

Without inheritance, we'd have to define all the fields of Car as well. This violates the DRY principle.

```java
public class SportsCar {
    int speed;
    String type;
    boolean isOn;
    boolean isConvertible;
    public void accelerate () {
        speed += 5;
    }
    public void printInfo () {
        System.out.println("Speed: " + speed);
        System.out.println("Type: " + type);
        System.out.println("On: " + isOn);
    }
}
public class MiniVan {
    int speed;
    String type;
    boolean isOn;
    boolean hasSlidingDoors;
    public void accelerate () {
        speed += 1;
    }
    public void printInfo () {
        System.out.println("Speed: " + speed);
        System.out.println("Type: " + type);
        System.out.println("On: " + isOn);
    }
}
```

## Conclusion

Inheritance allows us to save time and define shared behaviors. We can then work with our subclasses knowing that they all have these core `Car` behaviors in common. Later lectures will cover this concept in more detail.
