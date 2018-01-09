# Initialize an Array with Multiple Values  

This lesson will introduce you to using arrays in Java.

## Arrays in Java vs. JS

Arrays in Java are conceptually very similar to arrays in JavaScript. However, there are two key differences:

1. Arrays in Java must have a type. Each element of the array must be of the same type. Java Arrays can't contain a mix of Strings, ints, and other Objects.

2. Arrays in Java are of a fixed size. JavaScript allows the size of an array to change by adding or removing items. When initializing a Java array, we must declare the size of the array. An array initialized with size 5 will be size five until it is re-initialized.

3. In JavaScript, attempting to access an out of bounds index value will return undefined. For example, if the array has four elements and you ask for index 4, (the Fifth Element). Attempting to do the same thing in Java will result in an IndexOutOfBoundsException.


1
// js will return "undefined" 
2
let numbers = [ 2, 3, 5, 7 ];
3
console.log( numbers[ 4 ] );

Fullscreen

Reset Code
Run Code 

1
public class OutOfBoundsExample{
2
    public static void main( String args[] ){
3
        // We'll talk about this syntax in a short while
4
        int[] numbers = {2, 3, 5, 7};
5
        // This will throw an error
6
        System.out.println( numbers[ 4 ] );
7
    }
8
}

Fullscreen

Reset Code
Run Code 
Creating Arrays Method 1: Declare Array Size  
The simplest method to initialize a new array is using the new keyword and providing the size. When declaring array types, use []. For instance, to declare an array type of int the type is int[].

int[] numbers = new int[5];
String[] aNames = new String[4];
Object[] objects = new Object[3];
We can then provide values for the spots in the array:

numbers[1] = 3;
numbers[4] = 11;
numbers[5] = 5; //<- IndexOutOfBoundsException !!!!
aNames[2] = "Allison";
Creating Arrays Method 2: Provide Starting Values  
The new keyword isn't required to initialize arrays. An array can be assigned a series of initial values inside of a {} code block, separated by commas. Assigning initial values implicitly tells the array its size.

// this array is size 5
int[] numbers = {2, 3, 5, 7, 11};
// this array is size 4
String[] aNames = {"Alex", "Andrew", "Allison", "Adam"};
// this array is size 3
Object[] objects = {new Object(), new Object(), new Object()};
You can see that this works the same way for simple types and objects. Note that we are not providing the size of the array to Java; it infers this information based on the number of elements we provided. So the numbers array would have 5 elements, colors 4, and objects 3.

---

# Array Looping

Looping over arrays in Java is fun and easy. We will go over two similar methods.

Iteration: The For Loop  
A conventional method for iterating over array data is to use a for loop and the length of the array.

In the following example, we've declared an array of integers and are using a regular for loop to iterate through and find the sum. We use the length property of the array as our limit, ensuring we will never go out of bounds. As soon as the index equals the length of the array, our loop will exit.


1
public class ForLoopXample {
2
    public static void main (String[] args) {
3
        int[] numbers = {4, 2, 6, 9, 11};
4
        int sum = 0;
5
        for (int index = 0; index < numbers.length; index++) {
6
            sum += numbers[index];
7
            System.out.println(numbers[index]);
8
        }
9
        System.out.println("Sum of numbers array is " + sum);
10
    }
11
}

Fullscreen

Reset Code
Run Code 
Iteration: The For Each Loop  
The for each loop is a more compact structure for iterating through arrays. The syntax is a little different, but once you understand what is occurring, this compact iteration method is convenient and easy to read.

The following example demonstrates the for each loop by iterating over an array of numbers and summing the values. The key to understanding the for each loop is to understand what is the statement int num: numbers means.

int num is the variable that will hold the current value of the loop. During the first run of the loop, num is 4. The second run of the loop, num is 2. The third run of the loop, num is 6, etc.
The second part numbers, is the array being iterated over.
The for each loop uses the for keyword, but you aren't required to manage an index value or to access array values directly since the structure provides the appropriate value at each run of the loop (num). for each will run the loop once "for each" member of the array

The following for each loop can be read as follows: "For each element "num" in numbers" -> Take action


1
public class ForEachLoopXample {
2
    public static void main (String[] args) {
3
        int[] numbers = {4, 2, 6, 9, 11};
4
        int sum = 0;
5
        for (int num : numbers) {
6
            // The action
7
            sum += num;
8
            System.out.println(num);
9
        }
10
        System.out.println("Sum of numbers array is " + sum);
11
    }
12
}

Fullscreen

Reset Code
Run Code 
Comparing the Two Methods  
So which of these is better? If you don't need to know the index of the items, then the for each loop is cleaner. For each loops can also iterate over other collections. The goal is to make the code a bit cleaner to read and write. If you do need the index or if you want to iterate over the array at a different interval (maybe you want only the even indices), then you might as well use a regular for loop.

int[] numbers = {4, 2, 6, 9, 11};
for (int index = 0; index < numbers.length; index++) {
    if (index % 2 == 0) {
        sum += numbers[index];
    }
}
You can always keep track of the index "manually" in a for each loop.

int[] numbers = {4, 2, 6, 9, 11};
int index = 0;
int sum = 0;
for (int num : numbers) {
    sum += num;
    index++;
}

---

# Filtering an Array  
"Filtering" is the process of selecting a subset of values from a list. For example, I may want to filter a list of numbers to only include the even values

// My list is as follows
1, 2, 4, 5, 7, 8, 9, 12, 15, 16, 18 

// I run the list through a "filter"
...

// the filter returns even numbers, and I get
2, 4, 8, 12, 16, 18 
In this article, we will discuss filtering Java Arrays.

Filtering an Array with a Function  
In the following example, we filter an array of names to return only names four or fewer characters. For instance, the filter will return "Jon" because it has three characters, but will not return "Evelyn" because it has six characters.

Java arrays have a fixed length. When filtering an array, you must run the Array through a filtering function and return a new array with the filtered values.


1
import java.util.ArrayList;
2
import java.util.List;
3
​
4
​
5
class ArrayFilter{
6
    //Takes an array of "names" (any String) and
7
    // returns a filtered array
8
    //containing only names with fewer characters than <length>
9
    public static String[] filterNames (String[] names, int length) {
10
        List<String> goodNames = new ArrayList<>();
11
        for (String name : names) {
12
            if (name.length() < length) {
13
                goodNames.add(name);
14
            }
15
        }
16
        return goodNames.toArray( new String[ goodNames.size() ] );
17
    }
18
}
19
​
20
public class ArrayFilterExample {
21
​
22
    public static void main(String[] args) {
23
        String[] names = { "Jon", "Paul", "Sal", "Evelyn", "Reginald" };
24
​
25
        ////****** This is the filter function at work *****////
26
        String[] filteredNames = ArrayFilter.filterNames(names, 4);
27
​
28
        for (String name: filteredNames) {
29
            System.out.println(name);
30
        }
31
    }
32
}

Fullscreen

Reset Code
Run Code 
Notes:

The extra steps involved to create a List and convert back and forth are required because arrays are of a fixed size - we can't create the new array until we know how many elements it has. We could alternatively iterate through the original array twice: once to find out how many elements the new array should contain, and another to copy over the "good" names
We are using the toArray() method from the List interface to convert back to an array. This method uses generics (discussed elsewhere), but basically, we are passing the method the array into which we want it to copy the list's values
