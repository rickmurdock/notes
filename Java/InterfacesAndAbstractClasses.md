# Interfaces  
An interface is a special structure that defines a list of method signatures and is used to ensure that a collection of classes implement the same functions using the same signature.

Interfaces offer an alternative to class-based inheritance by providing a kind of contract that classes can share. In a moment, we'll revisit some of the examples we worked up in other lessons, but let's look at a simple example of class-based inheritance and then rework it to use an interface.

Interface Example - Calculator  
The following example provides a base Calculator class with two fields and one method. Two other classes then extend that class. When reading through this example, consider a few things:

The base class has a default implementation of the process method. You may want that in your application, but then again, you may not. Should a generic Calculator have a default process? Perhaps, not.

The fields defined in the base class are appropriately named for the base class implementation of the process method but are awkward in the other methods. If we are adding two numbers together, why are they called numerator and denominator? We could define new fields for Added and Multiplier, but those classes would still have the numerator and denominator fields and super would still assign values to those fields. Why have these unused fields on subclasses?

In this calculator scenario, is super really necessary? Why are we calling back to a base class if all we are doing is defining a common method between classes that process numbers? Is there a simpler and more direct way?

What if we forget to override process in a subclass? That could cause serious problems. For instance, what if we define a Subtractor class, but forget to override process? You might reasonably expect a class called Subtractor to subtract numbers, but this error will cause Subtractor to divide numbers because that's what the base class does. The compiler will not throw an error here because this behavior is normal for class-based inheritance.


1
// The Base Class
2
class Calculator{
3
    double numerator;
4
    double denominator;
5
​
6
    public Calculator( double num, double denom ){
7
        this.numerator   = num;
8
        this.denominator = denom;
9
    }
10
    public double process(){
11
        return numerator / denominator;
12
    }
13
}
14
​
15
class Adder extends Calculator{
16
    public Adder( double num1, double num2 ){
17
        super( num1, num2 );
18
    }
19
    public double process(){
20
        return numerator + denominator;
21
    }
22
}
23
​
24
class Multiplier extends Calculator{
25
    public Multiplier( double num1, double num2 ){
26
        super( num1, num2 );
27
    }
28
    public double process(){
29
        return numerator * denominator;
30
    }
31
}

Fullscreen

Reset Code
Run Code 
Let's rewrite this scenario using an interface. In the next example, Calculator as an interface, not a class. A few things to note as you read through the code:

An interface cannot be instantiated. We create Calculator objects because every class that implements Calculator IS-A Calculator, but we cannot call new Calculator.

super isn't required in the constructors.

Every class can define its fields. No base class is adding properties to our subclasses, and each class can set properties that make the most sense for that class.

The Calculator interface defines the signature for the process method, but no implementation. Instead of providing a default behavior, this signature acts as a contract between the interface and any classes that implement it. Any class that implements Calculator must implement a process class with the same signature. Interfaces are powerful tools for ensuring that a group of classes conform to a standard set of method signatures, making the code more easily understood and much more predictable. I know that every class in my example will have a process method and that the method will return a double. I know this because they all implement Calculator.

If a class fails to implement one of the method signatures, they an error is thrown. When you run this code, you will see the error. Errors are good! They tell you when things aren't working properly. To see the code run, uncomment the process method in the Subtractor class.


1
interface Calculator{
2
    public double process();
3
}
4
​
5
class Adder implements Calculator{
6
    
7
    double first;
8
    double second;
9
​
10
    public Adder( double first, double second ){
11
        this.first = first;
12
        this.second = second;
13
    }
14
    public double process(){
15
        return first + second;
16
    }
17
}
18
​
19
class Multiplier implements Calculator{
20
    
21
    double operand1;
22
    double operand2;
23
​
24
    public Multiplier( double operand1, double operand2 ){
25
        this.operand1 = operand1;
26
        this.operand2 = operand2;
27
    }
28
    public double process(){
29
        return operand1 * operand2;
30
    }
31
}

Fullscreen

Reset Code
Run Code 
Interface Example - Calculator  
In our Calculator example, we demonstrated a simple use case for an interface. Now, let's consider a more complex example and discuss some of the finer details about how and when to use an interface.

Consider our Animal example from before:

public class Animal {
    int hoursSinceEaten;
    int hoursSinceSlept;
    String animalType; //i.e. "Snake", "Horse", "Eagle", etc.
    boolean isAwake;
    double chanceToFindFood; //Number between 0 and 1
}
(We also considered examples of classes extending Animal, such as Cow, Dog, Lion, etc.). Here is an interface representing the shared behavior of "Living Creatures":

public interface LivingCreature {
    public void sleep();
    public void eat();
    public void findFood();
}
When writing an interface, don't define the method body for each method signature. If you try to define the functions, you will get an error: Interface methods cannot have a body. Remember, an interface is just a contract that says "Any class that implements this interface _must _have these methods".

Let's update the Animalclass to implement LivingCreature.

public class Animal implements LivingCreature {
    int hoursSinceEaten;
    int hoursSinceSlept;
    String animalType; //i.e. "Snake", "Horse", "Eagle", etc.
    boolean isAwake;
    double chanceToFindFood; //Number between 0 and 1

    //Added a new variable to help model "findFood"
    boolean hasFood;

    public void sleep() {
        //Now we get to define how these methods work
        isAwake = false;
        hoursSinceSlept
    }
    //It's completely up to us to decide how these methods work
    //We could decide that if we
    // don't have food we should findFood()
    public void eat() {
        if (hasFood) {
            hoursSinceEaten = 0;
            hasFood = false;
        }
        else {
            findFood();
        }
    }

    public void findFood() {
        double foodRoll = Math.random();
        if (foodRoll >= chanceToFindFood) {
            hasFood = true;
        }
    }
}
Again, it's completely up to the class implementing the interface to determine how the methods work. The interface only guarantees those methods will be present - it does not ensure they work as intended.
We cannot change the method signatures; so for example, if we wanted findFood() to return a boolean representing success, we would have to change that in the interface:
public interface LivingCreature {
    public boolean findFood();
}
Interface Defaults  
When defining an interface, you should be aware that the default access fields are different. Specifically, in an interface, methods are considered public unless you specify otherwise. They are also abstract in effect (the next lesson will dive into this keyword). It's also possible to have constants in an interface. For example, we could update the LivingCreature interface above to have a constant FOOD_CHANCE. It's still up to the class implementing the interface to fill in the function, of course.

Any constants defined in an interface are public static final by default. If you copy the following code into IntelliJ, you will see that the public static final keywords are grayed out because they aren't needed.

public interface LivingCreature {
    public static final double FOOD_CHANCE = 0.4;
    public boolean findFood();
}
Instantiation  
You cannot directly instantiate an interface:

LivingCreature myAnimal = new LivingCreature(); //<- Compiler error!
However, a class implementing an interface does have an IS-A relationship.

Animal myAnimal = new Animal();
So if Animal implements LivingCreature, myAnimal IS-A LivingCreature.

LivingCreature myAnimal = new Animal(); //<- Works
Implementing An Interface for Use in a Collection  
Let's say we have a class that represents Gummy Bears (ooh, tasty!).

public class GummyBear {
    private double sweetness;
    private int size;
}
We also have a List<GummyBear> that we want to sort based on size. To accomplish this, we will update our GummyBear class to implement the Comparable interface, which requires GummyBear tp define a compareTo() method. If we want our GummyBear to be comparable to anything, we could declare the class as such:

public class GummyBear implements Comparable {
    public int compareTo (Object otherObject) {

    }
}
We would then need to provide logic within the method to determine the type of otherObject. Instead, let's say that our GummyBear is only comparable to other GummyBears:

public class GummyBear implements Comparable<GummyBear> {

    public int compareTo (GummyBear otherBear) {
        if (this.size > otherBear.size) {
            return 1;
        } else if (this.size < otherBear.size) {
            return -1;
        } else {
            if (this.sweetness > otherBear.sweetness) {
                return 1;
            } else if (this.sweetness < otherBear.sweetness) {
                return -1;
            } else {
                return 0;
            }
        }
    }
}
Notice that we've used GummyBear as the parameterized type for the Comparable interface, which changes the signature (in particular the arguments) of the compareTo() method (we are now comparing to another GummyBear).

implements Comparable /*no type!*/ -> compareTo(Object other)
implements Comparable<GummyBear> -> compareTo(GummyBear otherBear)
Collections.sort()  
Now that we've done all the hard work of setting up our class to implement Comparable, sorting is going to be easy. Java provides several classes that contain static methods which can be useful for their respective areas. They include the Math class, which you've already encountered, as well as the Collections and Arrays classes (note the plural is part of the class name: Arrays does NOT represent an array - it's a class for working with arrays). The Collections class provides a simple way of sorting Comparables using the Collections.sort() method.


1
import java.util.ArrayList;
2
import java.util.Collections;
3
import java.util.List;
4
import java.util.Random;
5
​
6
public class SortBears {
7
​
8
    public static void main(String[] args) {
9
        List<GummyBear> gummyBears = new ArrayList<>();
10
        Random random = new Random();
11
        for (int i = 0; i < 10; i++) {
12
            double sweetness = random.nextDouble();
13
            int size = random.nextInt(5);
14
            gummyBears.add(new GummyBear(sweetness, size));
15
        }
16
        System.out.println("Unsorted List:");
17
        for (GummyBear gb : gummyBears) {
18
            System.out.println(gb);
19
        }
20
​
21
        Collections.sort(gummyBears);
22
        System.out.println("\n====\nSorted List:");
23
        for (GummyBear gb : gummyBears) {
24
            System.out.println(gb);
25
        }
26
    }
27
}
28
class GummyBear implements Comparable<GummyBear> {
29
​
30
    private double sweetness;
31
    private int size;

Fullscreen

Reset Code
Run Code 
Note that we can only use Collections.sort() here because our GummyBear class implements Comparable. There are many other ways to sort Collections, but in each method, we will have to tell Java how we want the sort to happen. For example, there is an alternative Collections.sort() method to which we can provide a Comparator object which knows how to compare things.

---

# Abstract Classes  
Imagine we are writing some software to model a zoo. We have a base class Animal that is extended by the Lion, Tiger, and Bear classes - oh my!

public class Animal {
  String name;
  int age;

  public void makeNoise () {
    System.out.println("???");
  }
}

public class Lion extends Animal {

  public void makeNoise () {
    System.out.println("Roar!");
  }
}

public class Bear extends Animal {

  public void makeNoise () {
    System.out.println("zzzz...."); //Bear is hibernating
  }
}
This is a useful object structure. Since all the animal classes in our zoo will extend Animal, we know they will all have a name, and age, and will be able to make noise. But it doesn't really make any sense to have an instance of the Animal class - all animals should have a type, and make their own noise (not "???"). The better solution in this case is to use an abstract base class.

Abstract classes allow us to create templates for other classes - just as our (non-abstract) Animal class did above. The difference is that abstract classes cannot be instantiated (created) directly. Abstract classes (or methods) are good to use when it wouldn't make any sense to have an instance of the class.

public abstract class Animal {
  String name;
  int age;

  public abstract void makeNoise();
}
Abstract classes cannot be directly instantiated; with the above example, Animal myAnimal = new Animal(); would result in a compile-time error. Within the class, we also have the abstract method makeNoise(), a method with no body which will need to be implemented (filled in) by all the non-abstract classes that extend Animal. Note that abstract classes can have non-abstract methods just like regular classes.

Here it makes sense for the makeNoise() method to be abstract, because every animal makes a different noise - there is no common "noise behavior" shared by all animals.

Spaceships - Wheee!  
Here we have an abstract class Spaceship. It has a few variables and three abstract methods (takeOff(), land(), and refuel(int)). These methods will have to be "filled in" by the non-abstract subclasses that extend this class.

public abstract class Spaceship {

    protected int fuelRemaining;
    protected boolean isDestroyed;
    protected boolean isFlying;

    public Spaceship (int fuelRemaining) {
        this.fuelRemaining = fuelRemaining;
        isDestroyed = false;
        isFlying = false;
    }

    //Here we will define three abstract methods.
    //Since the methods are abstract they have no body.
    //The extending class will have to decide exactly how these methods work
    public abstract void takeOff();

    public abstract void land();

    public abstract void refuel(int fuelAmount);

    //Accessor methods not shown
}
Now we're going to extend Spaceship into the non-abstract RicketySpaceship:

package abstract_example2;

public class RicketySpaceship extends Spaceship {


    public RicketySpaceship (int fuelRemaining) {
        super (fuelRemaining);
    }

    public void takeOff () {
        //The RicketySpaceship requires 10 fuel points to take off
        if (fuelRemaining > 10) {
            fuelRemaining -= 10;
            isFlying = true;
        } else {
            fuelRemaining = 0;
        }
    }

    public void land () {
        //Because this is a RicketySpaceship
        //there's a 10% chance when it lands it will explode! Yikes
        double roll = Math.random(); //A random number between 0.0 and 1.0
        if (roll < 0.1) {
            isDestroyed = true;
            fuelRemaining = 0;
            System.out.println("KABLOOOIE!");
        }
    }

    public void refuel (int fuelAmount) {
        fuelRemaining += fuelAmount;
    }
}

The RicketySpaceship is a little... dangerous. As you can see in the land() method, there is a 10% chance that the ship will explode when it lands. Not very safe, but presumably the passengers knew they were getting into a RicketySpaceship.

Next we have a different extension of the Spaceship:

public class GreenSpaceship extends Spaceship {

    public GreenSpaceship () {
        //Since the GreenSpaceship doesn't require fuel
        //we will pass 0 fuel to the super constructor
        super (0);
    }

    //The GreenSpaceship doesn't require fuel; it can take off and land for free
    public void takeOff () {
        isFlying = true;
    }

    public void land () {
        isFlying = false;
    }

    //However if you try to refuel it there is a
    //10% chance there will be a smugness freakout
    //The ship will be destroyed if that happens
    public void refuel (int fuelAmount) {
        double roll = Math.random(); //A random number between 0.0 and 1.0
        if (roll > 0.1) {
            System.out.println("I don't need dirty fossil fuels, I'm a GREEN spaceship");
        } else {
            System.out.println("AH!!!! SMUGNESS FREAKOUT!!!");
            System.out.println("KABLOOOIE!");
            isDestroyed = true;
        }
    }
}
Well, every method of transportation has its own associated risks. Just don't try to refuel() your GreenSpaceship and you'll be fine. Probably.

Conclusions  
Each of these subclasses (RicketySpaceship and GreenSpaceship) both extend Spaceship. They both implement the three abstract methods from Spaceship but in different ways. It's important to understand though that there are no restrictions on the behavior of these methods; just their signatures. In other words, in order to be a valid subclass of Spaceship, we have to have public void refuel (int). If we wanted to define public void refuel(double) in addition, that's fine, but it won't "count" as covering the original public abstract void refuel(int) from Spaceship.

The following class is technically legal (this code will compile just fine) but violates the spirit of the contract. This appears to be some kind of food ordering menu. Unless you're planning to enter a bad code writing competition, avoid doing things like this.

public class NotASpaceship extends Spaceship {

    public NotASpaceship () {
        super (-1);
    }

    public void land () {
        System.out.println("Would you like some ice cream?");
    }

    public void takeOff () {
        System.out.println("How about french fries?");
    }

    public void refuel (int fuelAmount) {
        switch (fuelAmount) {
            case 0:
                System.out.println("Hamburger?");
                break;
            case 1:
                System.out.println("Pizza?");
                break;
            case 2:
                System.out.println("Fried chicken?");
                break;
            default:
                System.out.println("Ham sandwich?");
        }
    }
}
Interfaces vs Abstract Classes  
When we have similar tools that do similar things (as in the case of interfaces and inheritance) it's important to understand the details of how they work so we can know which one to use.

Regular Inheritance:

A base class has some behavior and variables which can be inherited or overridden by a subclass
A subclass can only extend one base class
Both the base class and the subclass can be used by themselves and instantiated directly
A variable of type the base class can be initialized as an instance of type the subclass (polymorphism)
Interfaces:

An interface defines a set of expected behavior, but does not define how that behavior is implemented (it does have any actual functionality)
An interface cannot be instantiated directly
A class can implement as many interfaces as it wants
A variable of type the interface must be instantiated as an instance of type the implementation
Abstract Classes

An abstract class can have actual implementation code in it
But an abstract class cannot be instantiated
And an abstract class can only be extended through classic inheritance (meaning a subclass can only extend one abstract class)
