# JDBC (Java DataBase Connectivity)

In this lesson, you will be learning one method of talking to databases in Java. This is a "live coding" lesson, so please follow along with the bouncing ball.

JDBC stands for the Java DataBase Connectivity API. It's one of many tools in Java that allows us to communicate with a database. We will be writing standard SQL queries and the JDBC will take care of executing those queries and getting results back.

JDBC is part of Java SE (Standard Edition, or what you might think of as "core Java"). The `JdbcTemplate` class you will use later is Spring's implementation of JDBC.

## Setup Postgres  

Start the postgres server if it is not already running. (Refer back to BEF setup if needed)

Create a postgres user. Type `personpass` when prompted for a password

* `createuser personuser -l -P`

Create a database that allows the `personuser` access

* `createdb -O "personuser" persondb`

Log in

* `psql -U personuser persondb`

While logged in create a person table

* `CREATE TABLE persons (id serial not null, firstName text not null, lastName text not null, constraint person_pkey primary key (id));`

While logged in verify the table was created

* `\d` the output should be similar to

```sh
Schema |     Name      |   Type   |   Owner    
--------+---------------+----------+------------
 public | persons       | table    | personuser
 public | person_id_seq | sequence | personuser
```

Log out of the psql command line

* `\q`

## Model the Database Table  

Create a new Spring project using Initializr to hold your code. You will need to include JDBC as a dependency.

The following `Person` JavaBean models the entries in the 'persons' database table.

```java
package com.example.aggregate.model;

public class Person {
    private int id;
    private String firstName;
    private String lastName;

    //!!Add getters and setters
}
```

Notice that the package for this class is `com.example.aggregate.model`. It is common practice to put the JavaBeans that model database tables in a `domain` or `model` subpackage.

## Person Repository Interface  

We will define a `PersonRepository` interface which defines the contract we'll use to talk to the repository.

```java
package com.example.aggregate.respository;

import com.example.aggregate.domain.Person;

import java.util.List;

public interface PersonRepository {
    void add(Person person);
    Person getById(int id);
    List<Person> get();
    void update(Person person);
    void delete(int id);
}
```

Notice that this interface is in the `repository` subpackage.

## Implementing PersonRepository  

Our `PersonRepositoryImpl` class implements the `PersonRepository` interface. It is where the SQL resides and it does the actual work of talking to the database, getting results back, and mapping the row(s) of the result set to the `Person` class.

```java
package com.example.aggregate.respository;

import com.example.aggregate.domain.Person;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.stereotype.Repository;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;

@Repository
public class PersonRepositoryImpl implements PersonRepository {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    private final String INSERT_SQL = "INSERT INTO person (firstName, lastName) VALUES (?,?)";
    @Override
    public void add(Person person) {
        jdbcTemplate.update(INSERT_SQL, person.getFirstName(), person.getLastName());
    }

    private final String SELECT_BY_ID_SQL = "SELECT * FROM person WHERE id = ?";
    @Override
    public Person getById(int id) {
        return jdbcTemplate.queryForObject(SELECT_BY_ID_SQL, new PersonMapper(), id);
    }

    private final String SELECT_SQL = "SELECT * FROM person";
    @Override
    public List<Person> get() {
        return jdbcTemplate.query(SELECT_SQL, new PersonMapper());
    }

    private final String UPDATE_SQL = "UPDATE person SET firstName=?, lastName=? where id=?";
    @Override
    public void update(Person person) {
        jdbcTemplate.update(UPDATE_SQL, person.getFirstName(), person.getLastName(), person.getId());
    }

    private final String DELETE_SQL = "DELETE FROM person WHERE id=?";
    @Override
    public void delete(int id) {
        jdbcTemplate.update(DELETE_SQL, id);
    }


    // Map a row of the result set to a person object
    private static class PersonMapper implements RowMapper<Person> {
        @Override
        public Person mapRow(ResultSet rs, int rowNum) throws SQLException {
            Person person = new Person();
            person.setId(rs.getInt("id"));
            person.setFirstName(rs.getString("firstName"));
            person.setLastName(rs.getString("lastName"));
            return person;
        }

    }

}
```

There is a lot going on in this class.

### @Repository  

The class is annotated with `@Repository`. This informs Spring that this is a class that will be accessing a database. Spring translates the different exceptions from the underlying database into common spring data exceptions.

### @Autowired the JdbcTemplate  

The `JdbcTemplate` is "autowired" by spring. This is an annotation to Spring, asking Spring to set up something for us. Hence this class has a reference that Spring created "behind the scenes." (You can think of Spring as your electrician here. "Spring, I want this thing. Connect it and wire it up for me.")

### SQL Statements with Question Marks?  

The question marks ('?') in the SQL are where values are replaced. This is how Spring avoids [**SQL injection**](https://en.wikipedia.org/wiki/SQL_injection). As part of executing the query, the `?`s are replaced with the values we've specified.

### insert  

The `jdbcTemplate.update()` method for the inserting takes one parameter (the SQL) and then zero or more parameters, one for each question mark.

See the [update method Javadoc](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/jdbc/core/JdbcTemplate.html#update-java.lang.String-java.lang.Object...-)

### get by id  

The `jdbcTemplate.queryForObject()` method for getting the person by id takes two parameters

* the SQL

* a new RowMapper (explained below)

### get all  

The `jdbcTemplate.query()` method returns all the rows of the person table. It takes two parameters, the sql and a RowMapper.

### update  

Similar to the add method

### delete  

Delete by id

### PersonMapper class  

The `PersonMapper` class maps a row in the result set to a `Person` object.

## application.properties File

The postgres database connection information resides in the `application.properties` file.

```sh
spring.datasource.url=jdbc:postgresql://localhost:5432/persondb
spring.datasource.username=personuser
spring.datasource.password=personpass
```

## Person repository test  

This is a Spring/JUnit test. The annotations `@RunWith(SpringRunner.class)` and `@SpringBootTest` will cause the spring server to start and then run any method that is annotated with `@Test`.

```java
package com.example.aggregate.respository;

import com.example.aggregate.domain.Person;
import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.List;

@RunWith(SpringRunner.class)
@SpringBootTest
public class PersonRepositoryTest {

    @Autowired
    PersonRepository personRepository;

    @Test
    public void testAddGet() {
        // Get unique names every time this test runs
        String firstName = Long.toString(System.currentTimeMillis());
        String lastName = Long.toString(System.currentTimeMillis());

        Person person1 = new Person();
        person1.setFirstName(firstName);
        person1.setLastName(lastName);
        personRepository.add(person1);

        List<Person> people = personRepository.get();

        Person person2 = findInList(people, firstName, lastName);
        Assert.assertNotNull(person2);

        Person person3 = personRepository.getById(person2.getId());
        Assert.assertNotNull(person3);
        Assert.assertEquals(firstName, person3.getFirstName());
        Assert.assertEquals(lastName, person3.getLastName());
    }

    @Test
    public void testUpdate() {
        Person person1 = createTestPerson();
        personRepository.add(person1);

        List<Person> people = personRepository.get();

        Person person2 = findInList(people, person1.getFirstName(), person1.getLastName());
        Assert.assertNotNull(person2);

        String updateFirstName = Long.toString(System.currentTimeMillis());
        String updateLastName = Long.toString(System.currentTimeMillis());

        person2.setFirstName(updateFirstName);
        person2.setLastName(updateLastName);
        personRepository.update(person2);

        people = personRepository.get();

        Person person3 = findInList(people, updateFirstName, updateLastName);
        Assert.assertNotNull(person3);
        Assert.assertEquals(person2.getId(), person3.getId());
    }

    @Test
    public void testDelete() {
        Person person1 = createTestPerson();
        personRepository.add(person1);

        List<Person> people = personRepository.get();

        Person person2 = findInList(people, person1.getFirstName(), person1.getLastName());
        Assert.assertNotNull(person2);

        personRepository.delete(person2.getId());

        people = personRepository.get();
        Person person3 = findInList(people, person1.getFirstName(), person1.getLastName());
        Assert.assertNull(person3);
    }

    private Person createTestPerson() {
        // Get unique names every time this test runs
        String firstName = Long.toString(System.currentTimeMillis());
        String lastName = Long.toString(System.currentTimeMillis());

        Person person = new Person();
        person.setFirstName(firstName);
        person.setLastName(lastName);
        return person;
    }


    private Person findInList(List<Person> people, String first, String last) {
        // Find the new person in the list
        for (Person person : people) {
            if (person.getFirstName().equals(first) && person.getLastName().equals(last)) {
                return person;
            }
        }
        return null;
    }
}
```

The `PersonRepository` is auto wired and each test has an `@Test` annotation.

* `testAddGet()` calls the methods on the repository to add and get all people. It then asserts that the person added is in the list.

* `testUpdate()` adds a person, gets it from the list, updates it and then asserts the updates worked.

* `testDelete()` adds a person, gets it from the list, deletes it and then asserts it is no longer in the list of all people.

The test can be run by right clicking on the on the class and then selecting Run. See the results in the test runner window.

---

# Transactional

This lesson will discuss transactions, a way to group up changes to a database.

## Database Transactions  

When a method inserts, updates and/or deletes more than once the changes need to be wrapped in a **transaction**. When changes are wrapped up together in a transaction, what we're telling Spring is:

"These changes are all one unit. If there's a problem with **any** of them, I want you to undo **all** changes and roll back to where I was before I gave that command."

This will be accomplished by setting the transaction boundaries in a **service layer**. First, let's see what happens without using a transaction.

Define a new `PersonService` interface, which has the same methods as the `PersonRepository` interface, with one new method: `void add(List<Person> people)`. If the list has more than one person then multiple rows will be inserted in the person table.

```java
package com.example.aggregate.service;

import com.example.aggregate.model.Person;

import java.util.List;

public interface PersonService {
    void add(Person person);
    void add(List<Person> people);
    Person getById(int id);
    List<Person> get();
    void update(Person person);
    void delete(int id);
}
```

Reminder/Explanation: `The PersonServiceImpl` class implements the `PersonService` interface (so `PersonServiceImpl` IS-A `PersonService`). It contains an @Autowired `PersonRepository` (so `PersonServiceImpl` HAS-A `PersonRepository`).

```
package com.example.aggregate.service;

import com.example.aggregate.model.Person;
import com.example.aggregate.respository.PersonRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class PesonServiceImpl implements PersonService {
    @Autowired
    PersonRepository personRepository;

    @Override
    public void add(List<Person> people) {
        for (Person person : people) {
            personRepository.add(person);
        }

        // Functional programming alternative:
        //people.stream().forEach(e -> personRepository.add(e));
    }

    // other methods left out for brevity
}
```

The `PersonServiceImpl` methods just "pass through" the calls to the repository.

Notes:

* The `PersonServiceImpl` is annotated with @Service. You can read about that [here](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/stereotype/Service.html).

* The `PersonService` and `PersonServiceImpl` are in a new `service` subpackage.

## Test the new method  

The `PersonServiceTest` calls the methods in the the service class (not the repository class). For brevity the test will only test the new method that adds a list of persons.

```java
package com.example.aggregate.service;

import com.example.aggregate.model.Person;
import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import org.springframework.dao.DataIntegrityViolationException;

import java.util.ArrayList;
import java.util.List;

import static com.example.aggregate.common.PersonUtils.createTestPerson;
import static com.example.aggregate.common.PersonUtils.findInList;

@RunWith(SpringRunner.class)
@SpringBootTest
public class PersonServiceTest {

    @Autowired
    PersonService personService;

    @Test
    public void testTransactional() {
        Person person1 = createTestPerson();
        Person person2 = createTestPerson();
        List<Person> people = new ArrayList<>();
        people.add(person1);
        people.add(person2);

        // Cause an error
        person2.setFirstName(null);
        try {
            personService.add(people);
            Assert.assertFalse("Expected an exception to be thrown");
        } catch(DataIntegrityViolationException e) {
            System.out.println("Received an exception as expected.");
        }

        people = personService.get();
        Person person1Retrieved =
            findInList(people, person1.getFirstName(), person1.getLastName());
        Assert.assertNull("The first person created should have rolled back",
            person1Retrieved);
    }
}
```

The test creates two Person objects, puts them in a list and passes the list as the argument. Because we've written the test with the expectation that this is *transactional* (if any change causes an error, all changes will be rolled back), the test won't pass.

### Why The Test Fails

An error will occur when trying to add the second person because the first name is set to `null` and the column in the database is not nullable. The test uses the `try/catch` block to handle this error.

The test then asserts that the first person is not in the database (because of the error with the 2nd person, it should have been rolled back). This assert will fail because we're not using a transaction yet. This might seem a bit confusing but remember our process when writing unit tests:

1. Write the test

2. See the test fail *(We are here!)

3. Write the code to make it not fail

4. See the test pass

You can also verify this by opening up the PSQL terminal and querying the table manually.

## @Transactional to the Rescue!  

In the `PersonServiceImpl` we add an `@Transactional` annotation above each method that changes the database (i.e. inserts, updates, and deletes). For example:

```java
@Transactional
@Override
public void add(List<Person> people) {
    for (Person person : people) {
        personRepository.add(person);
    }
}
```

After adding the @Transactional annotations you should be able to re-run the test and see it pass. The error in the 2nd person is now causing the 1st person to be rolled back. Congratulations!
