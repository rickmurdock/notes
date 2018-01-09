# Overview

It is very common in web applications to give unrestricted access to some web pages (e.g. home page, product information, here is something shiny and cool to look at) but require authentication for others (e.g. save to wishlist, view my orders.)

## Lesson Goals & Expectations

As you are going through this lesson, you will see some code you don't understand, along with a lot of external links to Spring documentation. The expectation is NOT that you will visit every link and spend hours reading about Spring security in order to understand these concepts 100%. Instead, use this lesson as a reference and "big picture" introduction to the topic. We'll be setting up a basic security example - to extend that, you will need to do some reading on your own.

## Authentication and Authorization (A & A)  

There are many aspects to security. This lecture concerns itself with user *authentication* (i.e. user id and password) and what web pages they are *authorized* to access.

## Play Along  

The best way to understand how security works is to run the code as it progresses from "no security" to "authentication to authorization".

The code will be using the `persondb` that has been shown in previous lessons. If you have not created this database and person table then see the "Setup Postgres" section in the "JDBC and Transactional" lesson.

Get the code by cloning `https://github.com/gpratte-tiy/a-and-a.git` and move to the `without-authentication` branch.

## pom.xml  

It is always a good idea when cloning a project to have a look at the dependencies in the pom.xml.

To access the database and put up webpages the following three dependencies are in the pom.xml

* Thymeleaf

* JPA

* postgres

## View and Add People  

There are three pages for the project.

The home page is shown when you hit the URL `http://localhost:8080/`. This loads the `src/main/resources/static/index.html` file.

Note that this is **not** a Thymeleaf file (it is in the `static` directory rather than `templates`).

The index page has a link to the `/persons` route in the controller, which gets a list of all people. This list is processed by the `view_people` Thymeleaf template.

This page has an `Add` link and the GET mapping for `/person` in the controller puts up the `new_person` Thymeleaf file.

Filling in the form and clicking `Submit` results in the POST mapping for the `/person` route. This calls the service to add the person. Then this route does what the `/persons` route did, return list of all people.

## Add the security dependency  

Change to the `security-dependency` branch and you will see that the security dependency has been added to the pom.xml

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

Remember when the pom.xml is changed it is always a good idea in IntelliJ to

* right click on the pom.xml -> Maven -> Reimport

Now try to hit the home page. The basic authentication window is shown. That's the default behavior for web pages in Spring when there is no other configuration.

Spring spits out a lot of messages when it begins to run. Scroll up the messages and you will find something like

* `Using default security password: 9a2a7057-dc5d-4614-a8be-66f2fe86b4e3`

Copy the password and when presented with the login window type in `user` for the username and paste the password. Spring will allow you to hit pages for some number of minutes but sooner or later it will require you to log in again. We can see that we have the beginnings of security for our app, but we will need to configure it to be more user-friendly.

## Configure Security  

On the `enable-web-security` branch you will see a new `com.example.ana.config` package with a `WebSecurityConfig` class.

**Note:** The commented out `@formatter:off` and `@formatter:on` are something that IntelliJ offers to suppress formatting. This option has to be enabled in the preferences:

* IntelliJ IDEA menu in top left -> Preferences

* Editor -> Code Style

* Check the `Enable formatter markers in comments`

This class is annotated with

* [**@Configuration**](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/context/annotation/Configuration.html) - informs Spring that this class is for (wait for it) configuration!

* [**@EnableWebSecurity**](https://docs.spring.io/spring-security/site/docs/4.2.1.RELEASE/apidocs/org/springframework/security/config/annotation/web/configuration/EnableWebSecurity.html) - the description of the class from the javadoc is "Add this annotation to an @Configuration class to have the Spring Security configuration defined in any WebSecurityConfigurer or more likely by extending the WebSecurityConfigurerAdapter base class and overriding individual methods"

`WebSecurityConfig` extends `WebSecurityConfigurerAdapter`. This is a pattern in Spring (and a great example of the power of inheritance). There are a dozen or so methods in [**WebSecurityConfigurerAdapter**](https://docs.spring.io/spring-security/site/docs/4.2.1.RELEASE/apidocs/org/springframework/security/config/annotation/web/configuration/WebSecurityConfigurerAdapter.html). By extending the class and only overriding the behaviors we want to change (two methods) we are accepting default configurations for the remaining behaviors.

The `configure()` method takes an [**HttpSecurity**](https://docs.spring.io/spring-security/site/docs/4.2.1.RELEASE/apidocs/org/springframework/security/config/annotation/web/builders/HttpSecurity.html) parameter.

As you can see "method chaining" is employed here. The reason for this is because methods of `HttpSecurity` return an instance of `HttpSecurity` (just like intermediate `stream()` operations such as `filter()` return another stream).

The methods have nice descriptive names so the code is somewhat self-explanatory at a high level. See [**HttpSecurity**](https://docs.spring.io/spring-security/site/docs/4.2.1.RELEASE/apidocs/org/springframework/security/config/annotation/web/builders/HttpSecurity.html) if you want to dig deeper.

The `configureGlobal()` method is where the username and password are checked for authentication. Presently only "user" and "password" are allowed.

One more thing of note is that [**Cross-Site Request Forgery (CSRF)**](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) is enforced. Notice that both the forms in the thymeleaf files (login and new_person) have `th:action` and `method="post"`. View the page source of either of these pages and you will see a CSRF token at the end of the form.

* `<input type="hidden" name="_csrf" value="0701354a-4f6a-42ac-bd64-cfe49d84e097" />`

**Note** that you only get this hidden token if you:

1. Are in a `form` tag

2. Use the Thymeleaf `th:action` attribute

3. Method is `post`

## Database username/password authentication

On the `database-auth` branch the project has morphed again to use database authentication.

Spring security not only has users with passwords but also employs **roles** that can permit/restrict access. We've created two database tables: one for the users and one for the roles.

Logged in

* `psql -U personuser persondb`

Created a users table

* `CREATE TABLE users (username text not null, password text not null, enabled boolean, constraint users_pkey primary key (username));` 

Created a authorities table

* `CREATE TABLE authorities (username text not null, authority text not null, constraint authorities_pkey primary key (username, authority));`

Added users

* `insert into users (username, password, enabled) values ('captain', 'serenity', true);`

* `insert into users (username, password, enabled) values ('rivertam', 'miranda', true);` 

Added roles

* `insert into authorities (username, authority) values ('captain', 'ROLE_USER');`

* `insert into authorities (username, authority) values ('rivertam', 'ROLE_USER');`

* `insert into authorities (username, authority) values ('captain', 'ROLE_ADMIN');`

Log out of the psql command line

* `\q`

Back in the `WebSecurityConfig` class, we have autowired a data source (which is a way to get to the database).

```java
@Autowired
private DataSource dataSource;
```

There a many DataSource classes to choose from - we are using `javax.sql.DataSource`.

Updated the `configureGlobal()` method with:

```java
@Autowired
public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
    auth.jdbcAuthentication().dataSource(this.dataSource)
    .usersByUsernameQuery("select username, password, enabled from users where username=?")
    .authoritiesByUsernameQuery("select username, authority from authorities where username = ?");
}
```

This way we provide the SQL query that Spring security uses to get the `user` and its `authorities`.

Now a user can log in.

## Authorization  

The `role-auth` branch demonstrates what the user can do.

There are a couple things that need to be done to wrap up these web pages, dealing with questions like:

* is the user logged in?

* can the user access a certain page?

We want to show a logout button only if a user is logged in. We could jump through some hoops and code it ourselves (e.g. add a flag to the model for each route), but luckily there is a built-in way to do this.

Add the security Thymeleaf dependency to `pom.xml`:

```xml
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-springsecurity4</artifactId>
</dependency>
```

For the two HTML pages add a logout button that will only appear if there is someone authenticated.

```html
<div sec:authorize="isAuthenticated()">
  <form th:action="@{/logout}" method="post">
    <input type="submit" value="Sign Out"/>
  </form>
</div>
```

Note that the HTML page in IntelliJ will complain about the `sec` namespace. This will not affect the Thymeleaf engine from converting the file. To add the namespace change

```html
<html xmlns:th="http://www.thymeleaf.org">
```
    
to

```html
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity4">
```

One problem to overcome is that logout by default returns to the login page. Configure a different page in the `WebSecurityConfig` by changing

```java
//...
.logout()
    .permitAll();
```

to

```java
//...
.logout()
    .permitAll()
    .logoutSuccessUrl("/loggedout");
```

and adding this route to the controller:

```java
@RequestMapping("/loggedout")
String logout(Model model) {
    List<Person> persons = personService.getAll();
    model.addAttribute("listOPeople", persons);
    return "view_people";
}
```

This shows the `view_people` HTML page when logged out.

### Restrict Access by Role  

A special admin page will be restricted to admins only. Admins will be able to do thing like delete people.

Begin by adding an Admin Only link to the persons page.

```html
<a href="/admins-only">Admins Only</a>
```

Now add a route in the controller:

```java
@GetMapping("/admins-only")
String admins() {
    return "administration";
}
```

Add a very boring `administration.html` page:

```html
<body>
<div sec:authorize="isAuthenticated()">
    <form th:action="@{/logout}" method="post">
        <input type="submit" value="Sign Out"/>
    </form>
</div>
<h1>For admin eyes only</h1>
</body>
</html>
```

Update `WebSecurityConfig` to protect the route:

```java
.antMatchers("/admins-only").hasRole("ADMIN")
```

We can access it when logged in as "captain" who is an admin but not as "rivertam".

It would be better not to show the link at all. Add

```java
sec:authorize="hasRole('ROLE_ADMIN')"
```

attribute to the `<a>` tag will cause the Thymeleaf engine to add the link to the HTML template only if the current user is logged in **and** an admin.

## Git - master branch  

The final code from the `role-auth` branch has been merged into the `master` branch.
