# File I/O in Java  

You may need to read and write to files for many reasons:

* Maybe your program creates some CSV (comma separated value) data that you output to a file and other users use this data.

* You may create a debugging log file for your program.

* Your application may work with image data.

* You may need to grab JSON or XML data.

* etc.

In any case, Java has good built-in functions to write to (and read from) files.

## File Output - Writing To File

We will use the `File` and `FileWriter` classes (in the java.io package). The `File` class represents the file we'll be writing to - we have to provide it the file name. The `FileWriter` class will handle the heavy lifting of copying data and so on - all we have to do is point it to the `File` and have it get started.

**Call To Action**: Copy the code below into an IntelliJ project and run it.

```java
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

public class FileIOExample {

    public static void main(String[] args) {
        try {
            File file = new File("greetings.txt");
            FileWriter fileWriter = new FileWriter(file);
            fileWriter.write("Hello World!\n"); //Very simple!
            fileWriter.write("File IO is cool and not scary!");
            fileWriter.close(); //close() cleans up and commits changes
        } //If Java doesn't find the file it will create it for us
        catch (IOException ex) { //A general exception that covers many errors
            ex.printStackTrace();
        }
    }
}
```

You should see a `greetings.txt` file appear with the greeting we wrote.

## File Input - Reading From File 

In practice, reading from a file is usually a bit more complex because the data from the file might be interpreted and parsed as its read. For now, we won't worry about parsing the data.

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class FileIOExample2 {

    public static void main(String[] args) {
        String[] fileContents = getFileContents("list.txt");

        for (String line : fileContents) {
            System.out.println(line);
        }
    }

    //Returns the contents of the given file as a String[] separated by lines
    //If it can't find the file it will return null after printing an error
    public static String[] getFileContents (String fileName) {
        File file = new File (fileName);
        try {
            Scanner fileScanner = new Scanner(file);
            List<String> fileContents = new ArrayList<>();
            while (fileScanner.hasNext()) {
                fileContents.add(fileScanner.nextLine());
            }
            return fileContents.toArray(new String[0]); //Converts the list to an array
        } //Since this time we are asking for data back from the file
        //We have to specify what to do if it isn't found
        catch (FileNotFoundException ex) {
            System.out.println("Could not find file *" + fileName + "*");
            ex.printStackTrace();
            return null;
        }
    }
}
```

**Call To Action**: Copy the code above into an IntelliJ project and create a file called `list.txt`. Add some content to it, for example:

```sh
apples
bananas
raspberries
strawberries
```

When you run the code above, you should see those lines printed out.

---

# Overriding toString()

In Java, all classes extend the `Object` class. A class method of `Object` is `toString()`, which converts the object into a String.

```java
public class Test {
    public static void main (String[] args) {
        Object o1 = new Object();
        System.out.println(o1.toString());
    }
}
```

By default, the `toString` will print the Object's class and its address in memory. Running the code above you should see a result that looks like this: `java.lang.Object@4aa298b7`, taking the basic form `<fully qualified classname>@<memory address>`.

Most of the time, this information isn't useful - we don't interface directly with memory addresses in Java. When you define new classes, it can be helpful to override `toString()` to provide *useful* information for debugging or displaying to the user.

```java
public class Spaceship {
    String name;
    int numLasers;
    int numBombs;
    boolean shieldsUp;

    public Spaceship(String name, int numLasers, int numBombs, boolean shieldsUp) {
        this.name = name;
        this.numLasers = numLasers;
        this.numBombs = numBombs;
        this.shieldsUp = shieldsUp;
    }

    public static void main(String[] args) {
        Spaceship s1 = new Spaceship("Wuddship", 10, 2, true);
        Spaceship s2 = new Spaceship("Enterprise", 15, 0, false);

        System.out.println(s1.toString());
        System.out.println(s2.toString());
    }

    public String toString () {
        String response = name;
        response += "\nLasers: " + numLasers;
        response += "\nBombs: " + numBombs;
        if (shieldsUp) {
            response += "\nShields UP";
        } else {
            response += "\nShields DOWN";
        }
        return response;
    }
}
```

Here we have a Spaceship class with a few different variables. We have overridden the `toString()` method to print out those variables. Note that if we hadn't overridden the function, the behavior would be similar to the class@memory that we saw above.

`System.out.println(s1.toString())` could have been more simply written as `System.out.println(s1)`. Java will automatically call `toString` on an object. How nice!
