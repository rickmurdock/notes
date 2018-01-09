# Static Keyword

Static fields and methods are attached to the class and not the objects instantiated from a class. Each instance doesn't get a copy of anything marked static. Instead, all instances share static field or method and effect it universally.

This article discusses the static keyword.

## Understanding Static Fields

To understand the static keyword, it's easier to understand what "non-static" means: "non-static" fields get attached to class instances. In the following example, int processingSpeed and boolean isLaptop are both non-static variables. Whenever an object is instantiated from the Computer class, that instance gets a copy of those fields.

class Computer {
    int processingSpeed;
    boolean isLaptop;

    public Computer (int processingSpeed, boolean isLaptop) {
        this.processingSpeed = processingSpeed;
        this.isLaptop = isLaptop;
    }
}
public class ComputerApplication{
    public static void main (String[] args)
        Computer fastLaptop = new Computer(5000, true);
        Computer slowDesktop = new Computer(800, false);
    }
}
slowDesktop.processingSpeed is entirely independent from fastLaptop.processingSpeed. If I change fastLaptop.processingSpeed, slowDesktop.processingSpeed is unaffected.

In contrast, static variables belong to the class. A classic example of this would is to count the total number of Computers that have instantiated.


1
public class Computer {
2
    int processingSpeed;
3
    boolean isLaptop;
4
    static int numComputersMade;
5
​
6
    public Computer (int processingSpeed, boolean isLaptop) {
7
        this.processingSpeed = processingSpeed;
8
        this.isLaptop = isLaptop;
9
        numComputersMade++;
10
    }
11
​
12
    public static void main (String[] args) {
13
        Computer myComputer = new Computer(5000, true);
14
        Computer slowComputer = new Computer(800, false);
15
        Computer otherComputer = new Computer(4000, false);
16
​
17
        System.out.println(myComputer.numComputersMade);
18
        System.out.println(slowComputer.numComputersMade);
19
    }
20
}

Fullscreen

Reset Code
Run Code 
Static Methods  
You've already encountered the static method in Java. The main method of any application is static.

public static void main (String[] args){}
The static keyword can also be applied to other methods. Just like fields, static methods don't relate to a particular instance of a class. They belong to the class as a whole. An important consequence of this is that we cannot reference non-static fields inside of a static method without an object instance.


1
public class Computer {
2
    int processingSpeed;
3
    boolean isLaptop;
4
    int numComputersMade;
5
​
6
    public Computer (int processingSpeed, boolean isLaptop) {
7
        this.processingSpeed = processingSpeed;
8
        this.isLaptop = isLaptop;
9
        numComputersMade++;
10
    }
11
​
12
    public static void main (String[] args) {
13
​
14
    }
15
​
16
    public static void doSomething () {
17
        this.processingSpeed = 5; //<- Compilation error
18
        //Java is going to tell us we can't reference a non-static variable here
19
        //But if we have an object we can work with it normally
20
        Computer someComputer = new Computer(1000, false);
21
        someComputer.processingSpeed = 5; //This works fine
22
    }
23
}

Fullscreen

Reset Code
Run Code 
When to Use Static Methods/Variables  
The circumstances of what you're trying to do will determine if you need to use static variables/methods or not. In general, it's always possible to write a method as static. The example below illustrates this with a function that checks to see if a computer is "fast enough" to perform a task.

public class Computer {
    int processingSpeed;

    //Non-static method
    public boolean fastEnough (int requiredSpeed) {
        if (this.processingSpeed >= requiredSpeed) {
            return true;
        }
        return false;
    }

    //Static version
    public static boolean fastEnough (Computer computer, int requiredSpeed) {
        if (computer.processingSpeed >= requiredSpeed) {
            return true;
        }
        return false;
    }
}
As a rule of thumb, try to make methods non-static if possible. Only make methods static if they "really are" i.e. they don't have anything to do with a particular instance of the class. The example above violates this rule because we're passing in that particular instance to the method. The advantage of static methods is that they CAN be accessed without an instance of the class, which can be important.

Conclusion  
We are now in a better position to understand public static void main (String[] args). Namely, the main() method does not relate to a specific instance of the Computer class. We don't need to create a Computer object to run main(). (If you think about it, main() must be static because when it's called, we couldn't possibly have an instance of Computer - our program just started).

---

# Exceptions and Control Flow  
What is Exception Handling? Essentially, it is a form of control flow (like if statements, loops, etc.) that allows us to decide how our program should react to various circumstances. For example, let's say we need the user to enter a number. But what if they don't enter a number? Try running the code below and inputting something other than a number. "Yellow" for example.


1
public class Main {
2
  public static void main (String [] args) {
3
    System.out.println("Please enter a number.");
4
    String input = System.console().readLine();
5
    int userNumber = Integer.parseInt(input);
6
    System.out.println("You entered " + userNumber);
7
  }
8
}

Fullscreen

Reset Code
Run Code 
You should see that a NumberFormatException is thrown (assuming you did not enter a number). Exception handling allows us to decide what happens in this "error circumstance" - the user did not behave as expected. In the example below, you will see that (if you do not enter a number) the line System.out.println("You entered " + userNumber) does not run. This is because as soon as the exception is thrown (in this case from the Integer.parseInt(String) call) the JVM will skip to the appropriate catch block (if one exists).


1
public class Main {
2
    public static void main (String [] args) {
3
        System.out.println("Please enter a number.");
4
        String input = System.console().readLine();
5
        try {
6
            int userNumber = Integer.parseInt(input);
7
            System.out.println("You entered " + userNumber);
8
        } catch (NumberFormatException ex) {
9
            System.out.println("You did not enter a number. Goodbye.");
10
        }
11
    }
12
}

Fullscreen

Reset Code
Run Code 
The Syntax  
Let's lay out the basic syntax of exception handling. We will go into more details and examples later in the lesson.

try {
    //Code to attempt that might throw an exception
    //This block could contain nested try/catch blocks
} catch (<type of exception you want to catch>) {
    //Code to execute if <type> exception is thrown
    //Often includes exception.printStackTrace()
} //Potentially more catch blocks
finally {
    //Code to execute at the end of the block
    //Usually used to clean up resources
}
Using Multiple Catch Blocks  
It's possible to have try/catch blocks with multiple catch statements. We'll see an example of that next.

"FizzBuzz" is a common early programming challenge that you may have encountered before. The challenge is as follows: write a program that prints out the numbers 1-100, but if the number is divisible by three, print "Fizz" instead. If it's divisible by five, print "Buzz", and if it's divisible by both three and five print "FizzBuzz". Here is a standard solution for this problem for comparison.


1
public class FizzBuzzNoEx {
2
    public static void main(String[] args) {
3
        for (int index = 1; index <= 100; index++) {
4
            if (index % 15 == 0) {
5
                System.out.println("FizzBuzz");
6
            } else if (index % 3 == 0) {
7
                System.out.println("Fizz");
8
            } else if (index % 5 == 0) {
9
                System.out.println("Buzz");
10
            } else {
11
                System.out.println(index);
12
            }
13
        }
14
    }
15
}

Fullscreen

Reset Code
Run Code 
Now let's solve this challenge using Java's Exception handling. We will start out by defining three new types of exceptions for use in our project; DivBy15, DivBy3, and DivBy5. To define our own custom exceptions all we have to do is extend the Exception class. If we needed to, we could add constructors or additional functionality to these special exceptions.

class DivBy3 extends Exception{
}
class DivBy5 extends Exception {
}
class DivBy15 extends Exception {
}

1
public class FizzBuzzExceptions {
2
​
3
    public static void main(String[] args) {
4
        for (int index = 1; index <= 100; index++) {
5
            try {
6
                if (index % 15 == 0) {
7
                    //The "throw" keyword allows us to throw our own exceptions
8
                    throw new DivBy15();
9
                }
10
                if (index % 3 == 0) {
11
                    throw new DivBy3();
12
                }
13
                if (index % 5 == 0) {
14
                    throw new DivBy5();
15
                }
16
                //We will only get here if the number is not a multiple of 3 or 5
17
                System.out.println(index);
18
            } catch (DivBy15 ex) {
19
                System.out.println("FizzBuzz");
20
            } catch (DivBy3 ex) {
21
                System.out.println("Fizz");
22
            } catch (DivBy5 ex) {
23
                System.out.println("Buzz");
24
            } catch (Exception ex) {
25
                System.out.println("Some other kind of exception occured.");
26
                //This should never be called
27
            }
28
        }
29
    }
30
}
31
class DivBy3 extends Exception{

Fullscreen

Reset Code
Run Code 
Note: the specific order we chose for our custom exceptions does not matter because none of them inherit from each other (they all extend Exception). However, if you move the last catch block to the top and try to run the code, you will see a compiler error. Because all of our custom exceptions extend Exception, this means they ARE Exceptions, and would be caught by the "generic" Exception block if it were first.

Also note that this is not really a good use case for exception handling and is only an example. In general, an exception should only be thrown as the result of some unexpected/error-type circumstance.

Just When You Thought It Was FINALLY Over...  
There's one last piece of the puzzle. try/catch blocks can have a third section, finally. This defines a block of code that will always run after the try/catch block. Let's look at the syntax and basic control flow first.


1
public class FinalTime {
2
    public static void main (String[] args) {
3
        try {
4
            System.out.println("We're in the try block.");
5
            String s = null;
6
            //s.toUpperCase();
7
        } catch (NullPointerException ex) {
8
            System.out.println("Uh oh, null pointers are bad.");
9
        } finally {
10
            System.out.println("Finally, we're done!");
11
        }
12
    }
13
}

Fullscreen

Reset Code
Run Code 
Call To Action: Experiment with the above code by uncommenting line 6 (s.toUpperCase()). This will cause a NullPointerException to be thrown (remember, this happens any time we attempt to use dot notation on a null object). You can see that the finally block executes regardless of whether an exception is thrown or not.

Now that we've covered the basic syntax and flow, let's look at an example where we would actually want to use finally.

PrintWriter printWriter = null;
try {
    printWriter = new PrintWriter("stuff.txt");
} catch (IOException ex) {
    ex.printStackTrace();
} finally {
    if (printWriter != null) {
        printWriter.close();
    }
}
The main use for a finally block is to clean up resources. We always want to clean up after ourselves by closing resources we're not using. The finally block guarantees that the PrintWriter will be closed at the end of our block.

Exceptions Without Try/Catch  
So far we've seen examples of functions that throw exceptions, like the PrintWriter constructor above. It throws a FileNotFoundException, which is caught by the more general IOException - FileNotFoundException extends IOException. We've also seen examples of direct throw statements (if (index % 5 == 0) {throw new DivBy5();}). All of our examples except the first one have been using try/catch. It's important to understand what happens when an exception is NOT caught: the exception is thrown up.


1
public class ThrowUp {
2
​
3
    public static void main (String[] args) {
4
        helperFunction1();
5
    }
6
​
7
    public static void helperFunction1 () {
8
        helperFunction2();
9
    }
10
​
11
    public static void helperFunction2 () {
12
        int x = 0/0;
13
    }
14
}

Fullscreen

Reset Code
Run Code 
In this example, we have main() calling helperFunction1() which calls helperFunction2(). Unfortunately, helperFunction2() generates an ArithmeticException because we are dividing by zero (an undefined operation). This exception isn't caught anywhere in helperFunction2(), so it's thrown up to the calling function to deal with (helperFunction1()). In this case, that function doesn't catch the exception either! Sad days. The exception is thrown up again, finally to main(), which still doesn't know how to deal with the exception. At this point, the exception is thrown up to the JVM, which prints the stack trace (all of the methods that were involved in this chain of events).

Try With Resources  
Java 7 has added a new "twist" on try statements known as "try with resources". You can now pass in a parameter to the try statement, just as you would to an if statement. Instead of the try parameter being a boolean (like with an if/while statement) the parameter must be a "resource" that can be closed. More formally, the parameter must implement AutoCloseable - this will apply to all the resources we use to read and write files.

try (PrintWriter printWriter = new PrintWriter("stuff.txt")) {
    //Do things with printWriter
} catch (IOException ex) {
    ex.printStackTrace();
}
The try-with-resources statement will close() the resource after the try/catch block is finished executing. This is a great way to use resources because you will never accidentally forget to close them.

## Conclusion

We've discussed the ins and outs of exception handling in Java. More than the specific syntax, think of exception handling as a way to determine how your program behaves when things don't go as you expect - analogous to the errorCallBack() functions you've defined in JavaScript.

---

# Using Lists

It's important and useful to be able to organize our data into collections. There will be a later lesson that explains all the details. This lesson will cover how to use the most basic collection: Lists.

Creating a new List  
Let's say we want the user to type in several "messages" and we want to store these messages somewhere.

List<String> messageList = new ArrayList<>();
We've declared a new variable here and initialized it. The <String> token after List tells us the type of variable that will be stored in the List. Lists are type safe, so you will only be able to add Strings to the list, and anything you retrieve from it will be a String.

Adding Elements  
To add something to the List we use the add() method:

String myMessage = "Lists are super useful!";
//All of these work to add new messages
messageList.add(myMessage);
messageList.add("Also they are fun!");
messageList.add(new String("And great!"));
messageList.add(5); //<- Compiler error! 5 is not a String. (Though "5" is)
Retrieving Elements  
We can retrieve individual elements from the list by index. So for example, to get the first element, we would do:

String retrieved = messageList.get(0);
If we haven't added anything to the list this would cause an error (an IndexOutOfBoundsException).

Removing Elements  
To remove an element, we can use the remove(int index) or remove(Object o) methods:


1
import java.util.List;
2
import java.util.ArrayList;
3
​
4
public class ListXample {
5
​
6
    public static void main (String[] args) {
7
        String firstMessage = "Test message";
8
        String secondMessage = "?????";
9
        List<String> messageList = new ArrayList<>();
10
        messageList.add(firstMessage);
11
        messageList.add(secondMessage);
12
​
13
        System.out.println("messageList has " + messageList.size() + " elements");
14
​
15
        messageList.remove(firstMessage);
16
        messageList.remove(0); //This works because 2nd message moved up
17
​
18
        System.out.println("messageList has " + messageList.size() + " elements");
19
    }
20
}

Fullscreen

Reset Code
Run Code 
Iterating Over the List  
To iterate over the list we will want to use a for-each loop.

for (String message : messageList) {
    System.out.println(message);
}
Putting It All Together  

1
import java.util.Scanner;
2
import java.util.ArrayList;
3
import java.util.List;
4
​
5
public class ListXample {
6
​
7
    public static void main (String[] args) {
8
        Scanner inputScanner = new Scanner(System.in);
9
        List<String> messageList = new ArrayList<>();
10
        for (int i = 0; i < 3; i++) {
11
            System.out.println("Please type in a message.");
12
            messageList.add(inputScanner.nextLine());
13
        }
14
​
15
        System.out.println("Printing all messages now:");
16
        for (String message : messageList) {
17
            System.out.println(message);
18
        }
19
    }
20
}

Fullscreen

Reset Code
Run Code 
