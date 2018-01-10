# Classes and Objects

Welcome to Object Oriented Programming in Java! This lecture will introduce you to the basic concepts of OOP in Java, highlighting some similarities and differences between Java and JS.

## What is a Class?  

When writing Java, you will model almost everything as a "Class." Classes act as "blueprints" for the creation of objects. With one blueprint, an architect can create many houses. Like the architect, you may need to create multiple objects with the same structure.

An architectural blueprint doesn't provide shelter or storage space; you don't live in a blueprint. Blueprints specify how to build a house. On the other hand, a house does provide shelter and storage space, but doesn't tell you how to create another house.

> Classes are to blueprints as objects are to houses.

Creating an object is similar to an architect using their blueprint to build a building. The building may have different attributes like color and address, but it follows the blueprint's guidelines.

You don't directly *use* classes in a program. Instead, you use the `new` keyword to create and use object based on your class. Objects created from classes are "instances, " and they carry the same fields and methods as the class.

You create an instance of an object using the `new` keyword. This process is called *instantiation*.

To build an instance of a class, we need a **constructor**. The purpose of the constructor is to "set up" the instance with everything it needs. Most often, the constructor will receive some initial variable values and assign them to the instance. In Java, the `this` keyword refers to the instance begin created, not the class.

Let's look at a couple of examples:

### Class Example - Map Coordinates  

Perhaps, you are writing a coordinate system for maps. You may create dozens (maybe thousands) of location objects, all with the same basic fields. Each coordinate probably needs `latitude`, `longitude` and `label` fields so that it can be placed on a map and then given a label. For example:

`28.39, -80.60, "Cape Canaveral"`

The following example has two classes. The first class is `Coordinate` and is used to create `Coordinate` instances. The second class is `MapApplication` and contains the `main()` function for our application. Instances of `Coordinate` are set up in `main()` when the application runs.

You can think of `MapApplication` as the program and `Coordinate` as a class that is being used by the program.

```java
// to define a class use the `class` keyword
// and give the class a name
// all of the class fields, methods
// and constructors go inside in between the curly braces
class Coordinate{

    // Class fields
    // Each instance will have these fields
    String latitude;
    String longitude;
    String label;

    // Class constructor function
    public Coordinate( String latitude, String longitude, String label ){

        // Assign values to the new instance fields
        this.latitude = latitude;
        this.longitude = longitude;
        this.label = label;
    }

    // Class method
    // This method is available to each instance
    public String toString(){
        return this.label + " is located at " + this.latitude + ", " + this.longitude + ".";
    }
}

// The application class
public class MapApplication{

    // The `main` function is the
    // entry point for the application
    // When Java runs this program
    // `main` is the first thing thats run
    public static void main(String[] args) {

        // Create two instances of Coordinate

        // A coordinate object for Columbia, SC
        Coordinate scCapital = new Coordinate( "34.0007° N", "81.0348° W", "Columbia, South Carolina" );
        // A coordinate object for Sacramento, CA
        Coordinate caCapital = new Coordinate( "38.5816° N", "121.4944° W", "Sacramento, California" );

        // Print the description for each instance
        System.out.println( scCapital.toString() );
        //If we don't call toString, the println function will call it for us
        System.out.println( caCapital );
    }
}
``` 

### Class Example - Backpacks  

This code sample also has two classes `Backpack` and `HikingApplication`. `HikingApplication` contains the `main()` function for our program and instantiates two Backpack objects `myBackpack` and `otherBackpack`. The methods `addItem()` and `removeItem()` both change the value of the `items` property in each object.

Finally, `main()` prints some information about both backpack instances.

```java
class Backpack {

    // let's give our Backpack class some fields
    String owner;
    String color;
    int items;

    // here's our constructor. Its arguments are owner, color, and items
    public Backpack(String owner, String color, int items) {

        // Assign values to the new instance fields
        this.owner = owner;
        this.color = color;
        this.items = items;
    }

    // a simple method to add an item to our backpack
    public void addItem() {
        items += 1;
    }

    // a simple method to remove an item to our backpack
    public void removeItem() {
        items -= 1;
    }
}

public class HikingApplication{

    public static void main(String[] args) {
        // we'll create an instance of Backpack and name it myBackpack
        // we are providing the arguments owner, color,
        // and items as we create this object
        Backpack myBackpack = new Backpack("Conor", "blue", 3);
        Backpack otherBackpack = new Backpack("Paul", "green", 2);

        // we'll call some methods on our objects
        myBackpack.addItem();
        otherBackpack.removeItem();

        // let's print a statement to let us know how many items myBackpack has
        System.out.println(myBackpack.owner + "'s backpack has " + myBackpack.items + " items inside.");
        System.out.println(otherBackpack.owner + "'s backpack has " + otherBackpack.items + " items inside.");
    }
}
```

## Conclusion  

Classes create a blueprint for new objects. Classes also define a **constructor** function for defining new instances of the class. These instances are objects that can be used in applications.

---

# Writing Classes in Java  

Now that we've discussed classes and objects, let's talk about how to turn a mental "concept" of something into a Java class. When defining classes, you'll have to specify three things:

1. Fields - Class variables attached to instances of the class. In other words, what are the properties of your class?

2. Constructor Functions - special functions that are used to instantiate the class. Java allows us to define more than one constructor per class so that objects can be instantiated in more than one way.

3. Methods - Functions available to and shared by each class instance.

## Classes Example - Modeling a House  

### Data - Fields  

First, consider the data that you will need. Keep in mind that you only need fields for things your program needs. For example, consider the House class below:

```java
public class House {

    String address;
    String architecturalStyle;
    String ownerName;

    int numBathrooms;
    int numBedrooms;
    int numWindows;
    int numDoors;
    int numOccupants;

    boolean isHouseBoat;
    boolean isMovable;
    boolean isCarpeted;
    boolean hasFireplace;
    boolean hasGarage;
    boolean hasJacuzzi;
}
```

Houses have thousands of little details about them. Even though we already have 14 variables, we've only covered a fraction of those details. What about the names of all the occupants, or the number of windows in each room?

When modeling a concept into a class, discard all of the things your program doesn't care about. Maybe the only fields we are really interested in are the address, owner's name, and the number of bedrooms and bathrooms:

```java
public class House {
    String address;
    String ownersName;
    int numBedrooms;
    int numBathrooms;
    boolean isLocked;
}
```

### Constructor  

For the House class, we'll define one constructor function.

```java
public class House {
    String address;
    String ownersName;
    int numBedrooms;
    int numBathrooms;
    boolean isLocked;

    public void House( String address, String ownersName, int bedrooms, int bathrooms ){

        this.address = address;
        this.ownersName = ownersName;
        this.bedrooms = bedrooms;
        this.bathrooms = bathrooms;
    }
}
```

### Behavior - Methods  

Finally, we define any methods that all house objects will need. These methods will usually be associated with the data. In this example, we'll define a method `lockDoors`.

```java
public class House {

    String address;
    String ownersName;
    int numBedrooms;
    int numBathrooms;
    boolean isLocked;

    public void House( String address, String ownersName, int bedrooms, int bathrooms ){

        this.address = address;
        this.ownersName = ownersName;
        this.bedrooms = bedrooms;
        this.bathrooms = bathrooms;
    }

    public boolean lockDoors(){
        isLocked = true;
        return isLocked;
    }
}
```

Note that it's perfectly OK to have a class that does not define any interesting behaviors. A "simple" class with no real behaviors is often referred to as a Java Bean or POJO - Plain Old Java Object (we'll revisit this concept).

## Classes Example - Modeling an Animal  

### Data - Fields  

First, define some data fields for `Animal`.

```java
public class Animal {

    int hoursSinceEaten;
    int hoursSinceSlept;
    String animalType; //i.e. "Snake", "Horse", "Eagle", etc.
    double chanceToFindFood; //Number between 0 and 1

    boolean isAwake;
}
```

## Constructor  

Next, we should think about what kind of constructor(s) we will need for our class. We'll define two constructors for the `Animal` class.

```java
public class Animal {

    int hoursSinceEaten;
    int hoursSinceSlept;
    String animalType; //i.e. "Snake", "Horse", "Eagle", etc.
    double chanceToFindFood; //Number between 0 and 1

    boolean isAwake;

    //Constructors for Animal
    public Animal (String animalType, double chanceToFindFood) {
        this.animalType = animalType;
        this.chanceToFindFood = chanceToFindFood;
        hoursSinceEaten = 0;
        hoursSinceSlept = 0;
        isAwake = true;
    }
    public Animal (String animalType, double chanceToFindFood, int hoursSinceEaten,
    int hoursSinceSlept, boolean isAwake) {
        this.animalType = animalType;
        this.chanceToFindFood = chanceToFindFood;
        this.hoursSinceEaten = hoursSinceEaten;
        this.hoursSinceSlept = hoursSinceSlept;
        this.isAwake = isAwake;
    }
}
```

Each of these constructors could be useful depending on the overall context of our program. Both constructors insist that we receive the animal's type and its chance to find food, since we can't infer "reasonable defaults" for these variables. In contrast, it's not unreasonable to say that a newly created Animal is "fresh" and basically just woke up from a nap (though maybe it should wake up hungry). But there is no way to infer or guess at the `animalType` variable.

### Behavior - Methods  

Finally, we want to consider the behaviors that our class needs to model. Let's consider a more active example.

Given those variables, we can imagine some behaviors we might want to model. Animals need to eat and sleep. These could be complicated tasks if our model is complex. Let's keep it simple for now.

```java
public class Animal {

    int hoursSinceEaten;
    int hoursSinceSlept;
    String animalType; //i.e. "Snake", "Horse", "Eagle", etc.
    double chanceToFindFood; //Number between 0 and 1

    boolean isAwake;

    //Constructors for Animal
    public Animal (String animalType, double chanceToFindFood) {
        this.animalType = animalType;
        this.chanceToFindFood = chanceToFindFood;
        hoursSinceEaten = 0;
        hoursSinceSlept = 0;
        isAwake = true;
    }
    public Animal (String animalType, double chanceToFindFood, int hoursSinceEaten,
    int hoursSinceSlept, boolean isAwake) {
        this.animalType = animalType;
        this.chanceToFindFood = chanceToFindFood;
        this.hoursSinceEaten = hoursSinceEaten;
        this.hoursSinceSlept = hoursSinceSlept;
        this.isAwake = isAwake;
    }

    //Methods of Animal class
    public void goToSleep () {
        isAwake = false;
        hoursSinceSlept = 0;
    }

    public void searchForFood () {
        double foodRoll = Math.random(); //This function returns a random number
        //From 0 to 1

        if (chanceToFindFood > foodRoll) {
            //The animal has found some food! Om nom nom
            hoursSinceEaten = 0;
        }
    }
}
```

### Naming Convention  

Class names are typically UpperCamelCase (i.e Dog, BassGuitar, FootballTeam, TelevisionShow).

The class name should make sense for what you're modeling. For example, if you're modeling a playing card, a logical class name would be `PlayingCard`. It is perfectly OK for class names to be long. `Card` would not be a great name for this class because it could model a credit card, a greeting card, a gift card, etc. You will see many class names in Java much longer than `PlayingCard`. This author may have gone a bit overboard though:

`InternalFrameInternalFrameTitlePaneInternalFrameTitlePaneMaximizeButtonWindowNotFocusedState`
