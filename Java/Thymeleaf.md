# Thymeleaf Basics

Thymeleaf is a server-side Java template engine. Thymeleaf templates look much like regular HTML pages but provide additional attributes for HTML elements that can be processed on the server and then sent to the client as static files.

You can read about the details for each of the modes in the [Thymeleaf Documentation](http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html). In this article, we will discuss the basics of Thymeleaf templates and the general syntax for creating dynamic HTML templates.

## Standard Expression Syntax

Thymeleaf has several different types of **Standard Expressions**. They are used to handle the most common selection and output requirements for templates. Thymeleaf includes several types of Standard Expressions

* `${...}` Variable Expressions - Selects attributes from model objects

* `@{...}` Link (URL) Expressions - Used to build URLs for use in links and anchor tags

To read more about these and other types of standard expressions, visit the [Standard Expression syntax](http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#standard-expression-syntax) docs on the Thymeleaf site.

## A First Look at Thymeleaf  

To get started, clone the following repo to a location of your choice. This repo includes starter project files:

```sh
git clone https://github.com/gpratte-tiy/spring-boot-template.git
```

Once you've got the project file set up and opened in your IDE, create the file `src/main/resources/templates/hello.html` and include the following HTML. This file is our Thymeleaf template. We will add Thymeleaf Standard Expressions to this file. When the client renders the HTML page, Thymeleaf will access the `Model` and use the information to create a static HTML document.

```html
<!DOCTYPE HTML>
<!--This line is where Thymleaf is "imported".
If you add "th" tags to your file without this line, IntelliJ
will suggest this import which you can confirm with alt-enter.-->
<html xmlns:th="http://www.thymeleaf.org">

<head>
    <title>Working with Thymeleaf</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
    <!-- Template element TBD-->
    <h1>Hello</h1>
</body>
</html>
```

Then, create    src/main/java/com/example/HelloController.java` and include the following class code. `HelloController` contains all of the data and logic that will determine *how* `hello.html` is rendered. The `HelloController` contains one `hello` endpoint method for rendering `hello.html` that takes single `Model` object. The `model` contains all of the data that is passed into `hello.html`.

The `RequestMapping` annotation maps the root URL path to `HelloController.hello()`.

```java
package com.example;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloController {

    @RequestMapping("/")
    public String hello(Model model) {
        return "hello";
    }
}
```

`th:text`

Add a value to the model in the `hello` method of the `HelloController`. The method should look like this:

```java
public String hello(Model model) {
        model.addAttribute("name", "John Doe");
        return "hello";
    }
```

Then, modify the `h1` tag of `hello.html` to include a Variable Expression that renders the `name` attribute of the Model. It should look like this:

```html
<h1>Hello <span th:text="${name}"></span></h1>
```

Run the application and visit `http://localhost:8080/` in your browser. To see the rendered HTML, view page source or inspect the HTML in the Developer Tools.

This example is a quick and dirty demonstration for how templates access properties within Thymeleaf. By adding a `name` property in the `HelloController`, we can access that property in the template using a Variable expression.

### More `th:text`  

Let's expand on our example and add a `Person` object to the model. Add the class provided to your project.

Person.java:

```java
public class Person {
    private String name;
    private boolean soldier;
    private LocalDate dob;
    private List<String> nicknames;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public boolean isSoldier() {
        return soldier;
    }
    public void setSoldier(boolean soldier) {
        this.soldier = soldier;
    }
    public LocalDate getDob() {
        return dob;
    }
    public void setDob(LocalDate dob) {
        this.dob = dob;
    }
    public List<String> getNicknames() {
        return nicknames;
    }
    public void setNicknames(List<String> nicknames) {
        this.nicknames = nicknames;
    }
}
```

Update your "hello" endpoint:

```java
@RequestMapping("/")
public String hello(Model model) {
    model.addAttribute("name", "John Doe");

    Person person = new Person();
    person.setName("Ricardo");
    person.setDob(LocalDate.now());
    person.setSoldier(true);
    List<String> nicknames = new ArrayList<>();
    nicknames.add("Enforcer");
    nicknames.add("King Fish");
    person.setNicknames(nicknames);
    model.addAttribute("person", person);

    return "hello";
}
```

Now, update `hello.html` to include the following unordered list beneath the `h1` tag. The markup in `hello.html` should be as follows:

```html
<h1>Hello <span th:text="${name}"></span></h1>
<br/>
<ul>
    <li>Name: <span th:text="${person.name}"></span></li>
    <li>Soldier: <span th:text="${person.soldier}"></span></li>
    <li>DOB: <span th:text="${person.dob}"></span></li>
    <li>Nicknames: <span th:text="${person.nicknames}"></span></li>
</ul>
```

### th:each  

`th:each` allows us to iterate over a list of values. Define an enumeration `Day` in the `HelloController` class.

```java
enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}
```

Add the enumeration as an attribute of the model passed to the `hello` method.

```java
model.addAttribute("days", Day.values());
```

Then, add the HTML below to `hello.html` below the unordered list. In the following template code, the enumeration is passed in as `days` (plural). This attribute is accessible via a Variable Expression.

```html
<br/>
<h3>Select a day</h3>
<select>
    <option th:each="day : ${days}"
            th:value="${day}"
            th:text="${day}"></option>
</select>
```

`th:each` allows us to access each enumerated value in `days` as `day`. For each value in `days`, the template will render an `option` tag with a `value` property and text. (You can think of this as being analogous to a for-each loop).

Run your Spring App to make sure everything works. You should see a select box with seven options.

## Thymeleaf Tips  

**Tip 1**: Thymeleaf is very persnickety about HTML and insists that all tags be closed. For example, the HTML template IntelliJ generates for you includes `<meta charset="UTF-8">` (an unclosed tag). Thymeleaf wants you to close all of your tags: `<meta charset="UTF-8"/>`.

**Tip 2**: Don't use dashes in variable names (like `image-strings`). Thymeleaf can interpret this as a subtraction operator.

## Conclusion 

Thymeleaf has many more features for developing templates, but what you've seen here is the core of how to connect a template file to a controller file. Thymeleaf is a powerful tool for creating dynamic HTML. We've seen examples of how to alter HTML properties such as "value" and "text", but this concept can be extended to alter any HTML property - for example, the "src" property of an `img` tag.
