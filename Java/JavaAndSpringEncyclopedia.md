Language Specialization: Java and Spring Encyclopedia

The Encyclopedia is a compilation of all the lesson study notes in this course. Use it to look up the a term or concept that you have covered.

---

[Lesson: Java Lesson 1 - Getting Started](GettingStarted.md)

# Install Party  

## Terminology

Java - Java is one of the most versatile languages in the world. It runs behind the scenes for a large portion of the websites you use daily, as well as in many desktop and smartphone apps. It’s well-suited for large programs that need performance and scalability.

Java Development Kit (JDK) - The JDK is what turns the code you write into something the JRE can actually execute on your computer.

Java Runtime Environment (JRE) - the space and container in which the JVM will run.

Java Virtual Machine (JVM) - The JVM interprets byte code into the correct machine code based off of the underlying operating system and hardware combination.

Maven - Maven is a Java build tool that allows a project to build using its project object model (POM) and a set of plugins that are shared by all projects using Maven, providing a uniform build system.

IntelliJ - IntelliJ is a Java IDE used for developing computer software.

---

[Lesson: Java Lesson 1 - Getting Started](GettingStarted.md)

# Install Party  

## Terminology  

Java - Java is one of the most versatile languages in the world. It runs behind the scenes for a large portion of the websites you use daily, as well as in many desktop and smartphone apps. It’s well-suited for large programs that need performance and scalability.

Java Virtual Machine (JVM) - The JVM interprets byte code into the correct machine code based off of the underlying operating system and hardware combination.

byte code - these are the most basic instructions the JVM can accept. It is not very easy to read (see example below). There's a reason we don't want to be writing directly in byte code.

Examples  
Here are a few lines of Java byte code.

sipush  100
if_icmpge       44
iconst_2
istore_2

---

[Lesson: Java Lesson 2 - Type System](TypeSystem.md)

# Primitive Data Types  

## Terminology  

integer: counting numbers or whole numbers (...-1, 0, 1, 2, 3...)
int: 32-bit integer
short: 16-bit integer (very rarely used)
long: 64-bit integer
byte: 8-bit integer (very rarely used)
float: 32-bit floating point (rarely used)
double: 64-bit floating point
char: 16-bit unicode character
boolean: 1-bit value, true of false
String: A collection of chars in a sequential order.
Concatenation: the joining of two or more strings together.
Literal: refers to an actual value of the given type. For example, "Hello" is a String literal. true is a boolean literal.
Examples  
public class ExampleClass {
  int exampleInt = 25;
  short exampleShort = 25;
  long exampleLong = 25;
  byte exampleByte = 25;
  float exampleFloat = 25;
  double exampleDouble = 25;
  char exampleChar = 'a';
  String exampleString = "a string is a collection of chars";
  boolean exampleBoolean = false;
}
This example shows that an int, short, long, byte, float, and double could all have the same value. Remember that the point of having the different numeric data types is for memory efficiency. i.e. a byte takes up less memory than an int.

Below are a few examples of concatenation:


1
public class ExampleClass {
2
  public static void main(String args[])
3
    {
4
      int myAge = 23;
5
      System.out.println("You are " + myAge + " years old.");
6
    }
7
}

Fullscreen

Reset Code
Run Code 

1
public class ExampleClass {
2
  public static void main(String args[])
3
  {
4
    String a = "Hello";
5
    String b = "World";
6
    System.out.println(a + " " + b + "!");
7
  }  
8
}

Fullscreen

Reset Code
Run Code 
Here is another example of data types.


1
public class Main {
2
    public static void main(String[] args) {
3
       System.out.println("Hello Java Basic Types!");
4
​
5
       //create a java variable assign valid values and print
6
​
7
       boolean likesToCode = true;
8
       System.out.println("likesToCode: " + likesToCode);
9
​
10
       char aLetter = 'y';
11
       System.out.println("aLetter: " + aLetter);
12
​
13
       byte byteNumber  =  127;
14
       System.out.println("byteNumber: " + byteNumber);
15
​
16
       //a number of type short
17
       short longestShortNumber = 32767;
18
       System.out.println("longestShortNumber: " + longestShortNumber);
19
​
20
       //a large number of type long
21
       //Note the lower case 'l' at the end. This specifies the type of the
22
       //literal as long
23
       long longestLongNumber  =  9223372036854775807l;
24
       System.out.println("longestLongNumber: " + longestLongNumber);
25
​
26
       //a large number of type double, note 'E' notation for exponent
27
       double largestDouble  =  1.7976931348623157E308;
28
       System.out.println("largestDouble: " + largestDouble);
29
​
30
    }
31
}

Fullscreen

Reset Code
Run Code 

---

[Lesson: Java Lesson 2 - Type System](TypeSystem.md)

# Variables  

## Terminology  

Variable - A piece of memory that contains a data value.

Null - used to tell Java that a variable has an absence of value. Remember, null and zero are not the same.

camelCase - camelCase is a common naming convention across programming that helps the reader recognize the beginning of the next word. e.g. howAreYouToday

Examples  
Updating Variables  

1
public class Example {
2
  public static void main(String[] args) {
3
    int x = 42;
4
    System.out.println(x + 5);
5
    System.out.println("Now we are going to update the value of x");
6
    x = 95;
7
    System.out.println(x + 5);
8
  }
9
}

Fullscreen

Reset Code
Run Code 
Null  

1
public class Example {
2
  public static void main(String[] args) {
3
    String myHome = "condo in Midtown";
4
    System.out.println("I live in a " + myHome);
5
  }
6
}
7
​

Fullscreen

Reset Code
Run Code 
Here I'm saying that I have a condo in Midtown.


1
public class Example {
2
  public static void main(String[] args) {
3
    String myHome = " ";
4
    System.out.println("I live in a " + myHome);
5
​
6
  }
7
}    

Fullscreen

Reset Code
Run Code 
Here I'm saying that I have a home, but maybe it doesn't have a name. Or if it does have a name, the name of it is just .


1
public class Example {
2
  public static void main(String[] args) {
3
    String myHome = null;
4
    System.out.println("I live in a " + myHome);
5
  }
6
}

Fullscreen

Reset Code
Run Code 
Here I'm saying that I don't have a home. The value of myHome is nothing and therefore I'm homeless.

---

Lesson: Java Lesson 3 - Basic Tools
Method Return Study Notes  
Terminology  
Examples  

---

[Lesson: Java Lesson 3 - Basic Tools](BasicTools.md)

# Printing to the Command Line  

## Examples  

Below is an example of System.out.println():


1
public class Example {
2
  public static void main(String[] args) {
3
    System.out.println("This sentence will be printed to the console.");
4
    System.out.println("This sentence will be underneath the previous sentence.");
5
  }
6
}

Fullscreen

Reset Code
Run Code 
Now, look at this example of System.out.print():


1
public class Example {
2
  public static void main(String[] args) {
3
    System.out.print("This sentence will be printed to the console.");
4
    System.out.print("This sentence will be connected to the previous sentence.");
5
  }
6
}

Fullscreen

Reset Code
Run Code 
As you can see, println() creates a new line, whereas print() does not. Generally, println() will make outputs easier to read.

---

[Lesson: Java Lesson 3 - Basic Tools](BasicTools.md)

# Command Line Input  

## Terminology  

Standard Streams - refer to the input and output streams that are provided for us by default.

Scanner - a class introduced in Java 5. It is a good default option for reading files or getting input from the user.

Examples  

1
import java.util.Scanner;
2
​
3
public class Example {
4
  public static void main(String[] args) {
5
    System.out.println("Hello! What's your name?");
6
    Scanner scanner = new Scanner(System.in);
7
    String name = scanner.nextLine();
8
    // the user is asked to input on the command line through the Scanner class
9
    System.out.println("Nice to meet you, " + name + "!");
10
​
11
    System.out.println("What city do you live in?");
12
    String city = scanner.nextLine();
13
    System.out.println(name + " lives in " + city);
14
​
15
    System.out.println("How many years have you lived in " + city + "?");
16
    String yearsLived = scanner.nextLine();
17
    System.out.println(name + " has been living in " + city + " for " + yearsLived + " years.");
18
  }
19
}

Fullscreen

Reset Code
Run Code 
As you can see, you can use information from the command line and store it in variables. This is cool stuff!

---

[Lesson: Java Lesson 3 - Basic Tools](BasicTools.md)

# Java & JavaScript  

## Terminology  

Strongly-Typed vs. Weakly-Typed - A strongly-typed programming language enforces a type discipline. A weakly-typed programming language does not enforce any type disciplines. Take a look at the examples below.

Examples  
Below is a code sample in Java that would not compile:

int x = 5;
x = "hello";
This is an example of Java having strongly-typed data types. If you declare a variable an int, then it will always be an int. In contrast JavaScript has very limited typing of variables.

var x = 5;
x = "hello";

---

Lesson: Java Lesson 4 - Intro to OOP
CLASSES VS OBJECTS  
Terminology  
template - A definition of what some object will look like
instance - A concrete object
Examples  
// class definition
class Student {
    int id;
    string name;
    void print() { .. }
}
// an instance of Student
// student1 is an object
Student student1 = new Student();

---

[Lesson: Java Lesson 4 - Intro to OOP](IntroToOOP.md)

# TURN CONCEPT INTO CLASS  

# Terminology 

concept - An idea. Nouns are easier to turn into classes.
methods - Operations on a class
variables - Data maintaining state of a class
public & private - Determines whether access outside of class is permitted
getter & setter methods - Methods to read & write private data in a class
Examples  
// models a generic bank account
public class Account {
    // private data
    private String description;
    private double balance;
    // public methods
    public void deposit(double amount) {
        balance += amount;
    }
    public void withdraw(double amount) {
        balance -= amount;
    }
    // getter method to get balance
    public double getBalance() {
        return balance;
    }
}
public class House {

  // these are properties (or data)

  String color;
  String location;
  int rooms;
  int value;
  boolean paidOff;

  // this is a method (or behavior)

  public int appreciatingInValue(int value) {
    value *= 1.009;
    return value;
  }
}

---

[Lesson: Java Lesson 5 - OOP Discussion](OOPDiscussion.md)

# USING DOT NOTATION WITH OBJECTS  

## Terminology  

properties - The data within an object
methods - The operations or functions within an object
Examples  
// create an instance of Student
Student aStudent = new Student();
// set the value of id and name
aStudent.id = 1001;
aStudent.name = "George Washington";
// call the print method passing in the id and name
aStudent.print(aStudent.id, aStudent.name);

---

[Lesson: Java Lesson 5 - OOP Discussion](OOPDiscussion.md)

# REASON FOR OOP  

## Terminology

logic error - An error created by doing something incorrectly
model the world - Creating program constructs that operate like real world objects
Examples  
-- pseudocode
Definition of Account
    Data description, balance
    Operation Deposit In amount
        Assign balance + amount => balance
    End
    Operation Withdraw In amount
        Assign balance - amount => balance
    End
End

Definition of Savings USING Account
    -- description & balance included automatically
    Data interestRate
    -- Deposit & Withdraw included automatically
    Operation PayInterest
        Assign (balance * interestRate) + balance => balance
    End
End

---

[Lesson: Java Lesson 6 - Inheritance and Encapsulation](InheritanceAndEncapsulation.md)

# Authoring classes with Inheritance  

## Terminology 

Inheritance - Inheritance allows a class to use the properties and methods of another class. In other words, the derived class inherits the states and behaviors from the base class. The derived class is also called subclass and the base class is also known as super-class.

extends keyword - used when creating a subclass from another class.

Examples  
Let's start with a base class of Animal:

public class Animal {
    public String name;

    public void makeNoise() {
          System.out.println("Boo?");
    }
}  
Then let's create a Dog class, because dogs make their own kind of noise, so we want makeNoise() to do something specific when the animal is a dog:

public class Dog extends Animal {

    public void makeNoise() {
          System.out.println("Bark!");
    }
}
What about a cow?

public class Cow extends Animal {

    public void makeNoise() {
          System.out.println("Mooh!");
    }
}
What about a snake?

public class Snake extends Animal {

    public void makeNoise() {
          System.out.println("Sssss");
    }

}

---

[Lesson: Java Lesson 6 - Inheritance and Encapsulation](InheritanceAndEncapsulation.md)

# Using Public, Private, and Protected Access Modifiers  

## Terminology  

public - Accessible to all classes
private - Accessible only within the current class
protected - Same as private except provides access to inheriting classes
Examples  
// public access so class can be create from any other class
public class Account {
    // properties of a class are typically private
    private int id;
    private String description;
    private double balance;
    // methods of a class are typicially public
    public int getId() { return this.id; }
    public String getDescription() { return this.description; }
    public void setDescription(String aDescription) { this.description = aDescription; }
    public double getBalance() { return this.balance; }
    // method that is private except for inheriting classes
    protected void setBalance(double aBalance) { this.balance = aBalance; }
}

---

[Lesson: Java Lesson 7 - Exceptions and Static](ExceptionsAndStatic.md)

# Static Keyword  

## Terminology  

static - Not related to an instance of the class. A static variable belongs to the class itself.
Examples  
public class BankAccount {

    //This is a static (final) variable that represents a default interest rate
    //1.05 Translates to a 5% interest rate
    public static final double INTEREST_RATE = 1.05;

    double balance;
    String accountOwner;

    //This is a non-static variable which represents this particular account's
    //Interest rate
    double interestRate;

    //This is a static method to calculate interest in general
    //It doesn't care about having an instance of the BankAccount class
    public static double calculateInterest (double base, int numPeriods) {
        for (int i = 0; i < numPeriods; i++) {
            base *= INTEREST_RATE;
        }
    }

    //This is a non-static method that actually applies interest to the account
    public void applyInterest () {
        balance *= interestRate;
    }
}

---

[Lesson: Java Lesson 7 - Exceptions and Static](ExceptionsAndStatic.md)

# Exceptions and Control Flow  

## Terminology

Exception - an Object representing unexpected or undesirable circumstances of execution.

try - This defines the main code block that we will attempt to execute. If an exception is encountered, it will either be thrown up or caught in a catch block.

catch - This block defines what will happen if the specified exception is thrown. One common step is to print the stack trace of an exception by calling ex.printStackTrace().

finally - the finally block will always run at after the try block exits, whether an Exception was thrown or not.

Examples  

1
public class ExceptionExample {
2
​
3
    public static void main(String[] args) {
4
        System.out.println("Please input a number");
5
        getNumberAndDiscuss();
6
​
7
        System.out.println("Now input something that's not a number.");
8
        getNumberAndDiscuss();
9
​
10
        System.out.println("(No discussion if you didn't enter a number)");
11
    }
12
​
13
    public static void getNumberAndDiscuss() throws NumberFormatException {
14
        String input = System.console().readLine();
15
        //String input = new Scanner(System.in).nextLine();
16
​
17
        int response = Integer.parseInt(input);
18
​
19
        //This "discussion" will only take place if an exception is NOT thrown
20
        System.out.println("Now let's have a long discussion about the number.");
21
        System.out.println(".....");
22
        System.out.println("The number you entered was " + response + ". Cool number!");
23
        System.out.println("Great discussion!");
24
    }
25
  }

Fullscreen

Reset Code
Run Code 

---

Lesson: Java Lesson 7 - Exceptions and Static
Using Lists Study Notes  
Terminology  
List - a basic collection for storing objects.
Examples  

1
import java.util.List;
2
import java.util.ArrayList;
3
​
4
public class AnotherListXample {
5
​
6
    public static void main (String[] args) {
7
        List<Integer> squareNums = new ArrayList<>();
8
        for (int i = 0; i < 10; i++) {
9
            squareNums.add(i * i);
10
        }
11
        for (Integer square : squareNums) {
12
            System.out.println(square);
13
        }
14
    }
15
}

Fullscreen

Reset Code
Run Code 

---

Lesson: Java Lesson 8 - Arrays
Arrays with Multiple Values  
Terminology  
Examples  
Arrays can be created with multiple values by providing values within { } during initialization. String[] names = {"John", "Mary", "David", "Paula"};

Here are some additional examples.

class ArraysDemo {

    public static void main(String[] args) {
        //create an array of five ints (value zero by default)
        int[] numbers = new int[5];

        //Abbreviations of five states
        String[] fiveStatesNames = {
               "DC", "VA", "NC", "MD", "DE"
            };

        //populations of DC, VA, NC, MD, DE
        int[] fiveStatesPopulation = {
            681_170, 8_411_808, 10_146_788, 6_016_447, 952_065
        };
    }
}

---

Lesson: Java Lesson 8 - Arrays
Iteration  
Terminology  
for-each loop: syntactic sugar to loop over elements in a collection (arrays are collections)

Examples  
Here is an example of using a for-each loop to iterate over an array of Strings.


1
public class ArrayExample {
2
    public static void main (String[] args) {
3
        String[] tolkienBooks = {"Fellowship", "Two Towers", "Return", "Silmarillion"};
4
​
5
        for (String book : tolkienBooks) {
6
            System.out.println(book + " is a great book!");
7
        }
8
    }
9
}

Fullscreen

Reset Code
Run Code 

---

Lesson: Java Lesson 8 - Arrays
Filtering an Array  
Terminology  
toArray() - a method in the List interface that allows us to convert a List into an array automatically
Examples  
The toArray() function unfortunately can't create an array of simple types (i.e. int[], double[], etc). In the following example we'll work around that limitation:


1
public class ArraySimple {
2
​
3
    public static void main (String[] args) {
4
        int[] numbers = {1, 2, 3, 5, 9, 12, 24};
5
​
6
        int divisibility = 3;
7
        int numKeeping = 0;
8
​
9
        //Search through first to find the size of array necessary
10
        for (int n : numbers) {
11
            if (n % divisibility == 0) {
12
                numKeeping++;
13
            }
14
        }
15
        int[] filteredNumbers = new int[numKeeping];
16
        int filteredIndex = 0;
17
        for (int i = 0; i < numbers.length; i++) {
18
            if (numbers[i] % divisibility == 0) {
19
                filteredNumbers[filteredIndex] = numbers[i];
20
                filteredIndex++;
21
            }
22
        }
23
​
24
        System.out.println("Remaining values: ");
25
        for (int n : filteredNumbers) {
26
            System.out.print(n + " ");
27
        }
28
    }
29
}

Fullscreen

Reset Code
Run Code 

---

Lesson: Java Lesson 9 - File I/O and toString()
File I/O Study Notes  
Terminology  
FileWriter - a class Java provides us to take care of writing contents to a file. Scanner - we've used Scanner before, to get input from the user. Here we are using it to read a file.

Examples  
The following code constructs a Bank object from the bank.txt file. This file is expected to have a specific format. Read through the code and see if you can figure out what the file is supposed to look like.

public static Bank readBank() {
    try {
        File bankFile = new File("bank.txt");
        Scanner fileScanner = new Scanner(bankFile);
        String bankName = null;
        String bankBalanceStr = null;  
        while (fileScanner.hasNext()) {
            String currentLine = fileScanner.nextLine();
            if (currentLine.startsWith("bank.name")) {
                bankName = currentLine.split("=")[1];
            }
            if (currentLine.startsWith("bank.totalBalance")) {
                bankBalanceStr = currentLine.split("=")[1];
            }
        }
        if (bankName != null) {
            Bank savedBank = new Bank(bankName);
            return savedBank;
        }
    } catch (Exception exception) {
        exception.printStackTrace();
    }

    return null;
}

---

Lesson: Java Lesson 9 - File I/O and toString()
Override toString Study Notes  
Terminology  
toString() - All objects in Java have a toString() method. By default, this will print out the memory address of the object (usually not very useful). You can override this method to provide useful information.

@Override - this is an example of an annotation, something we will get into more later. For now, just be aware that this is a way of passing information to the compiler, telling it that we are overriding a method from the superclass

Examples  
Here is an example of a BankAccount class with an overridden toString() method. We've added a little "box" around the info to make it pretty.


1
public class BankAccount {
2
    double balance;
3
    String accountType;
4
    String accountOwner;
5
​
6
    public static void main(String[] args) {
7
        BankAccount ba1 = new BankAccount(1140.50, "Checking", "John");
8
        BankAccount ba2 = new BankAccount(60.00, "Savings", "Sal");
9
​
10
        System.out.println(ba1);
11
        System.out.println(ba2);
12
    }
13
​
14
    public BankAccount(double balance, String accountType, String accountOwner) {
15
        this.balance = balance;
16
        this.accountType = accountType;
17
        this.accountOwner = accountOwner;
18
    }
19
​
20
    @Override
21
    public String toString () {
22
        String response = "-------" + "\nAccount Owner: " + accountOwner;
23
        response += "\nAccount Type: " + accountType;
24
        response += "\nCurrent Balance: $" + balance;
25
        response += "\n-------";
26
        return response;
27
    }
28
}

Fullscreen

Reset Code
Run Code 

---

Lesson: Java Lesson 10 - Interfaces and Abstract Classes
Implementing Comparable Study Notes  
Terminology  
Interface - defines a contract (list of methods) that any class implementing the interface must implement.

Comparable - this is an interface with only one method: compareTo(). Implementing this interface allows us to determine the "natural order" of our objects.

Collections.sort() - the Collections class is a grouping of static methods for working with Collections (Lists, Maps, Sets, etc.). The sort() method sorts the objects according to their natural order.

Examples  

---

Lesson: Java Lesson 10 - Interfaces and Abstract Classes
Abstract Classes  
Terminology  
abstract - an abstract class cannot be directly instantiated; instead you must first extend it into a non-abstract subclass. Abstract classes can serve as a "template" for a group of related subclasses, similar to interfaces.

Examples  
Here is a more complex example of abstract classes. We have two abstract base classes (BodyPart and ClothingItem) which each have two subclasses (Arm and Leg for BodyPart, and Shirt and Pants for ClothingItem).

public abstract class BodyPart {
    private String ownerName;

    public BodyPart (String ownerName) {
        this.ownerName = ownerName;
    }

    public String getOwnerName () {
        return ownerName;
    }
}

public class Leg extends BodyPart {

    public Leg (String ownerName) {
        super(ownerName);
    }
}

public class Arm extends BodyPart {

    public Arm (String ownerName) {
        super(ownerName);
    }
}
import java.util.List;

public abstract class ClothingItem {

    private double weight;
    private boolean isRippedToShreds;

    public ClothingItem (double weight) {
        this.weight = weight;
        isRippedToShreds = false;
    }

    //Returns true if successful, otherwise false
    public abstract boolean putOn (List<BodyPart> bodyPartList);

    public void ripToShreds () {
        isRippedToShreds = true;
    }

    public void repair () {
        isRippedToShreds = false;
    }

    public double getWeight () {
        return weight;
    }
}
Pants will require two Leg objects.

public class Pants extends ClothingItem {

    public Pants (double weight) {
        super(weight);
    }

    public boolean putOn (List<BodyPart> bodyPartList) {
        //We will put on pants if we're given two legs from the same owner
        if (bodyPartList.size() != 2) {
            return false;
        }
        if (!(bodyPartList.get(0).getOwnerName().equals(bodyPartList.get(1).getOwnerName()))) {
            return false;
        }
        if (!(bodyPartList.get(0) instanceof Leg)) {
            return false;
        }
        if (!(bodyPartList.get(1) instanceof Leg)) {
            return false;
        }
        return true;
    }
}
A shirt requires two Arm objects.

public class Shirt extends ClothingItem {

    public Shirt (double weight) {
        super (weight);
    }

    public boolean putOn (List<BodyPart> bodyPartList) {
        //We will put on a shirt if we're given two arms from the same owner
        if (bodyPartList.size() != 2) {
            return false;
        }
        if (!(bodyPartList.get(0).getOwnerName().equals(bodyPartList.get(1).getOwnerName()))) {
            return false;
        }
        if (!(bodyPartList.get(0) instanceof Arm)) {
            return false;
        }
        if (!(bodyPartList.get(1) instanceof Arm)) {
            return false;
        }
        return true;
    }
}

---

Lesson: Java Lesson 11 - Boxed Types, Enums, and Recursion
Box Types  
Terminology  
Boxed Type - AKA "Wrapper Class", every primitive type (e.g. int, double) has a box type associated with it (int -> Integer, double -> Double, etc). They are treated as a mix between primitives and full Objects, and they have useful static methods.

Examples  
//Here are some examples of how the compiler will effectively convert from primitive to box types for us
List<Integer> numbers = new ArrayList<>();
int x = 5;
numbers.add(x); //This works even though x is an int not an Integer.
//It will be automatically "boxed" into an Integer
Integer y = x;
Integer z = new Integer(7);
numbers.add(z);
int w = numbers.get(0);
Double d = 2.9;
Float f = 2.129f;
d += 1.0; //We can use arithmetic operators (+, -, *, /) as normal
f /= 9293;
Here is an example of a static method from the Integer class, Integer.parseInt(). It allows us to take a String (for example "4572") and attempt to parse it as an Integer. If the String can't be successfully parsed as an Integer (for example, if the input was not a number, like "hello") a NumberFormatException will be thrown. Exceptions are covered in a separate lesson; for now just experiment with the code below and see how it works, and look at the use of the Integer.parseInt(String) function.


1
import java.util.Scanner;
2
public class BoxTypes {
3
​
4
    public static void main(String[] args) {
5
        //Integer.parseInt example
6
        while (true) {
7
            System.out.println("Please input a number.");
8
​
9
            String input = new Scanner(System.in).nextLine();
10
​
11
            try {
12
                // **Here is the Integer.parseInt(String s) call**
13
                int value = Integer.parseInt(input);
14
                System.out.println("You typed in the number " + value + ". Multiplied by 5 that's " + value * 5 + ".");
15
            } catch (NumberFormatException ex) {
16
                System.out.println("That wasn't a number. Goodbye.");
17
                break;
18
            }
19
        }
20
    }
21
}

Fullscreen

Reset Code
Run Code 

---

Lesson: Java Lesson 11 - Boxed Types, Enums, and Recursion
Enumerations  
Terminology  
Enum - Short for enumeration, enums are a way of defining a set of constant values.

Examples  
Here is an example of a Color enum that keeps track of RGB colors.

public enum Color {

    RED (255, 0, 0),
    GREEN (0, 255, 0),
    BLUE (0, 0, 255),
    PURPLE (255, 0, 255),
    YELLOW (255, 255, 0),
    WHITE (255, 255, 255),
    BLACK (0, 0, 0);



    private int red;
    private int green;
    private int blue;

    Color (int r, int g, int b) {
        red = r;
        green = g;
        blue = b;
    }

    public int getRed() {
        return red;
    }

    public int getGreen() {
        return green;
    }

    public int getBlue() {
        return blue;
    }
}
Note: in Java you will probably never have a reason to define your own Color enum, because whichever graphics framework/library you are working with will have already done so.

---

Lesson: Java Lesson 11 - Boxed Types, Enums, and Recursion
Recursion  
Terminology  
Recursion - solving problems by breaking them into smaller versions of themselves. A function that calls itself is an example of this.

Examples  
The following is a recursive example to numbers in the Fibonacci sequence (which is explained in the example if you're not familiar with it). Start with small values (10-20) and observe how the number of recursive function calls increases exponentially. (Side note for the mathematically-inclined: the rate of increase is equal to the golden ratio, ~1.61803. So the number of function calls required to calculate Fibonacci(40) is ~1.618 times the number required to calculate Fibonacci(39))


1
public class FibonacciExample {
2
​
3
    static long numFunctionCalls;
4
​
5
    public static void main(String[] args) {
6
        System.out.println("Welcome to Fibonacci calculation. Put in a negative number to exit.");
7
        System.out.println("Fibonacci(0) = 0");
8
        System.out.println("Fibonacci(1) = 1");
9
        System.out.println("Fibonacci(2) = Fibonacci(2-1) + Fibonacci (2-2)");
10
        System.out.println("(i.e. sum of the previous two)");
11
        System.out.println("Beginning sequence goes like this: 1, 1, 2, 3, 5, 8, 13, 21...");
12
        while (true) {
13
            System.out.println("Calculate which Fibonacci number?");
14
            String input = System.console().readLine();
15
            int response = Integer.parseInt(input);
16
            if (response < 0) {
17
                break;
18
            }
19
            if (response > 92) {
20
                System.out.println("Sorry, we cannot easily calculate over 92 as it overflows.");
21
                response = 92;
22
            }
23
            long startTime = System.currentTimeMillis();
24
            numFunctionCalls = 0;
25
            System.out.println("The " + response + "th Fibonacci is " + calcFibonacci(response));
26
            long endTime = System.currentTimeMillis();
27
            System.out.println("Processing completed in " + (endTime - startTime) + " milliseconds.");
28
            System.out.println("Completed in " + numFunctionCalls + " function calls.");
29
        }
30
        System.out.println("Goodbye!");
31
    }

Fullscreen

Reset Code
Run Code 

---

Lesson: Java Lesson 12 - Collections
Java Collections  
Terminology  
JCF - Java Collections Framework, a library that contains collections; ways of grouping data/objects together.

Examples  

---

Lesson: Java Lesson 13 - Unit Testing and HashCode
Unit Testing Study Notes  
Terminology  
Unit Test - unit tests test a small "unit" of code (a single method) to see if it is working properly
JUnit - a Java unit testing framework
ExpectedException rule - a tool in JUnit that allows us to make sure exceptions are being thrown in the way we expect. If an exception isn't thrown, the test will fail
Examples  
Consider the following function which should return true if the given number is within 2 of a multiple of 10 (i.e. number ends in 0, 1, 2, 8, or 9). The input will be non-negative.

public boolean nearMultTen (int n) {
    return false;
}
Here are a few asserts you could write to test the function:

@Test
public void someTests () throws Exception {
    assertTrue(nearMultTen(10));
    assertTrue(nearMultTen(159));
    assertFalse(55);
}
These three asserts do not test a wide enough range of values. Consider additional values you could test to make sure your function works in all cases.

---

Lesson: Java Lesson 13 - Unit Testing and HashCode
Override HashCode Study Notes  
Terminology  
@Override - (Reminder) this annotation tells the compiler we are overriding a method form our superclass.

HashCode - a numerical representation of an object, used for organizing the data in collections.

Bucket - buckets are how some collections like HashMap organize their data. All of the objects in the same bucket share the same hashcode.

Examples  
Here is an example of a very simple Animal class, with a hashCode() method provided by IntelliJ.

public class Animal {

    int yearBorn;
    String name;

    @Override
    public int hashCode() {
        int result = yearBorn;
        result = 31 * result + (name != null ? name.hashCode() : 0);
        return result;
    }
}

---

Lesson: Java Lesson 14 - Lambda Expressions
Lambda Expressions  
Terminology  
Functional Interface - an interface with only one method; Comparator for example.

Higher-Order Function - A function that accepts another function as one of its arguments.

Lambda Expression - an anonymous function that is passed as an argument to a higher-order function. Lambda (λ) is the 11th letter of the Greek alphabet and (like many Greek letters) you will see it crop up in various areas of math/science.

Anonymous Function - a function that has no name (Lambda Expressions in Java).

Functional Programming - a different style of programming that places higher value on functions. A "theme" in Functional Programming is that we want methods to not have any side effects (i.e. make changes to variables or other data). Instead of x = x + 5 we would say int x2 = x + 5.

Examples  
Here are two examples of simple Lambda Expressions to solve a problem we've already seen, FizzBuzz. To briefly restate the problem: we are supposed to print the numbers 1-100, but if the number is divisible by three we will instead print "Fizz"; if divisible by 5 "Buzz", and if both "FizzBuzz". In the first example, we have defined the Functional Interface DivisibilityCheck. We have three implementations, divBy3, divBy5, and divBy15, which are only responsible for checking whether the given number is cleanly divisible.


1
public class LambdaExample {
2
​
3
    interface DivisibilityCheck {
4
        boolean checkDivisibility (int d);
5
    }
6
​
7
    public static void main(String[] args) {
8
        DivisibilityCheck divBy3 = (d) -> d % 3 == 0;
9
        DivisibilityCheck divBy5 = (d) -> d % 5 == 0;
10
        DivisibilityCheck divBy15 = (d) -> d % 15 == 0;
11
​
12
        for (int i = 1; i <= 100; i++) {
13
            if (divBy15.checkDivisibility(i)) {
14
                System.out.println("FizzBuzz");
15
            } else if (divBy3.checkDivisibility(i)) {
16
                System.out.println("Fizz");
17
            } else if (divBy5.checkDivisibility(i)) {
18
                System.out.println("Buzz");
19
            } else {
20
                System.out.println(i);
21
            }
22
        }  
23
    }
24
}

Fullscreen

Reset Code
Run Code 
Here is another example that solves the same problem with Lambda Expressions. This time, we are going to ask the Lambda to do a bit more of the work for us (actually all of it). Instead of just outputting a boolean, fizzBuzzOutput(int n) produces the String we need.


1
public class LambdaExample {
2
    interface FizzBuzz {
3
        String fizzBuzzOutput (int n);
4
    }
5
​
6
    public static void main(String[] args) {
7
        FizzBuzz fizzBuzz = (n) -> {
8
            String response = "";
9
            if (n % 3 == 0) {
10
                response += "Fizz";
11
            }
12
            if (n % 5 == 0) {
13
                response += "Buzz";
14
            }
15
            if (response.equals("")) {
16
                response = "" + n;
17
            }
18
            return response;
19
        };
20
        for (int i = 1; i <= 100; i++) {
21
            System.out.println(fizzBuzz.fizzBuzzOutput(i));
22
        }
23
    }
24
}

Fullscreen

Reset Code
Run Code 

---

Lesson: Java Lesson 15 - Java8 Streams
Java 8 Streams  
Terminology  
Stream - a new way of working with collections in Java 8. Streams provides methods that filter or otherwise manipulate data in collections. Stream methods return Streams, so they can be "chained" together.

Aggregate Operations - also known as Stream Operations, Aggregate Operations are related to Iterators but work a little differently under the hood. Multiple aggregate operations chained together form a "pipeline".

Pipeline - a chain of Aggregate Operations.

Examples  
The code below shows examples of how we can use Stream functions to manipulate a collection. In this case, we start with a List<Animal>. Animal is a Java Bean (a simple class that just contains data, no real behavior/methods) that has the simple types: String name, String type, int age, boolean canFly, boolean canSwim.

Remember that Java supports dynamic whitespace.

animalList.stream()
        .filter(e -> !e.canFly())
        .filter(e -> !e.canSwim())
        .forEach(e -> System.out.println("\t" + e.getName()));
is the same as

animalList.stream().filter(e -> !e.canFly()).filter(e -> !e.canSwim()).forEach(e -> System.out.println("\t" + e.getName()));

The first option is more user-friendly and easier to read. Always remember that people other than you have to read your code!


1
import java.util.ArrayList;
2
import java.util.List;
3
​
4
public class StreamExample {
5
​
6
    public static void main(String[] args) {
7
        List<Animal> animalList = initializeList();
8
​
9
        System.out.println("Animals that can fly:");
10
        animalList.stream()
11
                .filter(e -> e.canFly())
12
                .forEach(e -> System.out.println("\t" + e.getName()));
13
​
14
        System.out.println("Animals with two names:");
15
        animalList.stream()
16
                .filter(e -> e.getName().contains(" "))
17
                .forEach(e -> System.out.println("\t" + e.getName()));
18
​
19
        System.out.println("Elephants:");
20
        animalList.stream()
21
                .filter(e -> e.getType().equals("Elephant"))
22
                .forEach(e -> System.out.println("\t" + e.getName()));
23
​
24
        System.out.println("Animals that can't swim OR fly:");
25
        animalList.stream()
26
                .filter(e -> !e.canFly())
27
                .filter(e -> !e.canSwim())
28
                .forEach(e -> System.out.println("\t" + e.getName()));
29
​
30
        System.out.println("Animals whose name matches their type:");
31
        animalList.stream()

Fullscreen

Reset Code
Run Code 

---

Lesson: Java Lesson 15 - Java8 Streams
Java 8 Stream Map Transpose  
Terminology  
map() - this function allows us to change the type of data we are working with. Don't get confused with the data type Map<K, V>. The terms are related because both of them contain a mapping. In the case of the data structure Map<K, V> the mapping is from Key to Value. In the case of the map() function the mapping is from one data type to another.

Examples  
Let's say we are interested in the lengths of String objects. We have a NumberString class, which accepts an int in its constructor and sets its String value parameter as the English version of that number (for example, new NumberString(3).getValue() produces "Three").


1
import java.util.Arrays;
2
import java.util.List;
3
import java.util.stream.Collectors;
4
​
5
public class StreamMapExample {
6
​
7
    public static void main(String[] args) {
8
        List<NumberString> numberStrings = Arrays.asList("Blah", "Bli p", "Bloo op", "Beeple").stream()
9
                .map(name -> name.length()) //Go from the Strings ^^ to their lengths
10
                .map(e -> new NumberString(e)) //Go from the lengths to NumberString objects
11
                .collect(Collectors.toList());
12
​
13
        for (NumberString ns : numberStrings) {
14
            System.out.println(ns.getValue());
15
        }
16
    }
17
}
18
class NumberString {
19
​
20
    private String value;
21
​
22
    public NumberString (int num) {
23
        switch (num) {
24
            case 0:
25
                value = "Zero";
26
                break;
27
            case 1:
28
                value = "One";
29
                break;
30
            case 2:
31
                value = "Two";

Fullscreen

Reset Code
Run Code 
Extension excercise: notice anything wrong with the NumberString class? Think about the range of possible inputs and how it would handle them. Change the class by having it throw an exception for bad input.

---

Lesson: Java Lesson 16 - Maven
Maven  
Terminology  
Maven - Maven is a tool used for building and managing Java-based projects.

Examples  

---

Lesson: Java Lesson 16 - Maven
Using Maven  
Terminology  
compile - compiles the Java code

test - runs the tests

package - creates the JAR

clean - removes the target direct (leaves you with just the src and the pom.xml)

Examples 

---

Lesson: Java Lesson 17 - Spring HTML
Java Annotations  
Terminology  
Annotation - a way of passing information to either the compiler or to a framework. A form of metadata.

Metadata - The Greek prefix "meta" means about; metadata is data about data.

@Override - this annotation tells the compiler that we are overriding a method from a superclass

@RequestMapping - this annotation asks Spring to call the attached function whenever the given path is reached.

@Autowired - this annotation asks Spring to automatically "hook up" the item associated with the annotation.

Examples  

---

Lesson: Java Lesson 17 - Spring HTML
Basic @Controller Study Notes  
Terminology  
Examples  

---

Lesson: Java Lesson 18 - Thymeleaf
Thymeleaf Study Notes  
Terminology  
Thymeleaf - a templating engine that allows us to author dynamic HTML. Similar to Mustache, Django, or Handlebars (other template engines).

Standard Expression - Thymeleaf has different types of expressions. The most common one you will use to start will be:

Variable Expression: ${variableName} Thymeleaf will fill in values based on the Model.

Examples  

---

Lesson: Java Lesson 19 - REST APIs
RestTemplate Study Notes  
Terminology  
RestTemplate - a Spring tool that makes it easy to query an API and get data back.

Examples  
Spring guide for getting started with RestTemplate

---

Lesson: Java Lesson 19 - REST APIs
REST APIs  
Terminology  
REST - stands for REpresentational State Transfer. It is a way to transfer data between a server and client, often in JSON.

API - Application Programming Interface (this is a reminder)

@RequestMapping - this annotation is associated with the method we want to be called when the given path is requested with the given method.

@RestController - this annotation identifies the file as the REST controller for our app. It tells Spring where to look for those JSON endpoints.

Examples  
The code below is a REST Controller that is part of a "Mastermind" web app. Mastermind is a board game where players take turns trying to guess a "secret code" of colored pegs. In this case, there are four endpoints:

board.json : tells the app the state of the current board so that it can be rendered
submit-move.json : allows us to attempt to submit a new move to the board
get-settings.json : gets the current "settings" information
save-settings.json : attempts to update settings
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import javax.servlet.http.HttpSession;
import java.util.List;
import java.util.Random;

@RestController
public class MastermindRestController {

    @RequestMapping(path="/board.json", method = RequestMethod.GET)
    public List<CodeRow> getBoardJson (HttpSession session) {
        Board board = (Board) session.getAttribute("board");
        if (board == null) {
            SettingsContainer settings = (SettingsContainer)session.getAttribute("settings");
            int boardWidth = settings.getBoardWidth();
            boolean duplesAllowed = settings.isDuplesAllowed();
            int numColors = settings.getNumColors();
            board = new Board(new CodeRow(new Random(), boardWidth, duplesAllowed, numColors));
            session.setAttribute("board", board);
        }
        return board.getGuesses();
    }

    @RequestMapping(path="/submit-move.json", method = RequestMethod.POST)
    public CompareResult submitMove (HttpSession session, @RequestBody String[] simpleRow) {
        Board board = (Board) session.getAttribute("board");
        if (board == null) {
            SettingsContainer settings = (SettingsContainer)session.getAttribute("settings");
            int boardWidth = settings.getBoardWidth();
            boolean duplesAllowed = settings.isDuplesAllowed();
            int numColors = settings.getNumColors();
            board = new Board(new CodeRow(new Random(), boardWidth, duplesAllowed, numColors));
            session.setAttribute("board", board);
        }

        Color[] colors = new Color[simpleRow.length];
        for (int index = 0; index < simpleRow.length; index++) {
            colors[index] = Color.valueOf(simpleRow[index].toUpperCase());
        }

        CodeRow codeRow = new CodeRow(colors);

        board.addGuess(codeRow);
        System.out.println("Secret = " + board.getSecret());
        return CodeRow.compareTwoRows(board.getSecret(), codeRow);
    }

    @RequestMapping(path="/save-settings.json", method = RequestMethod.POST)
    public SettingsContainer saveSettings (HttpSession session, @RequestBody SettingsContainer settings) {
        session.setAttribute("settings", settings);
        return (SettingsContainer)session.getAttribute("settings");
    }

    @RequestMapping(path="/get-settings.json", method = RequestMethod.GET)
    public SettingsContainer getSettings (HttpSession session) {
        SettingsContainer settings = (SettingsContainer) session.getAttribute("settings");
        if (settings == null) {
            settings = SettingsContainer.getDefaultSettings();
            session.setAttribute("settings", settings);
        }
        return settings;
    }
}

---

Lesson: Java Lesson 20 - JDBC and Transactional
Java Annotations  
Terminology  
Repository - where CRUDding the database is done

@Autowired - this annotation tells Spring that we want it to "set things up for us". An autowired object (connection to the repository) is created "behind the scenes" for us by Spring.

model or domain - common package names where the JavaBeans that map to tables live.

Examples  

---

Lesson: Java Lesson 20 - JDBC and Transactional
Java Annotations  
Terminology  
@Transactional - A transaction is a way to wrap up a group of database operations and say "these are all part of the same transaction. If one of them fails, roll them all back."

Examples  

---

Lesson: Java Lesson 21 - JPA
Google "JPA tutorial" and there are a bunch of them. Pick the one that works for you.

---

Lesson: Java Lesson 22 - Authorization and Authentication
Spring guide. https://spring.io/guides/gs/securing-web/

Spring security http://docs.spring.io/spring-security/site/docs/4.2.1.RELEASE/reference/htmlsingle/

---

Lesson: Java Lesson 24 - Deployment
Heroku Deployment  
Terminology  
Heroku - a (free) hosting service for our apps.

Examples
