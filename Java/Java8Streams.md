# Java 8 Streams

Let's imagine that we have an abstract data structure and want to query it. This data structure contains Robot object data. Here is the Robot class:

class Robot {
    private String name;
    private int liftCapacity;
    private int processingPower;
    private boolean isPrototype;
    private boolean isFunctional;
    //Constructor and accessor methods omitted
I want a list of all "functional" robots that can lift at least 50 pounds. If the data is in a SQL database we could write a query like this:

SELECT * FROM robots WHERE isFunctional = true AND liftCapacity >= 50

Or, if we already have a List<Robot> we could loop through it as follows:

List<Robot> strongFunctionalBots = new ArrayList<>();
for (Robot robot : robots) {
  if (robot.isFunctional() && robot.getLiftCapacity() >= 50) {
    strongFunctionalBots.add(robot);
  }
}
Either of these approaches can work depending on the type of data (whether we are dealing with a SQL database or an existing list).

Episode VIII - A New Approach  
Java 8 introduces a new way of working with data called Streams (not related to InputStream, OutputStream, etc.). Streams have some advantages over regular iteration that we'll discuss later. For now, let's take a look at an example with the Robot class we mentioned earlier.


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
        List<Robot> robots = makeRobots();
8
        System.out.println("Functional Robots that can lift 50 or more pounds:");
9
        robots.stream()
10
            .filter(e -> e.isFunctional())
11
            .filter(e -> e.getLiftCapacity() >= 50)
12
            .forEach(e -> System.out.println("\t" + e.getName()));
13
​
14
    }
15
​
16
    public static List<Robot> makeRobots () {
17
        List<Robot> robots = new ArrayList<>();
18
        robots.add(new Robot("BobBot", 10, 500, false, true));
19
        robots.add(new Robot("SallyBot", 20, 400, true, true));
20
        robots.add(new Robot("TommyBot", 5, 1000, true, false));
21
        robots.add(new Robot("MarkBot", 500, 10, false, true));
22
        robots.add(new Robot("CathyBot", 25, 10, false, false));
23
        robots.add(new Robot("JamesBot", 1, 950, false, true));
24
        robots.add(new Robot("AmazeBot", 800, 1250, true, true));
25
        robots.add(new Robot("LameBot", 1, 5, false, true));
26
        robots.add(new Robot("JohnBot", 20, 20, false, false));
27
        robots.add(new Robot("LiftBot", 90, 50, true, false));
28
        return robots;
29
    }
30
}
31
class Robot {

Fullscreen

Reset Code
Run Code 
Let's take a closer look at the stream() functions we've used.

robots.stream()
    .filter(e -> e.isFunctional())
    .filter(e -> e.getLiftCapacity() >= 50)
    .forEach(e -> System.out.println("\t" + e.getName()));
Each Stream consists of the following elements:

An initial .stream() call.
Zero or more "intermediate" operations - in our example, the two filter() calls.
A terminal operation - in our example, the forEach() call.
The initial .stream() call on the collection starts the process. After that, we can have any number of intermediate operations. Every intermediate operation will return a Stream, which continues the pipeline. Finally, the terminal operation will return a non-Stream result - in the case of forEach, no return.

(Reminder of Lambda Syntax: we can read e-> System.out.println("\t" + e.getName()) as an anonymous function that says, "Given 'e' I will call System.out.println() with e's getName()")

So, this series of calls:

Starts a stream
Filters the stream, removing robots that aren't "functional" (insert bad pun here)
Filters the stream again, removing robots that can't lift 50 pounds
For each remaining element in the Stream, calls the System.out.println() method to print the name of the robot.
Let's look at another interesting example of the work Streams can do for us.


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
        List<Robot> robots = makeRobots();
8
        int total = 0;
9
​
10
        System.out.println("Total lift capacity of functional bots:");
11
        total = robots.stream()
12
                .filter(e -> e.isFunctional())
13
                .mapToInt(e -> e.getLiftCapacity())
14
                .sum();
15
        System.out.println(total);
16
​
17
        System.out.println("Total lift capacity of non-functional bots:");
18
        total = robots.stream()
19
                .filter(e -> !e.isFunctional())
20
                .mapToInt(e -> e.getLiftCapacity())
21
                .sum();
22
        System.out.println(total);
23
​
24
        System.out.println("Total lift capacity of all bots");
25
        total = robots.stream()
26
                .mapToInt(e -> e.getLiftCapacity())
27
                .sum();
28
        System.out.println(total);
29
    }
30
​
31
    public static List<Robot> makeRobots () {

Fullscreen

Reset Code
Run Code 
Here we are using the mapToInt() function to collect the lift capacities of the robots we're interested in. mapToInt() returns an IntStream, which is a special type of Stream that contains (you guessed it) Integers. We then use the terminal function .sum() to sum up that IntStream and get the total.

Method Reference Syntax  
You may encounter Method References in Streams. Take a look at the example below (line 10):


1
import java.util.*;
2
public class MethodReferenceExample  {
3
  public static void main (String[] args) {
4
    List<String> messages = new ArrayList<>();
5
    messages.add("Hello World");
6
    messages.add("Today we'll learn about the Method Reference");
7
    messages.add("You will see it's not that bad!");
8
​
9
    messages.stream()
10
        .forEach(System.out::println);
11
  }
12
}

Fullscreen

Reset Code
Run Code 
forEach(System.out::println) is equivalent to forEach(e -> System.out.println(e)) The double colon (::) is the method reference.

Why Use Streams  
Why should we be interested in using Streams? First, it's important to understand all the syntax of the language. Even if you choose not to use Streams, you should be able to see them in others' code and understand what they do.

Streams are beneficial for a few reasons:

Functional Programming - functional programming is somewhat "in vogue" right now, and Streams are a functional programming approach. Using them will help us understand the techniques of functional programming.

Code Readability - Streams can help our code to be more readable because they establish and follow patterns. The method names provide some built-in "documentation" that shows a reader familiar with Streams exactly what we are trying to do. (For example, if we are calling the filter() method presumably it is because we want to filter our data).

Performance - Streams provide support for parallelism, which means that Stream tasks can easily be broken up and handled by different CPU cores, then put back together. There are other performance benefits of Streams as well.

Conclusion  
Streams are a powerful feature that can simplify your codebase and increase readability. As you work on your programs, don't worry so much about when they are appropriate. Code up your solutions and then think about using streams to refactor your code and simplify it.

If you find yourself filtering collections with loops over and over again, you may want to consider using streams. If you need to perform any sequential operations on streams and want to produce outputs, then consider using streams.

---

# Java 8 Stream Map Transpose

We have seen examples of using Java 8 Streams to filter and perform basic tasks (such as printing to the console) on data. Streams also allow us to change the type of data that we are working with. We've previously seen the mapToInt() function. The map() function allows us to convert to any type we need.

In the example below, we have three classes representing origami objects: PaperCrane, PaperSnake, and PaperTiger. They each can be "folded" into another shape. (PaperCrane has a constructor that accepts a PaperTiger, and so on). There's currently no data or behavior associated with these origami classes. Feel free to add your own.


1
import java.util.ArrayList;
2
import java.util.List;
3
import java.util.stream.Collectors;
4
​
5
public class OrigamiExample {
6
​
7
    static List<PaperCrane> paperCranes = new ArrayList<>();
8
​
9
    public static void main(String[] args) {
10
        paperCranes.add(new PaperCrane());
11
        paperCranes.add(new PaperCrane());
12
​
13
        List<PaperSnake> paperSnakes = paperCranes.stream()
14
                .map(e -> new PaperSnake(e))
15
                .collect(Collectors.toList());
16
​
17
        List<PaperCrane> originalList = paperSnakes.stream()
18
                .map(e -> new PaperTiger(e))
19
                .map(e -> new PaperCrane(e))
20
                .collect(Collectors.toList());
21
    }
22
}
23
​
24
class PaperCrane {
25
​
26
    public PaperCrane () {
27
        System.out.println("Folded a Paper Crane");
28
    }
29
​
30
    public PaperCrane (PaperTiger pt) {
31
        System.out.println("Folded a Paper Tiger into a Paper Crane");

Fullscreen

Reset Code
Run Code 
Optional Exercise: Create a new class PaperHippo that takes a paperSnake as input for it's constructor function. Create a new list in the main function to hold PaperHippos and stream your list of PaperSnakes to create the Hippos.

Another Example  
Let's say we have an array of Person objects, and we want to extract a property from them (for this example, their e-mail address).


1
import java.util.Arrays;
2
import java.util.List;
3
import java.util.stream.Collectors;
4
​
5
public class Map8Example {
6
​
7
    public static void main(String[] args) {
8
        Person[] people = new Person[4];
9
        people[0] = new Person("susie@gmail.com");
10
        people[1] = new Person("bob@comcast.net");
11
        people[2] = new Person("sneaky@ninja.net");
12
        people[3] = new Person("jeanlucpicard@starfleet.org");
13
​
14
        List<String> emails = Arrays.stream(people)
15
                .map(e -> e.getEmail())
16
                //Or use alternative syntax using method reference
17
                //.map(Person::getEmail)
18
                .collect(Collectors.toList());
19
​
20
        System.out.println(emails);
21
    }
22
}
23
​
24
class Person {
25
    private String email;
26
    String username;
27
    String password;
28
​
29
    public Person (String email) {
30
        this.email = email;
31
    }

Fullscreen

Reset Code
Run Code 
The two different syntax options for the map() function used above are interchangeable. The first (non-commented out) uses the "traditional" lambda syntax, whereas the second uses the method reference syntax. They are interchangeable here.

Summary  
The map() function allows us to change the type of the data we're working with in any way. Unlike mapToInt() it is not limited in the types it can handle (mapToInt() returns an IntStream, so the associated lambda function must return integer values).
