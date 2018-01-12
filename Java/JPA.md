# JPA

JPA (Java Persistence Api) is an Object-Relational Mapping (ORM) for Java. It is a common interface that abstracts the actual ORM (e.g. hibernate) that "does the real work".

## Following Along  

In this lesson, we will be working with the same code we started in the JDBC lessons. You can find starting code for this lesson on the `jdbc-final` branch at [v2_aggregate](https://github.com/gpratte-tiy/v2_aggregate). If you make all the changes made in this lesson, you should end up with something that looks a lot like the `jpa-final` branch of that same repository. We're going to gradually introduce JPA by converting over our existing JDBC project.

## ORMs and Relationships  

Set the way-back machine to four weeks ago during Back-End Fundamentals. In this lecture we're going to put the 'R' back in RDBs (Relational DataBases) in Java. We'll be discussing **one-to-one** relationships and **one-to-many** relationships. Refer back to your BEF lectures and/or discuss with classmates if you need a refresher on those concepts.

## Expand the persondb  

Add an address and an email table. `Person` will have a one-to-one relationship with `address` and a one-to-many relationship with `email`.

Log into postgres:

`psql -U personuser persondb`

Create an address table:

`create table address (id serial not null, street text, city text not null, state text not null, zip text, person_id integer not null, constraint address_pkey primary key (id));`

Add a foreign key constraint:

`ALTER TABLE address ADD CONSTRAINT address_person_fkey FOREIGN KEY (person_id) REFERENCES person (id);`

Create an email table:

`create table email (id serial not null, email text, person_id integer not null, constraint email_pkey primary key (id));`

Add a foreign key constraint: 

`ALTER TABLE email ADD CONSTRAINT email_person_fkey FOREIGN KEY (person_id) REFERENCES person (id);`

Log out of the psql command line: `\q`

## Exercise the Endpoints  

Begin by removing all rows from the person, address and email tables.

In the following examples replace the value `71` with the id of the person created. Replace the address value `5` with the address created for the address endpoints. Replace the address value `5` with one of the emails created for the email endpoints.

*Note:* The `| python -m json.tool` formats the json returned. If you don't have this on your system then it can be removed.

Create a person

* `curl -v -H "Content-Type: application/json" -X POST -d '{"firstName":"Mal","lastName":"Reynolds"}' http://localhost:8080/api/person`

Get all people

* `curl -v http://localhost:8080/api/persons | python -m json.tool`

Update person

* `curl -v -H "Content-Type: application/json" -X PUT -d '{"firstName":"Malcolm","lastName":"Reynolds"}' http://localhost:8080/api/person/71`

Get person

* `curl -v http://localhost:8080/api/person/71 | python -m json.tool`

Add address

* `curl -v -H "Content-Type: application/json" -X POST -d '{"street":"Serenity","city":"Book Village","state":"Haven","zip":"HAV3456"}' http://localhost:8080/api/person/71/address`

Get person

* `curl -v http://localhost:8080/api/person/71 | python -m json.tool`

Delete address

* `curl -v -H "Content-Type: application/json" -X DELETE http://localhost:8080/api/person/71/address/5`

Get person

* `curl -v http://localhost:8080/api/person/71 | python -m json.tool`

Add email

* `curl -v -H "Content-Type: application/json" -X POST -d '{"email":"mal@reynolds.uni"}' http://localhost:8080/api/person/71/email`

Get person

* `curl -v http://localhost:8080/api/person/71 | python -m json.tool`

Add email

* `curl -v -H "Content-Type: application/json" -X POST -d '{"email":"captain@reynolds.uni"}' http://localhost:8080/api/person/71/email`

Get person

* `curl -v http://localhost:8080/api/person/71 | python -m json.tool`

Delete email

* `curl -v -H "Content-Type: application/json" -X DELETE http://localhost:8080/api/person/71/email/5`

Get person

* `curl -v http://localhost:8080/api/person/71 | python -m json.tool`

## Review the JDBC code  

### com.example.aggregate.domain  

Person

* has a reference to a single `Address address`

* has a reference to a `List<Email>` emails with getter

Address and Email

* have the personId (not a Person reference) which is `person_id` foreign key to person in the database tables

* `equals()` and `hashCode()` exclude id

### com.example.aggregate.respository  

PersonRepositoryImpl

* insert does not return primary key

AddressRepositoryImpl

* insert does not return primary key

* findByPersonId returns a single object (1-to-1)

EmailRepositoryImpl

* insert does not return primary key

* findByPersonId returns a list (1-to-many)

### com.example.aggregate.service  

PersonServiceImpl

* private getPerson assembles the person

### com.example.aggregate.controller  

* Since the endpoints are RESTful they are pretty much self explanatory.

* Notice that the RESTful verbs are used (i.e. POST, PUT, GET, DELETE)

### Testing  

There is a PersonRepositoryTest (more about that later).

PersonServiceTest:

* For thoroughness, we could add more tests, but it's a good start for now

* Go ahead and run it

* Notice the @Transactional annotation which means there are no rows inserted in the databases when the test completes (pretty cool).

## Moving to JPA  

Let's start switching over to the wonderful world of JPA!

### pom.xml  

The first step was to update our `pom.xml`, replacing spring-boot-starter-jdbc with:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

If you are playing along make sure IntelliJ updates the libraries. In the project window, right click on pom.xml -> Maven -> Reimport. As the Java files are changed make sure to choose JPA imports and not hibernate ones when choosing from a list of classes to import.

## Changing Entities  

**Annotations**: You will be seeing more and more annotations as we move forward with Spring. Remember, Spring can automate a lot of background work for us. It's essentially building connections between different parts of your app in XML documents. These annotations tell it which parts need to be "wired up". **If you aren't solid on your understanding of annotations, ask your instructor for a review.**

For JPA to work the domain classes get annotated with the information:

* `@Entity` informs JPA that the class models a database table

* `@Table` the name of the table

* `@Id` the primary key

* `@GeneratedValue` how the key is generated. Since postgres is generating the key the `strategy` will be `GenerationType.IDENTITY`

For class fields (for example, the `id` field of `Person`), you can add annotations either above the field itself:

```java
public class Person {

    @Id
    @GeneratedValue(strategy= GenerationType.IDENTITY)
    private int id;
    //Other fields...
}
```

You can also put the annotations over the getter methods, which is the pattern we will be using. JPA will use reflection to match Java variables to columns.

### Updating Person  

Add the @Entity and @Table annotations to Person:

```java
@Entity
@Table( name = "person" )
public class Person {
```

Annotate the getId method:

```java
@Id
@GeneratedValue(strategy=GenerationType.IDENTITY)
public int getId() {
   //...
```

JPA will use reflection to match up Java properties (e.g. String firstName) with columns in the corresponding table. But it only works if the column names are "lowersnakecase" (e.g. first_name). If you name a column something other than that then `@Column` annotation can come to the rescue. As you see in the `Person` bean both `firstName` and `lastName` require this annotation:

```java
@Column(name = "firstname")  
public String getFirstName() {
```

Next, change `equals()` and `hashCode()` to only use the id field. The reason for this can be found in section [2.5.7. Implementing equals() and hashCode()](http://docs.jboss.org/hibernate/orm/5.2/userguide/html_single/Hibernate_User_Guide.html) of the hibernate user guide which states
"There is really just one absolute case: a class that acts as an identifier must implement equals/hashCode based on the id value(s)."

For relationships to work in JPA both sides of the relationship must be annotated.

### Update Address and Email  

Annotate address with a one-to-one relationship:

```java
@OneToOne(mappedBy = "person")
public Address getAddress() {
```

Annotate emails:

```java
@OneToMany(mappedBy = "person", fetch=FetchType.LAZY)
public List<Email> getEmails() {
```

### Why Do These Relationships Make Sense?  

**Address**: In the context of our program, we're saying that each Person should have one and only one address (no summer homes, sorry). While in real life people can have multiple addresses, PO boxes, etc. for the purposes of our app, we only want their primary address. So this is a **one-to-one** relationship. (1 Person has 1 and only 1 address)

**Email**: Again, in the context of our program, a Person can have multiple e-mails. Maybe they want several friends to receive the newsletter, or have a work e-mail and a personal e-mail. In any case, because our app allows each Person to have as many e-mail addresses as they want, from `Person's` perspective this is a **one-to-many** relationship. (1 Person can have many emails). From Email's perspective this is a **many-to-one** relationship (Many e-mail addresses can all belong to one person).

### FetchType  

The "FetchType" is the strategy that the ORM will use when it goes to "fetch" information from the database. It allows us to determine how much data we're getting back based on our needs. For example, the `fetch=FetchType.LAZY` property means that when a person is retrieved from the database DO NOT get the emails. Why?

**Thought Experiment**: Imagine a much larger database with tens of thousands or even millions of records, and a lot of different relationships. Someone's built a front-end for this database that will display a list of Person records. A user accesses that page. The front-end makes a call to the back-end (that's us) and wants all the names so they can display them.

If our strategy when fetching from the database is to get ALL the information associated with each person, and all their relevant connections (their favorite type of icecream, when they created their account, who their favorite sports teams are, and list of other users that also like those sports teams, the names of their pets, etc. etc.), that's going to take a while, and we'll keep the front-end waiting for a long time. The front-end is impatiently tapping its foot and saying "Gosh, all I wanted was a list of names."

On the other hand, when we're accessing the individual `Person` view in our front-end, we DO want all the various details.

OK, thought experiment over - back to our project. In short, when we get a list of people that should be fast (not slowed down by getting a bunch of extra data). We'll see later that we will get the list of emails when getting a single person.

#### com.example.aggregate.domain.address  

Next, we'll apply most of the same steps from changing Person above to Address.

Annotate the class:

```java
@Entity
@Table(name="address")
public class Address {
```

Remember, JPA is an ORM (**Object**-Relational Mapping). This means that in the world of Java, we are back on "familiar ground" of Objects instead of foreign keys. Instead of Address having a `personId` variable, it just has a Person. Which is how we'd have set it up in the first place, if we weren't concerned about storing in a database.

Changes:

* Remove the `personId` variable, getter and setter.

* Remove all getters, setters, `equals()`, `hashCode()` and `toString()`

* Added a `Person person` variable

* Generate getters and setters

* Generate `equals()` and `hashCode()` that only use the `id` variable

* Generate `toString()`

The problem with address.toString() as it stands is that it will create a circular reference, calling person.toString(), which in turn will call the address.toString(). Fix this by changing the person part of toString() from:

```java
", person=" + person +
```

to:

```java
", person=" + (person == null ? "" : person.getId()) +
```

Annotate the id and the one-to-one relationship with person:

```java
@Id
@GeneratedValue(strategy=GenerationType.IDENTITY)
public int getId() {}

@JsonIgnore
@OneToOne
@JoinColumn(name = "person_id")
public Person getPerson() {}
```

Notes:

* The `name = "person_id"` is the foreign key in the address table.

* The `@JsonIgnore` annotation is needed to avoid the circular problem of person having and address and an address having a person. Without the `@JsonIgnore` the JSON marshaller would blow out the stack person trying to marshall address and address trying to marshall person.

## com.example.domain.Email  

Do the all same steps from Address but use a many-to-one relationship. For brevity will only show the one difference, which is the relationship.

Annotate the many-to-one relationship with person:

@JsonIgnore
@ManyToOne()
@JoinColumn(name = "person_id")
public Person getPerson() {

## com.example.aggregate.repository  

We can get rid of all the classes that implement the interface (i.e. PersonRepositoryImpl, AddressRepositoryImpl and EmailRepositoryImpl).

From the [Accessing Data with JPA](https://spring.io/guides/gs/accessing-data-jpa/) guide:

"In a typical Java application, you’d expect to write a class that implements [the repository interface]. But that’s what makes Spring Data JPA so powerful: You don’t have to write an implementation of the repository interface. Spring Data JPA creates an implementation on the fly when you run the application."

This is the reason that you will see later the PersonRepositoryTest is deleted!

Changes:

* Remove all the methods from each repository interface. Yes, it is true!

* Change the PersonRepository interface to extend

```java
extends JpaRepository<Person, Integer>
```

* Change the AddressRepository interface to extend

```java
extends JpaRepository<Address, Integer>
```

* Change the EmailRepository interface to extend

```java
extends JpaRepository<Email, Integer>
```

Note: you will often see `CrudRepository` used interchangeably with `JpaRepository`. You can read about the differences [here](https://stackoverflow.com/questions/14014086/what-is-difference-between-crudrepository-and-jparepository-interfaces-in-spring).

The first class between the angle brackets is the class that the repository will be working with. The second class is the the primary key.

## com.example.aggregate.service.PersonService  

Since JPA sets the `id` of the entity when it is created and returns the entity we can now pass back the Person which the caller will love to have (this could be useful for unit testing, or any number of other reasons). Change:

from `void add(Person person);`
to `Person add(Person person);`

## com.example.aggregate.service.PersonServiceImpl  

There are a lot of small changes in the service layer.

### Add person  

Changed

```java
public void add(Person person) {
    personRepository.add(person);
}
```
to

```java
public Person add(Person person) {
    return personRepository.save(person);
}
```

This method now returns a person and the call to `add()` changed to a call to `save()`. Save is the JPA method that is used to create or update. See [spring data](https://docs.spring.io/spring-data/data-commons/docs/current/api/org/springframework/data/repository/CrudRepository.html?is-external=true#method.detail)

### Get all people  

Just changed the call to the repository from

```java
return personRepository.get();
```

to

```java
return personRepository.findAll();
```

### Update person  

Change from

```java
personRepository.update(person);
```

to

```java
personRepository.save(person);
```

### Delete person  

Deleting a person is now a bit more complicated. Here is our JDBC method:

```java
@Transactional
@Override
public void delete(int id) {
    personRepository.delete(id);
}
```

We'll change this to:

1. Get the Person (which gets the address and emails if there are any).

2. Call corresponding repositories to delete address and emails

3. Delete the person.

```java
@Override
public void delete(int id) {
    Person person = this.getPerson(id);
    emailRepository.delete(person.getEmails());
    if (person.getAddress() != null) {
        addressRepository.delete(person.getAddress());
    }
    personRepository.delete(id);
}
```

Note that since JPA works with objects that most the methods (e.g. delete) take objects. Delete is one of the few methods that will also take an id.
See [Spring data api](https://docs.spring.io/spring-data/data-commons/docs/current/api/org/springframework/data/repository/CrudRepository.html?is-external=true#method.summary)

#### Cascades

Much like the FetchType allows us to determine how much data we get back from the database (i.e. do we just want the "core" Person fields or do we need ALL data associated with a Person), **cascades** allow us to determine how data is deleted. In a different application, we might not want a person's address to go away when they are deleted. (Maybe our program models behavior for a community and when one community member leaves, another person takes over their address). In this instance, we've decided that if we're deleting a Person, all of the associated data should be removed.

### Add address  

```java
@Override
public Person addAddress(Address address) {
    address = addressRepository.save(address);
    Person person = personRepository.findOne(address.getPerson().getId());
    person.setAddress(address);
    personRepository.save(person);

    return getPerson(address.getPerson().getId());
}
```

Add the address, get the person, set the address on the person and save the person.

### Delete address  

```java
@Override
public Person deleteAddress(int personId, int addressId) {
    addressRepository.delete(addressId);
    Person person = personRepository.findOne(personId);
    person.setAddress(null);
    personRepository.save(person);

    return getPerson(personId);
}
```

Delete the address, get the person, set address to null and save the person.

### Add email  

```java
@Override
public Person addEmail(Email email) {
    emailRepository.save(email);        
    Person person = personRepository.findOne(email.getPerson().getId());
    person.getEmails().add(email);
    personRepository.save(person);

    return getPerson(email.getPerson().getId());
}
```

Save the email, get the person, add the email to the person's emails, save the person.

### Delete email  

```java
@Override
public Person deleteEmail(int personId, int emailId) {
    Email email = emailRepository.findOne(emailId);
    emailRepository.delete(emailId);
    Person person = personRepository.findOne(personId);
    person.getEmails().remove(email);
    personRepository.save(person);

    return getPerson(personId);
}
```

Find the email, delete the email, find the person, remove the email from the person's list of emails, save the person.

### Getting Person with Emails  

Remember when the emails for the Person were annotated with the "lazy" FetchType? "Lazy" is a great word to describe it, since we effectively told JPA "Don't go get this until I actually need it". So now, we need to convince JPA that we need those e-mails. In the example below the `size()` method is called which makes JPA load the emails ("Oh - you actually want to know something about e-mails, huh? I better go get those for you.").

This *has* to be done in a transaction. Made sure all methods that change the database are marked as `@Transactional`. Annotate the read-only methods with `@Transactional(readOnly = true)`

```java
// Has to be done in a transaction (which it is)
private Person getPerson(int id) {
    Person person = personRepository.findOne(id);
    // For the lazy to load
    person.getEmails().size();
    return person;
}
```

## PersonRepositoryTest  

Deleted it. No more repository implementation classes to test :).

## PersonServiceTest  

Only a few changes are needed here since the PersonService interface barely changed. We're changing the test to use person instead of personId.

For example

```java
address.setPersonId(person1.getId());
```

changed to

```java
address.setPerson(person1);
```

### com.example.aggregate.controller.PersonController  

Just like the test there are not many changes because the service interface barely changed.

We have to lookup the Person instead of using the id. For example:

```java
address.setPersonId(id);
personService.addAddress(address);
```

to

```java
Person person = personService.getById(id);
address.setPerson(person);
personService.addAddress(address);
```

## Play along results  

The code for the conversion can be found on the `jpa-final` branch at [v2_aggregate](https://github.com/gpratte-tiy/v2_aggregate)

## References  

[The Java EE 6 Tutorial](https://docs.oracle.com/javaee/6/tutorial/doc/bnbpz.html)
[Hibernate User Guide](http://docs.jboss.org/hibernate/orm/5.2/userguide/html_single/Hibernate_User_Guide.html)

## Conclusion  

In moving from JDBC to JPA, we've effectively added another layer of abstraction. We've gotten away from writing SQL queries "by hand", and have also moved away from referencing foreign keys in our code. Some of the operations to interact with the database have gotten a bit more complex (deleting). Overall, however, we have a much more flexible and powerful way of communicating with the database that more naturally matches up with our Object Oriented Programming paradigm.
