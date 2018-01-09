# Boxed Types

By now you should be familiar with the basic primitive data types (int, boolean, double, and so on). Primitive types are useful because they are the simplest form of data. However, that simplicity comes at a cost. Primitive types aren't handled like proper objects and classes.

Primitive types have a related class "boxed type" class that represents the primitive type as an object with methods.

For example:

int => Integer
double => Double
char => Character
boolean => Boolean
There are two main reasons you will want to be familiar with these classes:

Boxed types provide static libraries that don't exist for the primitive types.
Primitive types can't be added to collections, but their associated boxed type can.
We'll discuss both of these points in more depth, but first, let's examine how these classes work in more detail.

Autoboxing  
Java will allow you to work with these classes as if they are primitive types. So for example, while we certainly can say:

Integer x = new Integer(7);

we are not required to, and can use the simpler syntax of primitives:

Integer x = 7;

We can also use arithmetic operators (+, - for example, or combined assignment operators like +=):

x += 5;

What's happening behind the scenes is that Java is automatically converting (boxing and unboxing) these types for us.

Here is an example of Java automatically boxing things for us. How nice!

List<Integer> numbers = new ArrayList<>();
int x = 5;
numbers.add(x); //This works even though x is an int and `numbers` expects Integers
Static Methods  
Each of these boxed type classes also has several static methods associated with it. The most common of these are the "parse" methods: Integer.parseInt(), Double.parseDouble() etc. These methods convert Strings (for example, from user input) into their "actual" types. Try running the example below.


1
import java.util.Scanner;
2
public class BoxedExample {
3
  public static void main (String[] args) {
4
    System.out.println("Do you want to ride the merry go round?");
5
    System.out.println("Please enter true or false.");
6
    String input = new Scanner(System.in).nextLine();
7
​
8
    //** The Boolean.parseBoolean() method
9
    boolean flag = Boolean.parseBoolean(input);
10
    if (flag) {
11
      System.out.println("Wheeeeee!!!");
12
    } else {
13
      System.out.println("Aw... Ok.");
14
    }
15
  }
16
}

Fullscreen

Reset Code
Run Code 
User input comes in as a String. Here we used the Boolean.parseBoolean() method to convert the user input String into what we wanted; a true or false value we could use in an if statement.

When To Use Boxed types  
As a general rule, only use boxed types when you have a reason to do so. For example, if you needed a list of numbers in your code, a List<int> is not possible - collections can't hold primitive types. If you don't have a reason to declare an Integer, declare an int.

---

# Enumerations

Let's say we are writing an Event class. Each Event occurs on a given day of the week, and we want to track that. What data type should we use? All of our existing data types have problems.

If we use a String, someone using our class can put whatever value they want in. We have to be concerned that they might use different capitalization or abbreviation ("Thurs" or "thursday" instead of the "Thursday" that we expect).

If we use an integer value, every time we want to actually "know" the day we'll need to use a cumbersome switch/case statement.

int dayOfWeek;
switch (dayOfWeek) {
  case 0:
    return "Sunday";
  case 1:
    return "Monday";
  //Etc etc
}
And again, there is nothing to prevent someone from entering a value out of the expected range. There are only seven days of the week. Introducing...

Enums!  
Java provides a great way for us to keep track of a variable that should only have a limited set of possibilities - like days of the week. It's called an enum short for "enumeration". Enums are great to use anytime you can list ALL of the possibilities for a value that your program cares about. Let's define an enum for days of the week. It's very simple!

public enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}
Reminder: by convention Java uses "UPPERSNAKECASE" to represent constant values, and everything in an enum is constant (TUESDAY is always TUESDAY and always will be).

Now we can define our Event class:

public class Event {
  Day dayOfWeek; //<- Our enum!
  String address;
  String eventManagerName;
  List<Person> eventAttendees;

}
Here is how we'd use that variable:

dayOfWeek = Day.MONDAY; //Assignment
if (dayOfWeek == Day.TUESDAY) {
  System.out.println("It's Tuesday!");
}
If you want to be able to quickly refer to your enum you can use a static import:

import static Day.*;
dayOfWeek = MONDAY; //Assignment
if (dayOfWeek == TUESDAY) {
  System.out.println("It's Tuesday!");
}
Enum Class  
Even though we are treating enums like simple types (no using "new" to assign a value) they are actually Objects. All enums extend the Enum class, which extends Object. So in addition to the standard toString() and equals() methods, we also have compareTo() (because Enum implements Comparable) and ordinal(), which tells us the number associated with the particular value. For the Day Enum that would look like this:

SUNDAY - 0
MONDAY - 1
TUESDAY - 2
Etc Etc
Because enums have these constant values, Java lets us use them in a switch/case statement.

Properties, Constructors, and Methods (Oh my!)  
Java Enums allow us to define properties, much like we can have variables in classes. Let's improve our Day enum to have a String status, which will have some comment about the day, and a boolean isWeekday.


1
public enum Day {
2
​
3
    //These values are defined using the constructor (see below)
4
    SUNDAY (false, "Some people go to church"),
5
    MONDAY (true, "Most don't like this day"),
6
    TUESDAY (true, "Better than Monday"),
7
    WEDNESDAY (true, "Hump day"),
8
    THURSDAY (true, "Almost Friday!"),
9
    FRIDAY (true, "Yay!"),
10
    SATURDAY (false, "The best day, and some go to temple");
11
​
12
    private boolean isWeekday; //These fields should be private
13
    private String status; //Just like a regular class's fields
14
​
15
    //Here we are declaring a constructor
16
    Day (boolean isWeekday, String status) {
17
        this.isWeekday = isWeekday;
18
        this.status = status;
19
    }
20
​
21
    //Accessor methods for properties
22
    public boolean isWeekday() {
23
        return isWeekday;
24
    }
25
​
26
    public String getStatus() {
27
        return status;
28
    }
29
​
30
    public boolean daysAdjacent (Day otherDay) {
31
        int difference = Math.abs(this.ordinal() - otherDay.ordinal());

Fullscreen

Reset Code
Run Code 
Notes:

Since we've defined a constructor we are now forced to define our enum constants with that constructor.
We want to make the variables private and use accessor methods for the same reason we would do that with a class (protected access)
We aren't limited to accessor methods. We've defined a daysAdjacent() method that will tell us if two days are adjacent (next to each other, like Tuesday and Wednesday).
Closing  
This example shows some very simple things that can be done with enums. But just like classes, you can make these as complicated as you need - the sky's the limit.

The Java tutorial contains another example of this with some interesting functions, and provides more formal definitions. I highly encourage you to read it here

---

# Recursion

Imagine that you are a sous-chef in a restaurant. The chef has asked you to cut up a large piece of meat into small cubes. You only have one regular knife, so there is no way you'll be able to make one cut and be done.

Instead, you'll have to slice the meat into progressively smaller pieces. Depending on how big of a piece of meat you started with and how small the cubes are supposed to be, you'll cut the large piece into medium pieces. Then, you'll cut the medium pieces into small pieces. You'd repeat this process until you have cubes of the desired size.

Recursion is a problem-solving technique whereby data is fed back into a process over and over again until the desired result is achieved.

Think about the meat example above. The same piece of meat is run through the cutting process over and over. Each time the meat gets run through the cutting process, each piece gets smaller. Eventually, each piece is the desired size, and the cutting process stops.

In Java, recursion can be achieved by having a function call itself. In this article, we'll discuss recursion and provide some examples.

Factorial  
The function factorial(x) calculates the product of all positive integers less than or equal to x. Factorials are a mathematical concept with a wide variety of applications in multiple fields.

For example:

factorial(5) = 5 * 4 * 3 * 2 * 1

factorial(5) = 120

factorial(6) = 6 * 5 * 4 * 3 * 2 * 1

factorial(6) = 720

Here is a non-recursive implementation of factorial:


1
public class SimpleRecursionExample {
2
    public static void main(String[] args) {
3
        for (int i = 0; i < 10; i++) {
4
            System.out.println(noRecursionFactorial(i));
5
        }
6
    }
7
    public static int noRecursionFactorial (int n) {
8
        int out = 1;
9
        for (int i = 1; i <= n; i++) {
10
            out *= i;
11
        }
12
        return out;
13
    }
14
}

Fullscreen

Reset Code
Run Code 
Note: factorials grow fast! Try playing around to see how large of a factorial we can calculate before overflowing our integer.

Before we solve this with recursion, let's briefly discuss why a recursive approach works. Here is our example from before:

factorial(5) = 5 * 4 * 3 * 2 * 1

factorial(5) = 120

factorial(6) = 6 * 5 * 4 * 3 * 2 * 1

factorial(6) = 720

If you compare factorial(5) and factorial(6) you can see that factorial(6) = 6 * factorial(5). So another way to define the factorial function would be:

factorial(x) = x * factorial(x - 1) where x is a positive integer. Hey - a function calling itself!

Now let's use recursion to re-write the factorial function:


1
public class RecursionExample{
2
    public static void main( String[] args ){
3
        for (int i = 0; i < 10; i++) {
4
           System.out.println( factorial( i ) );
5
        }
6
    }
7
    public static int factorial( int n ){
8
        if( n == 0 ){
9
            return 1;
10
        }
11
        return factorial( n - 1 ) * n;
12
    }
13
}

Fullscreen

Reset Code
Run Code 
That's great and all, but we don't need recursion to calculate factorials. So why do we need recursion?

Tree Search  
Let's say we have a Tree structure that is modeled by the following class:

public class MessageTree {

    private String message;
    private List<MessageTree> children;

    public MessageTree (String message, List<MessageTree> children) {
        this.message = message;
        this.children = children;
    }

    public String getMessage() {
        return message;
    }

    public List<MessageTree> getChildren() {
        return children;
    }
}
This forms a "Tree" because each MessageTree can have "children" that it owns. Each MessageTree object is a "Node" on the tree. We will define a specific instance as follows:

List<MessageTree> firstGroup = new ArrayList<>();

firstGroup.add(new MessageTree("1c of 1c", null));
firstGroup.add(new MessageTree("2c of 1c", null));
firstGroup.add(new MessageTree("3c of 1c", null));

List<MessageTree> secondGroup = new ArrayList<>();
secondGroup.add(new MessageTree("1c of 2c", null));
secondGroup.add(new MessageTree("2c of 2c", null));

List<MessageTree> thirdGroup = new ArrayList<>();
thirdGroup.add(new MessageTree("1c of 3c", null));
thirdGroup.add(new MessageTree("2c of 3c", null));
thirdGroup.add(new MessageTree("3c of 3c", null));

List<MessageTree> mainGroup = new ArrayList<>();
mainGroup.add(new MessageTree("1c", firstGroup));
mainGroup.add(new MessageTree("2c", secondGroup));
mainGroup.add(new MessageTree("3c", thirdGroup));

return new MessageTree("Root", mainGroup);
Let's suppose we want to search through this data structure to see if it contains a node with a particular message. If we know what the data structure looks like (i.e. the number of "levels," in this case, 3), we could write a function like this:

public static boolean containsMessage (String message, MessageTree root) {
  //First check the root
  if (root.getMessage().equals(message)) {
    return true;
  } else {
    //If it's not in the root, check root's children
    for (MessageTree child : root.getChildren()) {
      if (child.getMessage().equals(message)) {
        return true;
      }
    }
    //If it's not in root's children check grandchildren
    for (MessageTree child : root.getChildren()) {
      for (MessageTree grandchild : child.getChildren()) {
        if (child.getMessage().equals(message)) {
          return true;
        }
      }
    }
    return false;
  }
}
This algorithm is already looking messy, and it only covers a tree exactly three levels deep. If we want a solution that works on any size tree, we will need a recursive search algorithm.

Why is this problem best solved with a recursive algorithm? The data structure is arbitrarily deep. We can't write a search algorithm for each possible tree structure.


1
import java.util.ArrayList;
2
import java.util.List;
3
​
4
public class RecursionExample {
5
​
6
    public static void main(String[] args) {
7
        MessageTree root = buildTree();
8
        System.out.println(searchTree("3c of 3c", root));
9
        System.out.println(searchTree("1c", root));
10
        System.out.println(searchTree("Foo", root));
11
    }
12
​
13
    public static boolean searchTree (String message, MessageTree root) {
14
        if (root.getMessage().equals(message)) {
15
            return true;
16
        }
17
        if (root.getChildren() != null) {
18
            for (MessageTree child : root.getChildren()) {
19
                if (child.getMessage().equals(message)) {
20
                    return true;
21
                }
22
            }
23
            for (MessageTree child : root.getChildren()) {
24
                if (searchTree(message, child)) {
25
                    return true;
26
                }
27
            }
28
        }
29
        return false;
30
    }
31
​

Fullscreen

Reset Code
Run Code 
Note: if we are lucky enough to have access to the MessageTree class, we could use the following (slightly more elegant) solution:

public class MessageTree {
    //...
    public boolean contains (String message) {
        if (this.message.equals(message)) {
            return true;
        }
        if (children != null) {
            for (MessageTree child : children) {
                if (child.contains(message)) {
                    return true;
                }
            }
        }
        return false;
    }
}
This solution shows a method we have written for the MessageTree class that will recursively search its Tree! Pretty nifty.

Call to Action: (Optional) Re-write the MessageTree class using Generics. This is an extra that we haven't covered in detail, but you can read about them here.

Exiting and Stack Overflow  
When writing recursive functions, it's important to make sure that they will exit. If your recursive function calls itself too many times, you will encounter a StackOverflowError before long.


1
public class Overflow {
2
  static numCalls;
3
  public static void main (String[] args) {
4
    myFunction();
5
  }
6
​
7
  public static void myFunction () {
8
    myFunction();
9
    //There is no condition under which this function *won't* recursively call itself
10
  }
11
}

Fullscreen

Reset Code
Run Code 
Conclusion  
Recursion is an advanced programming topic and isn't strictly necessary in most cases. However, once you understand the fundamental principle of recursion and learn to recognize it in code, then it can be a powerful tool that makes your code simpler to understand and easier to write.
