# What is Maven?

Maven is a tool used to build and manage Java-based projects.

## The Power of Maven  

* Maven makes the build process easy by hiding most of the details of underlying mechanisms.

* Maven uses POM (Project Object Model) files and a consistent set of plugins shared by all projects. The goal of this consistent structure is to provide a uniform build system.

* Maven provides quality project information such as change log information, dependency lists, and unit testing reports. As projects scale in size and complexity or as teams grow, this information becomes invaluable.

* Maven project management utilizes best practices to ease test driven development, release management, and issue tracking.

* Maven enables users to update their installations so that they can take full advantage of changes the most up-to-date technologies.

You can read a more in-depth breakdown of Maven's core goals and features in the [**Maven Docs**](https://maven.apache.org/what-is-maven.html).

---

# Using Maven

**Start a Maven project**

Navigate to your project directory in Terminal and create a new directory called "FirstMaven". `cd` into "FirstMaven" and run the following:

```sh
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

> The above script is pretty long. Be sure to copy all of it and paste it into terminal correctly.

This command will build a directory named "my-app" (see `DartifactID=my-app`). `cd` into "my-app":

```sh
cd my-app
```

Open "my-app" and look at the project structure. `App.java` contains `main` method. `AppTest.java` contains the tests for `App.java`. JUnit is a dependency in the `pom.xml`

### Maven in IntelliJ  

Maven exists in IntelliJ as well as the command line so you can utilize its power in both locations.

To see Maven at work in IntelliJ, import the project into IntelliJ. Expand the " External Libraries" folder in the project window. The JUnit libraries are present because of the dependency in the `pom.xml`. My including the POM file, Maven will automatically import any dependencies.

### Maven in the Command Line  

Maven also includes several commands for building Java projects via the command line. All Maven command line tools are available via `mvn`.

**Compile Java with Maven**

Maven can compile the `.java` files into `.class` files and will move those files into a compiled class folder called "target." From the "my-app" folder, run the following command:

```sh
mvn compile
```

**Run Tests with Maven**

Maven will also run unit tests. To run "AppTest," enter the following command from "my-app":

```sh
mvn test
```

**Create a JAR with Maven**

Maven will also package the application into a JAR file with the `package` command. `package` will generate several directories within "target" containing information provided by Maven. It will also generate a `.jar` snapshot.

Run the following command from "my-app":

```sh
mvn package
```

To run the `.jar` file, use the following command. Once you have run this script, you should see "Hello World!" in your Terminal prompt.

```sh
java -cp target/my-app-1.0-SNAPSHOT.jar com.mycompany.app.App
```

## A Quick Review of Maven Tools

* **compile** - Compile the Java code

* **test** - Runs the tests

* **package** - Creates the JAR

* **clean** - We didn't discuss this tool, but it will remove the files and directories generated during the Maven build process. Generally speaking, this command will delete the "target" folder.
