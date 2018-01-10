# Methods  

You should be familiar with how methods work in JavaScript. Methods in Java work similarly - the big difference being that because Java is a strongly-typed language, it enforces a lot more structure.

## Method Arguments  

Methods can accept arguments (parameters) in Java just like in JavaScript. The difference is that Java requires us to specify the type.

```java
//JavaScript
function shoutGreeting(firstName, lastName) {
    return "HELLO " + firstName.toUpperCase() + " " + lastName.toUpperCase() + "!!!!"
}

//Java
public static String shoutGreeting (String firstName, String lastName) {
    return "HELLO " + firstName.toUpperCase() + " " + lastName.toUpperCase() + "!!!!";
}
```

In JavaScript, the parameters `firstName` and `lastName` are *intended* to be Strings - but in reality, they could be something else. In Java, we specify the type: they must be Strings. The method can't be called without two Strings as inputs (the compiler would not allow the call). Try to run the code below.

```java
public class Example {

    public static void main (String[] args) {
        int x = 5;
        String y = "hello";
        shoutGreeting(x, y);
    }

    public static String shoutGreeting (String firstName, String lastName) {
        return "HELLO THERE " + firstName.toUpperCase() + " " + lastName.toUpperCase() + "!!!!";
    }
}
```

You should see any error message to the effect of `incompatible types: cannot convert int to String`. This illustrates a feature of type safety: we know that variables will be of the expected type, so we don't need to worry that we're calling `toUpperCase()` on something that's not a String. Note that method arguments can still be `null`:

```java
public class Example {

    public static void main (String[] args) {
        shoutGreeting(null, null);
    }

    public static String shoutGreeting (String firstName, String lastName) {
        return "HELLO THERE " + firstName.toUpperCase() + " " + lastName.toUpperCase() + "!!!!";
    }
}
```

We wouldn't want things to get too safe after all!

## Return Value  

Again, you are already familiar with the concept of a function returning something from JavaScript. And once again, Java imposes a few more rules. First, all methods must have a return type. If you don't want your function to return a value, you can declare the return type as `void`.

```java
public void printMessage (String message) { //void return type
    System.out.println(message);
}
```

If you specify any return type other than `void`, Java will be **very** insistent that you are returning something of this kind. Run the code below, and you should see a compilation error to the effect of `missing return statement`.

```java
public class Example {

    public static String makeMessage () {
        System.out.println("I don't do what I'm supposed to! I'm sassy!");
    }
}
```

Here is another example of Java's insistence that you return something. The `compare()` method compares two `ints`. `compare()` returns `true` if the first number is larger than the second. It also returns `true` if the two numbers are equal. Otherwise, `compare()` returns `false`.

However, if you try to run the code below, Java will **still** tell you you're missing a return statement.

```java
public class Example {

    public static void main(String[] args) {
      int x = 5;
      int y = 8;
      compare(x, y);
    }

    public static boolean compare (int x, int y) {
        if (x > y) {
            return true;
        }
        if (y > x) {
            return false;
        }
        if (y == x) {
            return true;
        }
    }
}
```

Even though logically we can see that this method would always return a value (if the two numbers aren't equal, one of them must be greater than the other), Java can't see that. You can fix the code above by changing the three `if` statements to `if`/`else if`/`else` - this makes Java happy.

**Call To Action**: Fix the code above using if/else statements.

Sometimes when writing a method, you know a portion of your code will return something, but there's no easy way to convince Java of that fact. To fix this, you can add a `return null` or `return 0` or `return ""` at the end of the method (depending on type) to satisfy the compiler. Just make sure that your "real" return block works correctly.

```java
public class Example {
    public static void main(String[] args) {
      int x = 5;
      int y = 8;
      compare(x, y);
    }

    public static boolean compare (int x, int y) {
        if (x > y) {
            return true;
        }
        if (y > x) {
            return false;
        }
        if (y == x) {
            return true;
        }
        return false; //This line should never be reached
    }
}
```

## Conclusion  

Methods are a fundamental part of Java, and though similar to JavaScript require more strict adherence to rules. Luckily, Java will let you know when you've made a mistake! Errors exist to help you understand what is wrong with your code. As you develop your understanding of functions and methods, pay close attention to the errors that you generate. They will help you debug the structure and logic of your code.

---

# Output Text to the Command Line  

Printing to the console can be very useful when programming. It allows us to see certain information about our program. For instance, if a variable is constantly changing because it's in a method, then using `System.out` can show us the progress of that variable.

In the following example, you'll see a lot of code. However, the only line that prints something to the console is `System.out.println(x)`;

Run the code below to see an example of this:

```java
public class Example {
  public static void main(String[] args) {
    for (int i = 1; i < 11; i++) {
      int x = 5 * i;
      System.out.println(x);
    }
    // I'm multiplying the number 5 incrementally from 1 through 10
  }
}
```

As you can see, `int x` is increasing by five each time we run through this `for` loop. It would be challenging to track the progress of `int x` if we didn't use a `System.out` to print it to the console for us.

`System` is a final class from the `java.lang` package. `out` is a class variable of type `PrintStream` declared in the System class. `println` and `print` are methods of the `PrintStream` class.

## System.out  

`System.out.print()` - This method will print its contents to the console.

`System.out.println()` - This method will print its contents to the console and start a new line.

## Conclusion  

You will see `print()` and `println()` a lot in our code samples. Whenever you see it, you can expect something to print to the screen.

---

# Input from The Command Line  

Programs get run from the command line. These programs usually interact with the user from the command line environment. Java supports this kind of interaction in two ways: through the `Standard Streams` and the `Console`. For now, we'll talk about `Standard Streams`.

## Standard Streams  

`Standard Streams` are a feature of many operating systems. By default, they **read input** from the keyboard and **write output** to the display. They also support I/O on files and between programs, but the command line interpreter controls those features, not the program.

The Java platform supports one `Standard Stream` called `Standard Input`, accessed through `System.in`. This `Standard Stream` is defined automatically and is available anywhere.

## Scanner  

The `Scanner` class allows us to intercept user input in the command line, capture it in a variable and then use it in our program. It's incredibly useful for creating interactive command line tools.

Below is an example of `Standard Input` using the `Scanner` class. Read through the comments to follow the details about what is happening. In general, the logic falls into a few steps.

1. `Scanner` is being imported

2. The program prints out a prompt

3. An instance of `Scanner` is created to capture `System.in`

4. The `Scanner` instance function `nextLine()` is used to wait for user input. That string data gets stored in a new variable `userInput`

5. `userInput` is concatenated with another string and printed to the console.

```java
// 1. Import Scanner
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {

        // 2. The program prints out a prompt
        // Here the system is printing to the console
        System.out.println( "Hello. What is your name?" );

        // 3. An instance of `Scanner` is created to capture `System.in`
        // I'm creating a variable of type Scanner and naming it scanner
        Scanner scanner = new Scanner(System.in);

        // 4. The `Scanner` instance function `nextLine()`
        // is used to wait for user input.
        // userInput is a string that has the value of
        // the input from the command line. What happens here is
        // the program stops and waits for there to be input from the command line.
        // It will not continue running until it has received that input
        // and the user has pressed "return"
        String userInput = scanner.nextLine();

        // 5. `userInput` is concatenated with another string and printed to the console.
        // using concatenation, I combine a hard-coded String
        // with the userInput String and print to the command line
        System.out.println("It's nice to meet you, " + userInput);
    }
}
```

## Conclusion  

By using the `Scanner` class and `System.in`, we can to take input from the command line and use it in our code! We'll expand on this later, and eventually you will understand all the pieces, but for now, it's useful to know that we can get input from the user.

---

# Java & JavaScript  

It's easy to assume that because JavaScript has “Java” in its name that it's somehow related to Java. Most programmers groan at this comparison, and many feel that the name confusion is part of a marketing gimmick. However, the history of the two programming languages did intersect for a brief moment in time during the early days of Netscape. The project was originally called Mocha, then renamed to LiveScript, and finally to JavaScript when Netscape and Sun did a license agreement. The idea at the time was to make it a scripting language complimentary to Java. Instead, the two languages took wildly different paths.

Let's compare them.

## Compiled vs. Interpreted  

Java code is typically written in an Integrated Development Environment (IDE) and compiled into bytecode. This bytecode is not very readable by humans - it's the job of the Java Virtual Machine (JVM) to run it. JavaScript code gets executed by a JavaScript engine in a web browser. Making changes in Java application environments can require several steps using your IDE to compile and deploy the changes. Making changes to JavaScript can be done with just a simple text editor.

## Debugging  

Java gets compiled before it will run. If there are any structural problems with code, then it becomes apparent very quickly. Once it is running, IDEs enable the developer to attach to the JVM to debug in real-time. JavaScript doesn't get compiled, so all bugs are found at runtime. As such, the debugging capabilities of JavaScript are highly dependent on the execution environment, and this can vary quite a bit. Java is far better at capturing bugs and has better tools for debugging.

### Examples  

A simple function in JavaScript:

```javascript
function shoutGreeting( firstName, lastName ){
    return "HELLO THERE " + firstName.toUpperCase() + " " + lastName.toUpperCase() + "!!!!"
}
console.log( shoutGreeting( "Angela", "Smith" ) );
```

The same function in Java:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println(shoutGreeting("Angela", "Smith"));
    }

    public static String shoutGreeting (String firstName, String lastName) {
        return "HELLO THERE " + firstName.toUpperCase() + " " + lastName.toUpperCase() + "!!!!";
    }
}
```

As you can see, there are a lot of similarities between the two languages:

* Both languages execute line-by-line

* The naming conventions are similar (camel case for method and variable names)

* Both use C-style syntax; methods are invoked using parentheses. Methods of objects are accessible using dot notation (for example the String's `toUpperCase()`)

There are also differences:

* Java is strongly typed. We have to explicitly decide the types of the variables `firstName` and `lastName`. (More on this below)

* Java even forces us to declare return types for functions. In the case of a function with no return value, (the `main()` method for example) we must declare the return type as `void`.

* Java functions must be a part of a class (in this case "Main")

## Strongly-Typed vs. Weakly-Typed  

In a strongly typed language, when variables have a type, they will always be that type. You *cannot* assign a value of one type to a variable of another type. In a weakly typed language, variables don't have a type. You can assign anything to them. There are advantages to Java being a type-safe language.

Let's revisit an earlier example:

```java
function shoutGreeting(firstName, lastName){
    return "HELLO THERE " + firstName + " " + lastName + "!!!!"
} //Our function is the same as before
//But the function call has been changed
console.log(shoutGreeting(0.6, "Smith"))
```

Whoops. 0.6 isn't a very attractive name.

In Java, this would result in a compile-time error when we attempt to call the `shoutGreeting(String, String)` method with a number as the parameter. Java is "type safe" and requires that we pass a function only the type declared in the function definition. We don't need to worry that we might accidentally pass the wrong type. If the function expects a `String` and we pass it an `int`, this will result in a compiler error.

## Control Flow Differences  

The `if`, `else/if`, `else`, `while`, `for`, and `switch/case` statements you are already familiar with from JavaScript work **very** similarly in Java. Again, because Java is a more strictly-typed language, there are things that JavaScript will let you "get away with" that Java does not.

For example, the following code "works" in JavaScript - that is, it may or may not do what you want it to do, but won't directly cause a crash, and will not cause a compiler error. The number 5 is "truthy" and the interpreter will evaluate it as true in lieu of a real conditional statement.

Placing the number 5 in the condition of the following if statement is perfectly acceptable in JavaScript.

```javascript
let x = 5;
if (x) {
    //This works just fine
}
```

In contrast, equivalent Java code will result in an error. Java insists that whatever's inside the `if` statement must evaluate to a `boolean` (so for example, `if (x == 5)` works fine). **There is no "truthy" and "falsy" in Java.**

```java
int x = 5;
if (x) {
    //Statement
}

// ERROR!! 5 is not a boolean
```

Similarly with a `for` or `while` loop: Java will insist that the conditional part of the statement evaluates to a boolean.

## Conclusion  

Java and JavaScript look very similar at times, but knowing the small inconsistencies can make all the difference when moving from one language to the other. A good rule of thumb is that Java is *much* more strict about types and about the rules you must follow when authoring your programs.

You must be a stickler about types and structure, and you must keep track of the operations between different data types.

## Additional Resources  

http://www.seguetech.com/java-vs-javascript-difference/

http://www.htmlgoodies.com/beyond/javascript/article.php/3470971
