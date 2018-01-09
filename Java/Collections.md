# Java Collections

Java arrays can store multiple instances of the same type. This feature is useful for storing high volumes of similar data. A good example of this is an array of ints or objects:

```
int[] numbers = new int[5];
User[] users = new User[2];
```

By now you should be familiar with arrays and probably have already noticed their biggest flaw: inflexibility. Once we declare the array, we can't change the size without making an entirely new array. Then we'd have to copy over all the data from the array. What a hassle.

To overcome the limitations of arrays, Java provides a whole group of classes to store data in more useful and flexible ways.

In this article, we will discuss Java Collections.

Java Collections Framework  
The Java Collections Framework (JCF) is a library of classes and interfaces designed to facilitate the storage and manipulation of groups of objects. The JCF has too many classes to cover in this article, so we'll look at a few of the most common.

Interfaces and Classes  
The instantiation of Collection classes will follow a pattern because of the architecture of the library. The JCF includes several interfaces which are implemented by several classes. For instance, the List interface is implemented by ArrayList, LinkedList, Vector. Additionally, some classes extend other classes. For instance, Stack extends Vector.

This article has a helpful chart of the JCF hierarchy.
List  
Instantiating collections is demonstrated in the following example using the List class. List is a flexible data structure used to store objects of the same type. In the following example, the variable message is defined using the List keyword. message is set to new ArrayList<>() because ArrayList is a class that implements the List interface. <String> specified which type of object to store in the list.


1
import java.util.List;
2
import java.util.ArrayList;
3
​
4
public class CollectionExample{
5
    public static void main( String[] args ){
6
        List<String> messages = new ArrayList<>();
7
​
8
        // Add 3 Strings to the list
9
        messages.add("Hello!");
10
        messages.add("Order more pizza");
11
        messages.add("Need napkins now!");
12
​
13
        // Remove the second String
14
        messages.remove(1);
15
​
16
        // Print the second String
17
        System.out.println( messages.get( 1 ) );
18
    }
19
}

Fullscreen

Reset Code
Run Code 
ArrayList is an implementation of the List interface. It should be your go-to List when you need to store data (LinkedList is another List implementation). Behind the scenes, ArrayLists use arrays - but we don't need to interact with the array directly. As you see, no [] in sight.

Access objects in the List with get(int index), and add new items with add(). Since we've used String as the type for this List, get() will return a String, and add() requires a String argument. To remove Objects from the list, use the remove() method.

Map  
The Map interface provides behaviors analogous to "dictionaries" or "hashes" in other languages. Maps store key/value pairs. Again, Map is an interface that is implemented by many classes, including (but not limited to) HashMap, AbstractMap, and TreeMap.

In the following example, a HashMap is used to create a dictionary. In a dictionary, the "key" is the word, and its associated "value" is the definition of that word.


1
import java.util.HashMap;
2
import java.util.Map;
3
​
4
public class MapXample2 {
5
    public static void main(String[] args) {
6
        Map<String, String> dictionary = new HashMap<>();
7
​
8
        dictionary.put("chromosome", "a threadlike structure of nucleic acids" +
9
                " and protein found in the nucleus of most living cells," +
10
                " carrying genetic information in the form of genes");
11
​
12
        dictionary.put("hammer", "a tool with a heavy metal head" +
13
                " mounted at right angles at the end of a handle");
14
​
15
        System.out.println(dictionary.get("chromosome"));
16
        System.out.println(dictionary.get("genetics"));
17
        System.out.println(dictionary.get("hammer"));
18
    }
19
}

Fullscreen

Reset Code
Run Code 
Of course, the dictionary isn't magical! It only knows the words we teach it. We add items to maps using the put() method, which accepts two arguments - the key and the value.

The get() method is used to access a map, but unlike the get() method for Lists, which has one parameter. The get() parameter is determined by the first generic parameter in the Map interface.

The Map interface has two generic parameters Map<String, String>. In this case, they are both Strings, but they could be any type. When the types aren't the same, the order does matter: the first generic is the key and the second is the value.

Here are some examples of maps that we might use in various applications.

Map<String, User> userMap; //A map to look up Users by username
Map<User, List<String>> userMessages; //A map between users received messages
Map<Boolean, String> trueFalseStatements; //A map that sorts statements into t/f
HashMaps are a good default implementation of Map. In general, Maps are an excellent solution and perform well when the key/value pairs are needed.

Consider the following problem: you're writing an application for bank tellers. A customer will walk up and give the teller an account number. The teller will then need to be able to look up their account information.

If we modeled this with a List of Accounts it would look something like this:

List<Account> accountList = new ArrayList<>();
public Account findAccountByNumber (int accountNumber) {
  for (Account account : accountList) {
    if (account.getAccountNumber() == accountNumber) {
      return account;
    }
  }
  return null;
}
If we have tens or hundreds of thousands of accounts, this is a highly inefficient approach. We will have to iterate through on average half of those accounts, checking to see "are we there yet? are we there yet?" Maps provide a much better solution:

Map<Integer, Account> accountMap = new HashMap<>();
public Account findAccountByNumber (int accountNumber) {
  return accountMap.get(accountNumber);
}
Because of the way the get() function works behind the scenes, this method won't have to search through a list of 100,000 accounts. The Map will either return the Account, or null.

Set  
The Java "Set" collection models the mathematical concept of a "set." In math, sets can contain anything (often sets of numbers) but each element must be unique. In other words, an element can't be in the set twice. For example, a set cannot contain the numbers 3, 5, 5 because 5 is in the Collection twice. Sets won't allow two of the same object. The set would be reduced to 3, 5.

Try playing around with the code below by adding 5 to the collections.


1
import java.util.*;
2
​
3
public class SetXample {
4
​
5
    public static void main(String[] args) {
6
        List<Integer> numList = new ArrayList<>();
7
        Set<Integer> numSet = new HashSet<>();
8
​
9
        numList.add(3);
10
        numList.add(5);
11
​
12
        numSet.add(3);
13
        numSet.add(5);
14
​
15
        System.out.println("List:" + numList);
16
        System.out.println("Set:" + numSet);
17
​
18
        while (true) {
19
            System.out.println("What number to add to the collections?");
20
            int answer = Integer.parseInt((new Scanner(System.in)).nextLine());
21
            numList.add(answer);
22
            numSet.add(answer);
23
​
24
            System.out.println("List:" + numList);
25
            System.out.println("Set:" + numSet);
26
        }
27
    }
28
}

Fullscreen

Reset Code
Run Code 
5 got added to the List, but the Set remained the same. Set does not throw an error - the duplicate element just isn't added. Sets are a useful data structure when you want "uniqueness" in a collection.

In the following example, six people have entered a lottery for a prize. We want to pick three lucky winners using the Random class.


1
import java.util.HashSet;
2
import java.util.Random;
3
import java.util.Set;
4
​
5
public class SetXample2 {
6
​
7
    public static void main(String[] args) {
8
        String[] lotteryNames = {"Johnny", "Jason", "Lily", "Sandra", "Garry", "Nicole"};
9
        Set<String> winnersNames = new HashSet<>();
10
        Random random = new Random();
11
        while (winnersNames.size() < 3) {
12
            int index = random.nextInt(lotteryNames.length);
13
            winnersNames.add(lotteryNames[index]);
14
        }
15
        System.out.println(winnersNames);
16
    }
17
}

Fullscreen

Reset Code
Run Code 
In this example, the Set interface does all of the work. Instead of worrying about keeping a list of indexes that I've already used, I continue attempting to add names until the set has three names in it. If a name comes up multiple times, it won't be inserted again because Set won't allow it. This method will always return three different names (no double winners).

(Note that this approach is probably not the most scalable option; if we had to pick 1,000 winners out of 2,000 people, we'd end up looping through a lot of extra times. It's just an example of the use of Sets).

Default Collections  
HashSet is a good default implementation of Set. Similarly, ArrayList is a good default implementation for List, as HashMap is for Map. These implementations have their pros and cons, but these 3 classes will serve you well:

List - ArrayList
Map - HashMap
Set - HashSet
Performance  
Different implementations of these collections will sometimes perform better or worse. Performance is a measure of speed. How long does a process take? How quickly can I sift through 1,000,000 objects? All operations (adding items to a collection, removing items from a collection, iterating through a collection) take time. The amount of time depends on the implementation of the collection.

For example, LinkedList is fast at adding and deleting elements, but slow to get specific elements. ArrayList is the opposite, performing add and delete more slowly, but accessing individual elements quickly and easily. It's important to keep in mind that "fast" and "slow" are relative terms. You will have trouble noticing any performance difference between an ArrayList and a LinkedList with eight elements since computers are in general very fast. Once your application scales up to having thousands or millions of objects, speed can make a big difference.

In general, the three defaults above are good. If you need a List, unless you've done some research and considered why a LinkedList is going to perform better for your program, you should use ArrayList. Similarly, use HashMap and HashSet. To look up more classes, visit The Collection Docs. You can find lots of information available online about the pros and cons of various collections.

Benchmarks  
The code below adds the numbers 1 to 1 million to the different collections we've discussed. It also tracks the time to complete the operations and prints the totals.


1
import java.util.*;
2
​
3
public class Benchmarkin {
4
​
5
    public static void main(String[] args) {
6
        final int ONE_MILLION = 1000000; //Note use of final/const variable
7
        List<Integer> arrayList = new ArrayList<>();
8
        List<Integer> linkedList = new LinkedList<>();
9
        Set<Integer> hashSet = new HashSet<>();
10
        Map<Integer, Integer> hashMap = new HashMap<>();
11
​
12
        long startTime;
13
        long endTime;
14
​
15
        startTime = System.currentTimeMillis();
16
        for (int i = 0; i < ONE_MILLION; i++) {
17
            arrayList.add(i);
18
        }
19
        endTime = System.currentTimeMillis();
20
        long arrayListAddTime = endTime - startTime;
21
​
22
        startTime = System.currentTimeMillis();
23
        for (int i = 0; i < ONE_MILLION; i++) {
24
            linkedList.add(i);
25
        }
26
        endTime = System.currentTimeMillis();
27
        long linkedListAddTime = endTime - startTime;
28
​
29
        startTime = System.currentTimeMillis();
30
        for (int i = 0; i < ONE_MILLION; i++) {
31
            hashSet.add(i);

Fullscreen

Reset Code
Run Code 
Notes:

This is not a rigorous scientific experiment; we're only testing one operation (add/put), and only with one particular data type.
The outcomes of these benchmarks can change wildly based on the optimization. (Basically, as your code gets compiled into Java byte code, it will attempt to make it run as fast as possible)
Nevertheless, you should be able to see that ArrayLists are very fast
