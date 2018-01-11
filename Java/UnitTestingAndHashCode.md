# Unit Testing in Java

Writing code is like building a house. First, plans are drawn up. Then, the basic structure is built out. Details get tacked on and refined. Eventually, the structure is finalized and declared complete. Some parts of your code will rely on other parts, and if a critical area of your code doesn't work the way you intend, the whole thing can come crashing down.

**How do you make sure that doesn't happen?** When building a home, each stage of the construction is reviewed and evaluated by a competent inspector. They evaluate the building, plumbing, electrical systems, the foundation, and every other facet of the construction process.

With programming, no inspector is going to look over your shoulder and ensure that your code adheres to standards and operates as expected. Instead, you'll write a suite of tests that run each small parts of your application and check that the code runs as expected. This process is called "Unit Testing."

In this article, we'll discuss unit testing in Java.

## Live Coding!  

This lesson is a live coding exercise - please follow along with the lesson. We'll write a function and a suite of tests to ensure that the function behaves as expected. The function we will write is `containsOne(int)` and has a few specifications for its behavior.

* Receives a positive integer as an argument.

* Returns `true` if the number contains a `1` digit (1 -> true, 219 -> true, 97970001000 -> true)

* Returns false if the number doesn't contain a `1`. (3 -> false, 478 -> false, 97970009000 -> false)

* If the argument is negative, the method should throw a `BadInputException`.

Don't worry about solving the function - we'll do that as we go. The focus of this article should be unit testing.

```java
public class MethodsClass {
    public static boolean containsOne (int n) {
        //We'll write the function after we've written our unit tests
        //This is TDD: Test Driven Development.
        return false;
    }
}
```

The goal of writing unit tests for this function is to ensure that the function will work as expected for all possible values. In practice, it would be challenging and time-consuming to test ALL of those values, so we will be trying to come up with a small set of **test values** that cover all the scenarios. After you create a new Java project, the following steps will allow you to import JUnit using Maven.

## Import JUnit  

1. From IntelliJ's main menu, go to File -> Project Structure.

2. Under the Project Settings heading on the left, select Libraries.

3. Add a new module with the "+" button and select "from Maven" from the drop-down menu.

4. In the search field, enter `junit:junit:4.12`. Maven will look for the files - it might take a minute to find them. Once the spinning icon stops, Maven has found JUnit.

5. Press "ok."

6. When the "Choose Modules" dialog opens, select your project folder. Press "ok" to confirm and back to the "Project Structure" panel, which should now list the JUnit `.jar` (and possibly other dependencies)

7. Press "ok" in the "Project Structure" panel, and you will go back to your project, which should now contain JUnit in your "External Libraries" folder.

## Setup a Test Folder  

Next, you'll want to set up a folder to contain all of your unit tests.

1. Right-click your project either in the regular project view or the Project Structure menu (described above).

2. Select New -> Directory.

3. Name the folder "test."

4. To mark the new "test" directory as a test folder. Right click on "test" and select "Mark Directory as" -> "Test Sources Root" and the "test" folder should turn green.

## Create A Test  

1. Return to your original file where you defined the `containsOne()` function (in the example above it was called `MethodsClass`).

2. From this file select Navigate from the top menu, and choose "Test", -> Create new Test.

3. Under Generate, check the boxes next to "setUp," "tearDown," and the `containsOne()` method to create a class that looks a lot like this:

```java
import org.junit.After;
import org.junit.Before;
import org.junit.Test;

import static org.junit.Assert.*;

public class MethodsClassTest {
    @Before
    public void setUp() throws Exception {
    }

    @After
    public void tearDown() throws Exception {
    }

    @Test
    public void containsOne() throws Exception {
    }

}
```

Go ahead and add a body to each of these functions (and rename `containsOne()` to `testBasicCase()`) and run your code.

```java
@Before
public void setUp() throws Exception {
    System.out.println("Setting up...");
}

@After
public void tearDown() throws Exception {
    System.out.println("Cleaning up");
}

@Test
public void testBasicCase() throws Exception {
    System.out.println("Running basic test");
    assertTrue(MethodsClass.containsOne(1));
    assertFalse(MethodsClass.containsOne(2));
}
```

You should see something like this:

```sh
Setting up...
Running basic test
Cleaning up

java.lang.AssertionError
    at org.junit.Assert.fail(Assert.java:86)
    at org.junit.Assert.assertTrue(Assert.java:41)
    at org.junit.Assert.assertTrue(Assert.java:52)
    at MethodsClassTest.testBasicCase(MethodsClassTest.java:25)
```

## What We Have So Far  

JUnit and IntelliJ set up three functions. JUnit uses **annotations** to identify the functions using @Before, @After, and @Test. Annotations will be covered in more detail later - for now just think of them as a little message to JUnit.

For each function marked with @Test, it will run the @Before function, then the @Test, then @After. These before/after functions do setup and cleanup work. One of the principles of unit tests is that they clean up after themselves (they don't leave changes in the code).

The @Test function, `testBasicCase()` is failing. Why is this? Well, currently we're testing a function that we haven't written yet. It always returns false, so the `assertTrue()` call will always fail. The second assertion isn't even getting checked yet - once an assertion fails, the test ends.

## Extending Tests and Solution  

Let's (partially) solve the problem:

```java
public static boolean containsOne (int n) {
    if (n % 10 == 1) {
        return true;
    }
    return false;
}
```

After adding this, rerun your test. It should pass now. But we're only examining two fundamental cases (1 and 2) out of millions of possible numbers. It should be apparent that our function has some significant flaws. For example, the number `10` contains a `1`, but the test will assert that it doesn't.

Let's go back and write a few more tests to check a wider range of values.

```java
@Test
public void testLargeNumbers () throws Exception {
    System.out.println("Testing large numbers");
    assertTrue(MethodsClass.containsOne(10000002));
    assertFalse(MethodsClass.containsOne(986974958));
}

@Test
public void testMediumNumbers () throws Exception {
    System.out.println("Testing medium numbers");
    assertTrue(MethodsClass.containsOne(10));
    assertFalse(MethodsClass.containsOne(92));
}
```

If you run your tests once again, you should see that the both new tests fail. At this point, we have a robust set of basic tests - we're testing both `true` and `false` cases for small, medium, and large numbers. Let's write the function.

```java
public static boolean containsOne (int n) {
    String numString = "" + n; //Convert the number to a string
    return numString.contains("1"); //Check to see if it contains 1
}
```

All of your tests should pass! But as Columbo said, "Just one more thing."

## Testing Exception Throwing  

The original specifications for our function state that it should only accept positive numbers. It should throw an exception for any negative input. Let's write a test for that. We want to check that our program handles error cases as we expect.

To accomplish this, we will be making use of a feature in JUnit, the `ExpectedException` rule. Essentially, we are telling JUnit that we **expect** the method to throw an exception. If it does, the test will pass (because we wanted it to throw the exception). If the exception **isn't** thrown, the test will **fail**.

To set this up, we'll need to add the rule declaration:

```java
@Rule //Another annotation directed to JUnit
public ExpectedException expected = ExpectedException.none();
```

Also add the following class to your project:

```java
public class BadInputException extends Exception {
}
```

Now we can define a new test.

```java
@Test
public void testNegativeNumbers () throws Exception {
    System.out.println("Testing negative values");
    expected.expect(BadInputException.class);
    MethodsClass.containsOne(-1);
    MethodsClass.containsOne(-20000);
}
```

If you run this test right now, you should see something like this:

```sh
Setting up...
Testing negative values
Cleaning up

java.lang.AssertionError: Expected test to throw an instance of BadInputException

    at org.junit.Assert.fail(Assert.java:88)
    at org.junit.rules.ExpectedException.failDueToMissingException(ExpectedException.java:263)
    at org.junit.rules.ExpectedException.access$200(ExpectedException.java:106)
    at org.junit.rules.ExpectedException$ExpectedExceptionStatement.evaluate(ExpectedException.java:245)
    at org.junit.rules.RunRules.evaluate(RunRules.java:20)
```

Now we can finally finish writing our function:

```java
public static boolean containsOne (int n) throws BadInputException {
    if (n < 0) {
        throw new BadInputException();
    }
    String numString = "" + n; //Convert the number to a string
    return numString.contains("1"); //Check to see if it contains 1
}
```

Run your tests one last time. They should all pass. At this point, we can feel reasonably confident that we're testing a good range of values.

## Conclusion  

Unit testing is an essential tool for any modern programmer. Any time you write a function with any real complexity (i.e. more complex than a getter/setter, or just adding up some numbers), write a few unit tests for that function to ensure it behaves as expected. In this lesson, we covered how to use JUnit to write unit tests. We discussed considering a broad range of possible inputs, and also how to use the `ExpectedException` rule.

---

# HashCode

All Java classes extend Object. The Object class has several methods that child classes can override. An example of one of these methods is `toString()`.

In this lesson, we will discuss the `hashCode()` method and how to override it.

## What is a HashCode?  

The `hashCode()` method converts the data of a class instance and turns it into a number (specifically, a 32-bit signed integer). Simply put, it turns an object into a number. You can think of it as a `toInt()` method for objects.

Like `toString()`, `hashCode()` not "loss-less". When `hashCode()` converts an object, it will possibly destroy some of the data. You would probably have trouble taking a hashcode and rebuilding the object that generated it.

So, if "hashing" an object destroys some of the data of the object, what's the point? The answer is that an object's hashcode gets used when it is inserted into a `HashMap`, `HashSet`, or another collection.

### Buckets  

When you add an object to a collection, (for example a `HashMap`) behind the scenes, it gets dumped in a "bucket." This bucket represents a virtual space where the objects exist. The `HashMap` could have many buckets, each containing the objects stored in the `HashMap`. When a call comes in to ask if the collection contains a given object, it just looks at the hashcode for the object we want (say, 59). Then it checks the appropriate bucket (59).

### Overriding HashCode  

Overriding the `hashCode()` method can provide significantly better performance when using objects in collections. The basic mechanics of overriding the method are the same.

```java
public class Pojo {
    int value;
    String name;
    double weight;
    boolean hasPants;

    @Override //<-Optional but suggested to use the @Override annotation
    public int hashCode() {
        return 1;
    }
}
````

However, `hashCode()` requires one additional contract:

`If two objects are equal according to the equals(Object) method, then calling the hashCode method on each of the two objects must produce the same integer result.`

So the `hashCode()` function we've written above does not follow this rule. We will look at two methods for generating better `hashCode()` methods.

## Method 1 - Objects.hash()  

The simplest method, introduced in Java 7, is to use the Objects class, specifically the `static int hash(Object... values)` method. This method takes care of the hashing behind the scenes - all we have to do is tell it what variables we want to "count" in the hash.

```java
import java.util.Objects;

public class Pojo {
    int value;
    String name;
    double weight;
    boolean hasPants;

    @Override
    public int hashCode() {
        return Objects.hash(value, name, weight, hasPants);
    }
}
```

## Method 2 - IDE Generation  

Another way is to have IntelliJ generate a method for you. Right click in your class file, and select "Generate -> equals()" and hashCode(). IntelliJ will put together something like this:

```java
public class Pojo {
    int value;
    String name;
    double weight;
    boolean hasPants;

    @Override
    public int hashCode() {
        int result;
        long temp;
        result = value;
        result = 31 * result + (name != null ? name.hashCode() : 0);
        temp = Double.doubleToLongBits(weight);
        result = 31 * result + (int) (temp ^ (temp >>> 32)); //Bitshift op
        result = 31 * result + (hasPants ? 1 : 0); //Ternary op
        return result;
    }
}
```

The result of running "Generate -> equals()" probably looks very mathematical and intimidating. You don't need to know what is involved in all the algorithms for shaping the hash. Understanding that math is not the point of this article. However, we'll point out a couple of details you may want to research.

* The Ternary Operator, AKA Conditional Operator - `(boolean) ? <first>:<second>` - essentially a shorter way of saying "if this expression is true, use the first value, if not, use the second value"

* The Bitshift Operator - `>>>` - these operators change the binary/bit representation directly.
In short, all of this code is turning the object into a unique-ish number. Don't worry if you don't understand every aspect of the code above. For instance, **Why 31?!?**

## Conclusion  

**The point of this article is to explain the concept of a hash and to demonstrate how to generate a hash using IntelliJ.** You will encounter code that uses hashes, and you will work with classes that override `hashCode()`. It's important to understand what you are looking at and what the code is accomplishing.
