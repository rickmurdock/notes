# Install Party  

The purpose of this install party is to get you the tools that you will need to write and run programs in Java. You may have already done some of these tasks. If so, double check that you have installed your tools and that you understand where to find everything.

## Java  

The Java Development Kit (JDK) is a whole environment of tools you'll need to develop Java applications. It includes most of what you will need to create, compile, and run Java code. Go ahead and download the Java Development Kit (JDK) using homebrew:

First, double check that we have Homebrew and cask installed.

```sh
# verifies we have cask installed
brew tap caskroom/cask
```

Then, install Java.

```sh
brew cask install java
```

Once the JDK installation is complete, open up the terminal and type:

```sh
javac -version
```

That should display something along the lines of:

```sh
javac 1.8.0_25
```
Don't worry if the version ("0_25" above) does not match.

To see where Java has been installed, type:

```sh
which java
```

You should see something like this:

```sh
/usr/bin/java
```
You now have the JDK installed and configured, which means you can now "speak" Java and your computer will understand you!

## IntelliJ 

Next, we're going to download an Integrated Development Environment (IDE), IntelliJ. You can think of IntelliJ, and other IDEs, as text editors with **superpowers**. IDEs have powerful features to assist in software development.

IntelliJ will become an essential tool for building Java Applications.

```sh
brew cask install intellij-idea-ce
```

This command should install IntelliJ in your Applications folder. If not, then search for it with "Spotlight" and find it on your computer. Move it to Applications.

## Running Your first IntelliJ Project  

* Start IntelliJ

* Select "Start a new project."

* If a is JDK selected, click on "New" and navigate to where you installed the JDK

* With "Java" selected as the type of project on the left panel, and the JDK selected, click "Next."

* Check the box labeled "Create project from template" and select "Command Line App"

* For project name, location and package name, use whatever you'd like. Note that the package name cannot have spaces in it. Something along the lines of "com.theironyard.installparty" will do, but feel free to customize to your liking, and create the project

* IntelliJ should now start and show you the contents of a file - ignore this for now

* Go to the "Run" menu and select the "Run Main" option.

* IntelliJ should start doing things and display a console output at the bottom

## Maven

Maven is a dependency management system that we'll be using later. For now, we just want to install it:

```sh
brew install maven
```

To check that maven is installed and configured properly, run `mvn -version` from the command line.

## Closing  

Now that your computer can speak Java and you're all set with your IDE, we are ready to begin our Java adventure!

---

# Compiling a .java File into Byte Code  

## Source Code

When working with compiled code, like Java, you must make a distinction between the *code that you write* and *the code that Java can run.*

**Source Code is the collection of text files that we write when we are programming in Java.** However, the Java Virtual Machine (JVM), reads Byte Code.

**Byte code is the compiled version of our source code.** Byte code isn't human readable; it's just for the JVM. Here is an example of byte code:

```sh
//These are just a few lines of Java Byte code to go through a for loop.
iconst_2
istore_1
iload_1
sipush  1000
if_icmpge       44
iconst_2
istore_2
iload_2
iload_1
//Thank goodness we don't have to program in byte code!
```

Because we write source code and the JVM reads byte code, our source code has to be compiled into byte code. Whenever we write a Java application, we must follow a few steps to get our application to run:

1. First, write our source code files. For Java, our code lives in `.java` files.

2. Run our source code through a compiler (`javac`). Running this command will convert our source code into byte code and produce `.class` files.

3. Run the new file using the `java` command line tool.

These three steps weren't necessary for scripting languages like JavaScript or Python, but they are essential for compiled languages.

## Compiling Your First Application

You haven't yet learned the Java programming language, but in this article we will practice writing a Java class, compiling it, and then running the resulting file.

In the next few steps, we'll create a `.java` file, compile it and run it. You will have to save the source code file and the byte code file somewhere on your computer. You can save this file anywhere, but here is a simple recommendation:

Navigate to your `Documents` folder and create a folder called `JavaProjects`. Then, navigate to the `JavaProjects` folder and create a new folder called `Party`. Navigate to that folder and save `Party.java` (see below) there. When you compile `Party.java`, it will generate another file in the same directory as `Party.java`. Having a dedicated folder (Party) for this project will help keep your code organized.

Whenever you create a new project, put it in a project folder in `JavaProject`.

Back to `Party.java`. Create a new file in your text editor of choice. We aren't using IntelliJ just yet. If you are familiar with a text editor like Atom or Sublime Text, then please use that. Save the file as "Party.java".

### Step 1 - Create the Source Code  

Copy the code in the following snippet into `Party.java` and save the file to the `Documents/JavaProjects/Party folder`.

```java
public class Party {
    public static void main(String[] args) {
        System.out.println("It's a big ol' Java party!");
    }
}
```

### Step 2 - Compile the Source Code  

Next, open the terminal and navigate to `Documents/JavaProjects/Party`. Use the "java compile" command `javac` to compile the code.

```sh
    javac Party.java
```

The compiler generates a `Party.class` file that consists of byte code. It will place this file in the same folder as `Party.java`.

### Step 3 - Run the Java Application

Finally, run the application by using the command `java` followed by your class name. The Java Virtual Machine (JVM) will translate it and successfully execute the application.

```sh
    java Party
```

## Conclusion

Humans write source code to author software. The JVM reads and interprets Byte Code. In this class, much of the code you write will be `.java` files containing Java code. More complex projects will also include other text files with extensions like `.js`, `.xml`, `.json`, `.pom`, image files, and other resources.

Source Code needs to be compiled into Byte Code using `javac`. Once the `Byte Code` is created, its run using the `java` command.
