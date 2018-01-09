# Lambda Expressions

Let's say that we have a List<Employee> that we want to sort.

List<Employee> employeeList = new ArrayList<>();
//new Employee(int salary, String name, int age)
employeeList.add(new Employee(40000, "Sam", 24));
employeeList.add(new Employee(50000, "Bob", 5));
//Etc...
Java provides several ways to sort collections. If Employees are sorted using a single criterion, the simplest approach is to have Employee implement the Comparable interface and use the Collections.sort() method.

class Employee implements Comparable<Employee> {
     public int compareTo (Employee otherEmployee) {
        //Our comparison
    }
}
Comparable works well if we only sort employees one way - alphabetically by the first name, for example. What if we want to sort employees by multiple criteria or even combinations of criteria? Today, I want to sort employees by the ratio of their salary to their age; tomorrow maybe in reverse alphabetical order. The next day, I'll want to sort them by the number of letters in their full name.

Writing classes for all these different "sorts" isn't practical, but let's look at an example anyway:


1
import java.util.*;
2
​
3
public class NoAnonClasses {
4
    public static void main(String[] args) {
5
        List<Employee> employeeList = new ArrayList<>();
6
​
7
        employeeList.add(new Employee(40000, "Sam", 24));
8
        employeeList.add(new Employee(50000, "Bob", 5));
9
        employeeList.add(new Employee(80000, "Carol", 35));
10
        employeeList.add(new Employee(20000, "Alex", 20));
11
​
12
        //Sort by salary
13
        employeeList.sort(new SortBySalary());
14
        System.out.println(employeeList);
15
​
16
        //Sort by name
17
        employeeList.sort(new SortByName());
18
        System.out.println(employeeList);
19
    }
20
}
21
​
22
class SortBySalary implements Comparator<Employee> {
23
    public int compare(Employee e1, Employee e2) {
24
        if (e1.getSalary() > e2.getSalary()) {
25
            return 1;
26
        } else if (e2.getSalary() > e1.getSalary()) {
27
            return -1;
28
        }
29
        return 0;
30
    }
31
}

Fullscreen

Reset Code
Run Code 
Now imagine that we want five or even ten different ways of sorting. Using Comparable could get very cluttered, very quickly.

In this article, we will discuss two more concise approaches to solving this type of problem. First, we'll consider anonymous classes and then lambda expressions.

Anonymous Classes  
Anonymous classes allow us to declare and instantiate a class at the same time. Anonymous is quite literal here - it has no name. The class immediately instantiates in place. It is a one-use object. After all, you can't later reference a class with no name. Let's look at a new version of the above code that uses anonymous classes.


1
import java.util.ArrayList;
2
import java.util.Comparator;
3
import java.util.List;
4
​
5
public class AnonClassExample {
6
    public static void main(String[] args) {
7
        List<Employee> employeeList = new ArrayList<>();
8
​
9
        employeeList.add(new Employee(40000, "Sam", 24));
10
        employeeList.add(new Employee(50000, "Bob", 5));
11
        employeeList.add(new Employee(80000, "Carol", 35));
12
        employeeList.add(new Employee(20000, "Alex", 20));
13
​
14
        //Sort by salary
15
        employeeList.sort(new Comparator<Employee>() {
16
            //Inside this block, we will need to implement all the methods of the interface
17
            @Override
18
            public int compare(Employee e1, Employee e2) {
19
                if (e1.getSalary() > e2.getSalary()) {
20
                    return 1;
21
                } else if (e2.getSalary() > e1.getSalary()) {
22
                    return -1;
23
                }
24
                return 0;
25
            }
26
        }); //The close paren here matches up to the .sort(
27
        System.out.println(employeeList);
28
        //ArrayList has a reasonable toString() method
29
​
30
        //Sort by name
31
        employeeList.sort(new Comparator<Employee>() {

Fullscreen

Reset Code
Run Code 
Pay attention to each run of employeeList.sort() for each run new Comparator<Employee>() creates a new anonymous class that handles the sorting operation. This approach is beneficial because it doesn't require several single use named classes. The classes that handle the sorting are created only in the places where they are needed.

Overall this is a more succinct way of accomplishing our goal. It avoids cluttering our project with a bunch of classes that we will only use once (note: when we say "only use once" that refers to the number of occurrences in the code. These sort functions could easily be in methods that get called many times).

Lambda Expressions  
Just like anonymous classes, Java (as of Java 1.8) provides support for anonymous functions. Just like anonymous classes, anonymous functions are more concise. We can use anonymous functions in any case where we have a Functional Interface - an Interface that only has one method. The Comparator interface we've been using is a perfect example. It only has int compare(T, T). Anonymous functions in Java are known as "Lambda Expressions."

In the following example, we refactor the anonymous class code to use Lambdas.


1
import java.util.ArrayList;
2
import java.util.List;
3
​
4
public class LambdaSortExample {
5
​
6
    public static void main(String[] args) {
7
        List<Employee> employeeList = new ArrayList<>();
8
​
9
        employeeList.add(new Employee(40000, "Sam", 24));
10
        employeeList.add(new Employee(50000, "Bob", 5));
11
        employeeList.add(new Employee(80000, "Carol", 35));
12
        employeeList.add(new Employee(20000, "Alex", 20));
13
​
14
        //Sort by name
15
        employeeList.sort((e1, e2) -> e1.getName().compareTo(e2.getName()));
16
        System.out.println(employeeList);
17
​
18
        //Sort by salary
19
        employeeList.sort((e1, e2) -> {
20
            if (e1.getSalary() > e2.getSalary()) {
21
                return 1;
22
            } else if (e2.getSalary() > e1.getSalary()) {
23
                return -1;
24
            }
25
            return 0;
26
        });
27
        System.out.println(employeeList);
28
    }
29
}
30
class Employee {
31
    private int salary;

Fullscreen

Reset Code
Run Code 
Let's look at the syntax of the lambda more closely. To sort employees by name, we used the following expression:

employeeList.sort((e1, e2) -> e1.getName().compareTo(e2.getName()));
First, why are we even allowed to do this? If we look at the documentation for myArrayList.sort(), we see that it expects a Comparator object. What is passed in doesn't look like an instance of a Comparator, but it is.

The Comparator interface defines one method to implement, compare(). Since this is the only method required, the Comparator interface is said to be "functional".

The Java compiler will see the syntax above and reason that, because the Comparator interface only specifies one method and because lambdas are anonymous methods, it should generate an anonymous class and implement the method defined by the lambda.

The sort() method will run the code to the right of the -> symbol, providing arguments with the names specified to the left. You don't need to specify argument data types or method return types; that is all determined by the functional interface. The lambda expression is-a Comparator, but the syntax is new.

In the next section, we'll look at lambda syntax to better understand what is being defined.

Lamdba Syntax  
(e1, e2) -> e1.getName().compareTo(e2.getName())

Lambda expressions are functions. Functions have inputs (arguments, parameters) and outputs (what they return or do). The simplest way to interpret above expression is to think of it as a function. The arguments (inputs) are difined as a comma separated list surrounded by parentheses, just like regular functions. The output follows the arrow operator (->). We could read this as "Given (e1, e2), this lambda will return/perform e1.getName().compareTo(e2.getName())".

public int anonymous (Employee e1, Employee e2) {
  return e1.getName().compareTo(e2.getName());
}
Lambda functions usually encapsulate one expression and do not require the return keyword. However, if the work done by the lambda is complex and spans multiple lines, then the lambda body should be wrapped in curly braces {} and should contain an explicit return statement. This syntax is demonstrated in the following example:

//Sort by salary
employeeList.sort((e1, e2) -> {
    if (e1.getSalary() > e2.getSalary()) {
        return 1;
    } else if (e2.getSalary() > e1.getSalary()) {
        return -1;
    }
    return 0;
});
Lambda expressions can be as complex as you need them to be. You can call other functions, print to the console, or anything else you might need to do.
