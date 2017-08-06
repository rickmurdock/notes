Algorithms  

We grab a microwaveable meal and follow the instructions on the back. Then in a couple of minutes, we are eating our dinner. We could call the uncooked meal the input, the instructions the steps and the tasty meal the output. Moreover, we can repeat these steps for the same meal time after time and get the same result, a gourmet delight! The microwavable meal instructions are a perfect example of an algorithm.

An algorithm is an explicit and sequential step-by-step set of instructions for performing a particular task.

In programming, algorithms exist for generalizable, repeatable tasks. We encounter algorithms when we perform a Google search, look up a product on Amazon, search for inventory in a warehouse or play an online word search game. JavaScript uses algorithms when it divides numbers, finds a square root, or sorts an array. Algorithms make it possible to search and manipulate collections of data.

In this lesson, we will discuss how to author an algorithm, plus we will look at some algorithms to see how they work.

Writing an Algorithm  

When giving instructions to another person, clarity and precision matter a great deal. However, we also expect them to use human reason and intuition to fill in any gaps. Using our microwaveable meal example above, we might give a person this set of instructions:

Put a small slit in the plastic covering of the meal
Microwave the meal on high for 3 minutes
Put the sauce on the noodles and stir
Most people would find these instructions reasonable and would find success following them. However, the instructions are incomplete. When talking to another human, we can infer that they know (generally) how a microwave works. We can assume that they will "put a small slit in the plastic..." using an appropriate tool and a reasonable method. We can assume that they know to open and close the microwave door. We can expect that they will empty the sauce packet into the noodles and not drop the entire package onto their meal. We assume all of these things because humans have the ability to reason out problems and to fill in missing information. We believe that a person will "figure it out."

In the interest of precision, let's try to write the algorithm more carefully, this time giving clearer and more complete instructions.

A more explicit set of instructions might be:

Remove the plastic noodle container and sauce packet from the box.
Using a knife, cut a 1-inch slit anywhere on the plastic covering of the noodle container.
Put the noodle cup in the microwave.
Press the HIGH button.
Press 3, then 0, then 0.
Press the START button.
After three minutes, take the noodle container out of the microwave.
Remove the plastic covering from the container.
Open the sauce packet.
Squeeze the sauce from the packet onto the noodles.
Stir the noodles.
And still, we're missing steps! We never mention opening or closing the microwave door, for example! We never suggest what to use to stir the noodles. We can get away with this lack of precision when talking to other humans, but not with computers.

Herein lies the first lesson about algorithms and computers: Instructions (algorithms) given to a computer must be specific. Computers do not make inferences. If you don't tell the computer to open the microwave door, it will not do so.

Coding an Algorithm  

Imagine that you want to write an algorithm to calculate the tip for one or more restaurant checks. We will implement this algorithm as a function calculateTips. The function will take two arguments: tip percentage as a decimal (0.15) and an array of objects representing checks, each with a name and a total. calculateTips will return an array of objects containing the name and tip amount for each check.

As an example, we'd expect our function to do this:

let checks = [
  { name : "Dennis", amount : 21 },
  { name : "Kim", amount : 18 }
];

calculateTips( 0.15, checks );

// This function should return an array:
// [
//    { name : "Dennis", amount : 3.15 },
//    { name : "Kim", amount : 2.70 }
// ]
What steps should calculateTips follow? One set of steps are:

Create an empty array for tips.
Look at the first check.
Given the check, calculate the tip by multiplying the total by the tip percentage.
Store the tip percentage and name property in a new object.
Put this object in the tip array.
Look at the next check and go back to step #3. Repeat until you have gone through all checks.
Return the tips array
Let's see this in code:


1
let checks = [
2
    { name : "Bill", amount : 22 },
3
    { name : "Mary", amount : 44 },
4
    { name : "John", amount : 88 }
5
];
6
​
7
let tipPercentage = 0.18;
8
​
9
function calculateTips(tipPercentage, checks) {
10
    // Step 1: Create an empty array for tips.
11
    let tips = [];
12
​
13
    // Combines step 2 and 6: starts with the first check and iterates through.
14
    for (let i = 0; i < checks.length; i++) {
15
​
16
        // Step 3: Calculate the tip by multiplying the total by the tip percentage.
17
        let tipAmount = checks[i].amount * tipPercentage;
18
​
19
        // Step 4: Store the tip percentage and `name` in a new object.
20
        let tip = {
21
            name : checks[i].name,
22
            amount : tipAmount
23
        };
24
        // Step 5: Put this object in the tip array.
25
        tips.push(tip);
26
    }
27
    // Step 7: After the loop has finished running, return
28
    // the entire tips array.
29
    return tips;
30
}
31
​
32
console.log(calculateTips(tipPercentage, checks));

Fullscreen

Reset Code
Run Code 
The main idea of this discussion is that algorithms are a series of step-by-step commands. When giving those commands to a computer, you have to be explicit and use a syntax that the computer can interpret.

Let's look at a few more examples of algorithms.

Demonstrating Algorithms - Is it Even?  

Let's say that you are validating input for a game. In your game, players enter numbers into a field, and you need to check to see if the numbers are even. Utility algorithms like this are common and are usually implemented as functions. We'll do the same and call our function isEven(). The function takes one argument, a number. If the number is even, the function returns true. If the number is odd, the function should return false.

// This should return `false`
function isEven( 7 );

// This should return `true`
function isEven( 8 );
The big problem with constructing this function is deciding how to determine if the number is even. Let's consider this issue. Even numbers are evenly divisible by 2. Odd numbers are not. If I divide an odd number by 2, then I get a remainder, meaning I can check if a number is even by dividing it by 2. If the number is even, then the remainder will be 0. If the remainder is not 0, the number is odd.

// division to test if the number is even
9 / 2 = 4.5 // odd
8 / 2 = 4   // even
7 / 2 = 3.5 // odd
6 / 2 = 3   // even
5 / 2 = 2.5 // odd
4 / 2 = 2   // even
3 / 2 = 1.5 // odd
2 / 2 = 1   // even
1 / 2 = 0.5 // odd
JavaScript even provides a special operator that returns just the remainder of a division operation. The modulo operator returns only the values we need.

// modulo to test if the number is even
9 % 2 = 1   // odd
8 % 2 = 0   // even
7 % 2 = 1   // odd
6 % 2 = 0   // even
5 % 2 = 1   // odd
4 % 2 = 0   // even
3 % 2 = 1   // odd
2 % 2 = 0   // even
1 % 2 = 1   // odd

Given all of that, what steps should isEven follow? One set of steps are:

Modulo the input value with the number 2
Check to see if the result is 0
If the result is equal to 0, return true
If the result other than 0, return false
Let's see this algorithm as code.


1
// define the function
2
function isEven( input ){
3
    // 1. Modulo the input with 2
4
    let remainder = input % 2;
5
​
6
    // 2. Check to see if the result is equals 0
7
    if( remainder === 0 ){
8
        // 3. If the result is equal to 0, return `true`
9
        return true;
10
    }else{
11
        // 4. If the result other than 0, return `false`
12
        return false;
13
    }
14
}
15
​
16
console.log( isEven( 100002 ) );     // true
17
console.log( isEven( 3 ) );          // false
18
console.log( isEven( 1967923752 ) ); // true
19
console.log( isEven( 2227 ) );       // false

Fullscreen

Reset Code
Run Code 
isEven is a simple function. We've taken our algorithm and converted it into JavaScript syntax. The version we used above is perfectly reasonable, but there are many ways of writing this function. I'll show a couple more ways to implement this algorithm.

No variable and a Default Return of false  

This version inlines the modulo operation with the condition, thereby eliminating the need for a variable. It also removes the else block. The function will return false only if the condition is false. This implementation is nice because it doesn't require a variable, but it's a little harder to read and to understand.


1
// define the function
2
function isEven( input ){
3
    if( ( input % 2 ) === 0 ){
4
        return true;
5
    }
6
    return false;
7
}
8
​
9
console.log( isEven( 100002 ) );     // true
10
console.log( isEven( 3 ) );          // false
11
console.log( isEven( 1967923752 ) ); // true
12
console.log( isEven( 2227 ) );       // false

Fullscreen

Reset Code
Run Code 
Compact  

This version is super compact. In a single expression, the modulo of input and 2 gets compared to 0. Because the equals operator results in a boolean, we can return that result from our function. The trade off with this implementation is that it is much harder to read and to understand. You'll see solutions like this from more senior programmers who want to write compact code that is efficient.


1
// define the function
2
function isEven( input ){
3
    return input % 2 === 0;
4
}
5
​
6
console.log( isEven( 100002 ) );     // true
7
console.log( isEven( 3 ) );          // false
8
console.log( isEven( 1967923752 ) ); // true
9
console.log( isEven( 2227 ) );       // false

Fullscreen

Reset Code
Run Code 
Demonstrating Algorithms - Binary Search  

Binary Search is a well-known algorithm to search for the presence of an element in a sorted array. The algorithm doesn't tell you where the element appears in the array, just whether it is present. This algorithm also assumes that the array is sorted from least to greatest.

// The binary search algorithm won't work on this array
let nope = [ 6, 7, 4, 5, 8, 9, 1, 2, 3 ];

// arrays need to be sorted from least to greatest
let works = [ 1, 2, 3, 4, 5, 6, 7, 8, 9 ];
Before we try to implement the algorithm in code, let's illustrate how it works. The following five steps are the binary search algorithm:

Look at the element in the middle of the array. Is it greater than, equal to, or less than the element we are looking for?
If the current element is equal to the value we are looking for, then great! We are done. The algorithm can return true, and the algorithm ends.
If the current element less than the element we are looking for, discard it and the elements before it in the array.
If the current element greater than the element, discard it and the elements after it in the array.
If the array has 0 elements, we did not find the element. Return false. Otherwise, repeat step 1-4 on the remaining elements.
Binary Search for 13  

We're going to write this out in code, but let's illustrate it first. Imagine we have an array equal to [1, 2, 3, 4, 6, 8, 9, 13, 19] and we want to see if the number 13 is in the array.

First, we select the element that is in the middle of the array. The element in the middle of the array is 6.

             v
[1, 2, 3, 4, 6, 8, 9, 13, 19]
We then compare that number to the one we are searching for. 6 is less than 13, so we discard it and all elements before it in the array. Now the array has only four elements. Removing the elements completes one iteration of the binary search algorithm. Now that we've cut down the number of indexes to search, we repeat the operation again, on the smaller array.

[8, 9, 13, 19]
Again, select the central element in the array. We have a problem, though -- we have an even number of elements in the array. In this case, we'll look at the element in the lower position from the middle. That number is 9:

    v
[8, 9, 13, 19]
We then compare 9 to the one we are searching for. 9 is less than 13, so we discard it and the elements below it, leaving us with 2 elements left in the array.

[13, 19]
We repeat the operation a third time, again on an even-numbered array. The lower element is 13.

 v
[13, 19]
We compare this number with the number we are searching for. 13 is the same as 13, so we've found our number. And we've found 13 in 3 steps!

Binary Search for 5  

Let's search the same array for the number 5.

             v
[1, 2, 3, 4, 6, 8, 9, 13, 19]
6 is greater than 5, so we discard it and all the numbers that follow and repeat the algorithm.

    v
[1, 2, 3, 4]
2 is less than 5. Discard 2 and the numbers below it and repeat the algorithm.

 v
[3, 4]
3 is less than 5. Discard it and repeat.

 v
[4]
4 is less than 5. Remove it, and the array is empty! Because the algorithm hasn't found 5 and the array is empty, it means that 5 doesn't exist in the array. We did not find 5, and we took 4 steps.

Binary Search Code  

Here are the 5 steps that we'll implement in our algorithm.

Look at the element in the middle of the array. Is it greater than, equal to, or less than the element we are looking for?
If the current element is equal to the value we are looking for, then great! We are done. The algorithm can return true and the algorithm ends.
If the current element is less than the element we are looking for, discard it and the elements before it in the array.
If the current element is greater than the element, discard it and the elements after it in the array.
If the array is empty, we did not find the element. Return false. Otherwise, repeat step 1-4 on the remaining elements.

1
function binarySearch( sourceArray, searchItem ){
2
​
3
    // Copy the array. Not part of the algorithm, but
4
    // necessary in JavaScript
5
    let arrayToSearch = sourceArray.slice( 0 );
6
​
7
    // 5. If the array is empty, we did not find the element. Return `false`.
8
    // Otherwise, repeat step 1-4 on the remaining elements.
9
    while( arrayToSearch.length > 0 ){
10
​
11
        // logging the array so you can see how
12
        // it is being reduced during the search
13
        console.log( arrayToSearch );
14
​
15
        // 1. Find the middle of the array
16
        let mid = Math.floor( arrayToSearch.length / 2 );
17
        //    Get the value of the middle element
18
        let midValue = arrayToSearch[ mid ];
19
​
20
        // 2. If the current element is equal to the value
21
        //    we are searching for, then we've found our value, return true;
22
        if( midValue === searchItem ){
23
            return true;
24
        }
25
        // 3. If the current element is less
26
        //    than the element we are looking for
27
        else if( midValue < searchItem ){
28
            // discard it and all the elements before it
29
            arrayToSearch.splice( 0, mid + 1 );
30
        }
31
        // 4. If the current element is greater than the element
32
        else{
33
            // discard it and the elements after it in the array
34
            arrayToSearch.splice( mid, arrayToSearch.length - mid );
35
        }
36
    }
37
    // Return false if the value isn't found in the array
38
    return false;
39
}
40
​
41
let numbers = [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 ];
42
​
43
console.log( binarySearch( numbers, 10 ) );
44
console.log( binarySearch( numbers, 12 ) );
45
console.log( binarySearch( numbers, 0 ) );
46
console.log( binarySearch( numbers, 2 ) );

Fullscreen

Reset Code
Run Code 
The beauty of this search algorithm is that each iteration cuts the search area in half. The total iterations will stay low even if the array is enormous. This algorithm can search a million items in 20 or fewer steps.

Demonstrating Algorithms - All Possible Combinations  

In this final example of algorithms, we want to find all the possible combinations of string elements in an array. This algorithm will be implemented as a function called combos. combos will take an array as an argument. The output will be an array of strings. For example:

let characters = [ "a", "b", "c" ];

combos( characters );
// This will return [ "aa", "ab", "ac", "ba", "bb", "bc", "ca", "cb", "cc" ]

How can we do this? Here is one set of steps that will work:

Create a new empty array to hold pairings.
Select the first element in the array
Iterate over the array, combining the first element with every element in the array
Store each new string in the new array
Repeat for each element in the array
Let's see this in code:


1
function combos( stringArray ){
2
​
3
    // 1. Create a new empty array to hold pairings
4
    let pairs = [];
5
    let length = stringArray.length;
6
​
7
    // 5. Repeat for each element in the array
8
    for( let i = 0; i < length; i++ ){
9
​
10
        // 2. Select the first element in the array
11
        let str1 = stringArray[ i ];
12
​
13
        // 3. Iterate over the array
14
        for( let j = 0; j < length; j++) {
15
            // combining the first element with every element in the array
16
            let combination = str1 + stringArray[ j ];
17
​
18
            // 4. Store each new string in the new array
19
            pairs.push( combination );
20
        }
21
    }
22
    // return all of the combinations
23
    return pairs;
24
}
25
​
26
let students = ["Blake", "Emerson", "Sam", "Ariel"];
27
let characters = [ "a", "b", "c" ];
28
let numbers = [ 0, 1, 2, 3, 4, 5 ];
29
​
30
console.log( combos( students ) );
31
console.log( combos( characters ) );
32
console.log( combos( numbers ) );

Fullscreen

Reset Code
Run Code 
Conclusion  

An algorithm is a generalizable, repeatable set of steps needed to generate an output from an input. While there are many algorithms available to meet our needs, we can create our own. You will spend a lot of time thinking of ways to implement your logic.

Authoring an algorithm takes practice — lots of practice. Give yourself time to adjust to this way of thinking and you will go far.

 Multiple Choice Exercise View Exercise
 Short Answer Exercise View Exercise
Working With Complex Objects  

So far, we've worked with relatively simple objects. However, JavaScript objects can do more than store property values of number, string, and boolean types. Objects can also be used to store functions, data, and even other objects. We will continue our discussion about objects using object literal syntax. Over time, you will learn other ways to create objects that provide even more powerful features and patterns. For now, we'll focus on literals, so that we can easily see the structure of the object and how to access the deeply nested members.

In this lesson, we will discuss complex object and how to access deeper information within the object. This article shouldn't introduce many new topics. Rather, this article attempts to pull together several topics we've discussed in the past and to give you some practice manipulating objects.

Object Literal Nesting  

Nesting is the practice of assigning one object as the property value of another object. The following example demonstrates nesting. The outer object obj has a property called sub that contains another object. sub is accessed using dot notation (obj.sub). To access a property of sub, also use dot notation.

You can access deeply nested properties by "chaining" the dot operator.


1
let obj = {
2
    name : "outer object",
3
    sub : {
4
        name : "nested object"
5
    }
6
};
7
​
8
// Access the name property of obj
9
console.log( obj.name );
10
​
11
// Access the name property of sub
12
console.log( obj.sub.name );

Fullscreen

Reset Code
Run Code 
In the following example, appearance is nested in person. Further, hair is nested inside appearance. To access any properties of hair, we use dot notation to traverse the object.

We can also access array data of objects using a combination of dot notation (to access the array) and bracket notation (to access the array member). For instance, to access the string "pizza", we use dot notation to access the favoriteFoods array and bracket notation to access the string.


1
let person = {
2
    name: "Jesse",
3
    age: 35,
4
    lovesTacos: true,
5
    favoriteFoods: [ "tacos", "pho", "pizza" ],
6
    appearance: {
7
        hair: {
8
            length: "short",
9
            color: "brown"
10
        },
11
        height: "69in"
12
    }
13
}
14
​
15
// Accessing deeply nested properties
16
console.log( person.appearance.hair.length );
17
console.log( person.appearance.hair.color );
18
​
19
// Access deeply nested array data
20
console.log( person.favoriteFoods[ 2 ] );

Fullscreen

Reset Code
Run Code 
Altering Nested Properties  

You can also use dot notation to change property values or to add new properties. In the following example, we change Jesse's hair length to "medium" and add a favoriteMovie property with a value of "Iron Man".


1
let person = {
2
    name: "Jesse",
3
    age: 35,
4
    lovesTacos: true,
5
    favoriteFoods: [ "tacos", "pho", "pizza" ],
6
    appearance: {
7
        hair: {
8
            length: "short",
9
            color: "brown"
10
        },
11
        height: "69in"
12
    }
13
}
14
​
15
// change the hair length property value
16
person.appearance.hair.length = "medium";
17
​
18
// add a new property
19
person.favoriteMovie = "Iron Man";
20
​
21
console.log( person );

Fullscreen

Reset Code
Run Code 
Object Methods  

A Method is a function nested inside of an object. To discuss the importance of methods, let's simplify our person object and add a new method sayHello. Methods are accessed using dot notation, but you still have to call the method using the parenthesis (). person.sayHello will return the function. person.sayHello() will execute the method and return whatever is returned from the method.


1
let person = {
2
    name: "Jesse",
3
    age: 35,
4
    sayHello : function(){
5
        return "Howdy, my name is Jesse.";
6
    },
7
    tellAge : function(){
8
        return "I am 35.";
9
    }
10
};
11
​
12
// returns the actual method without running it
13
console.log( person.sayHello );
14
​
15
// calls the method and returns the result
16
console.log( person.sayHello() );
17
console.log( person.tellAge() );

Fullscreen

Reset Code
Run Code 
The this Keyword  

A flaw in the person object is that sayHello and tellAge have hard-coded some of the other properties into strings. sayHello returns the string "Howdy, my name is Jesse.", but "Jesse" is the value of person.name. tellAge returns the string "I am 35." but 35 is the value of person.age. What if I change name or age later in the application? person.age and person.name will be updated, but the strings returned from the methods will not.

In the following code sample, updating name and age doesn't change the output of sayHello or tellAge.


1
let person = {
2
    name: "Jesse",
3
    age: 35,
4
    sayHello : function(){
5
        return "Howdy, my name is Jesse.";
6
    },
7
    tellAge : function(){
8
        return "I am 35.";
9
    }
10
};
11
​
12
// calls the method and returns the result
13
console.log( person.sayHello() );
14
console.log( person.tellAge() );
15
​
16
// update the property values
17
person.name = "Clive";
18
person.age = 52;
19
​
20
// calls the method and returns the result
21
console.log( person.sayHello() );
22
console.log( person.tellAge() );

Fullscreen

Reset Code
Run Code 
Hard coding data isn't a great practice, so we need a way to make the strings returned from our method more dynamic. To start, let's only replace parts of the string with our property values.


1
let person = {
2
    name: "Jesse",
3
    age: 35,
4
    sayHello : function(){
5
        return "Howdy, my name is " + person.name + ".";
6
    },
7
    tellAge : function(){
8
        return "I am " + person.age + ".";
9
    }
10
};
11
​
12
// calls the method and returns the result
13
console.log( person.sayHello() );
14
console.log( person.tellAge() );
15
​
16
// update the property values
17
person.name = "Clive";
18
person.age = 52;
19
​
20
// calls the method and returns the result
21
console.log( person.sayHello() );
22
console.log( person.tellAge() );

Fullscreen

Reset Code
Run Code 
Awesome. Now, if we change person.name or person.age, then our methods will return strings that reflect those changes. Accessing property values can open a lot of doors for creating dynamic objects that do a lot of cool stuff. However, we still have a bit of a problem.

The Problem With Direct References  

What if we pass around this object and it's assigned to a new variable? Doing so isn't uncommon. Once you start working with arrays of objects, it becomes prevalent. In the following example, we'll create person and assign it to a new variable friend. Then, we'll overwrite person with a string. The object will still exist, but it will be stored in friend. person also still exists, but it now holds a string. We've created the object, but shuffled it between two variables.


1
let person = {
2
    name: "Jesse",
3
    age: 35,
4
    sayHello : function(){
5
        return "Howdy, my name is " + person.name + ".";
6
    },
7
    tellAge : function(){
8
        return "I am " + person.age + ".";
9
    }
10
};
11
​
12
// Assign a reference to the object in person
13
// to friend
14
let friend = person;
15
​
16
// Assign some other value to person
17
person = "New thing";
18
​
19
// calls the method and returns the result
20
console.log( friend.sayHello() );
21
console.log( friend.tellAge() );
22
​
23
// log the friend object
24
console.log( friend );

Fullscreen

Reset Code
Run Code 
Uh, oh. Our functions still work, but they reference person.name and person.age. This is a problem because person isn't the object anymore. person is now a string and strings don't have name or age properties. Because person no longer has these properties, its returning undefined.

We need a more portable way of passing around objects so that they don't directly reference variable names. Luckily, JavaScript provides a keyword to do only that.

this  

The this keyword acts as a stand-in for the object that is calling the property.

this is a complex subject with several caveats. In truth, it confuses a lot of professional developers, so we aren't expecting you to master its intricacies. For now, you need to wrap your head around the fact that this refers to the object doing the calling. You should use the this keyword when writing methods. When you use this, you are saying "this object."

Let's revisit our previous example and rework sayHello and tellAge to use this instead of directly access properties through the variable name. In the return strings for sayHello and tellAge, person.name and person.age have been replaced with this.name and this.age. When the object is created, this refers to person. Once person has been assigned to friend, then both person and friend can call the methods.

If person calls the methods (person.sayHello()), then this refers to person. If friend calls the methods (friend.sayHello()), then this refers to friend.

Because this is flexible enough to know what object is calling the method, we can safely overwrite person and still call the methods on friend.


1
let person = {
2
    name: "Jesse",
3
    age: 35,
4
    sayHello : function(){
5
        return "Howdy, my name is " + this.name + ".";
6
    },
7
    tellAge : function(){
8
        return "I am " + this.age + ".";
9
    }
10
};
11
​
12
// Assign a reference to the object in person
13
// to friend
14
let friend = person;
15
​
16
// Calls the method and returns the result
17
console.log( person.sayHello() );
18
console.log( friend.sayHello() );
19
​
20
// Assign some other value to person
21
person = "New thing";
22
​
23
// calls the method and returns the result
24
console.log( friend.sayHello() );
25
console.log( friend.tellAge() );
26
​
27
// log the friend object
28
console.log( friend );

Fullscreen

Reset Code
Run Code 
Conclusion  

Objects are a complex subject with lots of pitfalls and considerations. Spend some time with these examples and play with the code. Try to access some of the properties in the examples and to run methods. With these basic building blocks, you can now create complex objects. Nesting objects can be an expressive way of describing complex ideas and things. Use the this keyword to make your methods more flexible.

Again, this is a complex subject. You'll need to work with it in multiple contexts to grasp the intricacies of its power. Give yourself the time to explore it and you'll become accustomed to seeing it in your code.
