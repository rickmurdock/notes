# Spring's RestTemplate

In this lesson, we will be using Spring's "RestTemplate" to consume a JSON REST API.

## What is a REST API?  

REST stands for REpresentational State Transfer. Wikipedia provides the following (not very useful) definition: "Representational state transfer (REST) or RESTful Web services are one way of providing interoperability between computer systems on the Internet. REST-compliant Web services allow requesting systems to access and manipulate textual representations of Web resources using a uniform and predefined set of stateless operations."

In practice, REST services are a way for us to receive or provide data (in our case, JSON). A REST API is defined through **endpoints**, just like our Controller from before. But instead of endpoints returning HTML views (through Spring), REST endpoints will return JSON data. Later in the lesson we'll cover how to create your own REST API. For now, we'll discuss using someone else's service.

## Non-Programmatic API Access  

We'll be using the API at http://gturnquist-quoters.cfapps.io/api/random since it is relatively simple. It returns a random testimonial-type quote about how great Spring Boot is. Access the endpoint through a browser and you should see something like this:

```json
// 20170704160741
// https://gturnquist-quoters.cfapps.io/api/random

{
  "type": "success",
  "value": {
    "id": 4,
    "quote": "Previous to Spring Boot, I remember XML hell,
     confusing set up, and many hours of frustration."
  }
}
```

That's great, but we want to be able to access REST APIs from our programs.

## Spring Setup  

Start a new Spring Boot project - you only need the Web dependency for now. You'll also need to add

```xml
<dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
</dependency>
```

to your Maven POM.

## Entity Setup  

We'll need to create classes to hold the data we're getting from the API. We can see that the response has "type" and "value" properties, with value being an object that contains an "id" and a "quote". We'll create a class to represent the response:

```java
import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties(ignoreUnknown = true)
public class RandomQuoteResponse {

    private String type;
    private Value value;

    public RandomQuoteResponse() {
    }

    public String getType() {
        return type;
    }

    public void setType(String type) {
        this.type = type;
    }

    public Value getValue() {
        return value;
    }

    public void setValue(Value value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return "Quote{" +
                "type='" + type + '\'' +
                ", value=" + value +
                '}';
    }
}
```

This is a simple Java Bean/POJO. We use the @JsonIgnoreProperties annotation to indicate that any "extra" properties we receive back should be ignored. Note that the variable names and types must match with the JSON we're receiving from the API. Next, add the Value class:

```java
import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties(ignoreUnknown = true)
public class Value {

    private Long id;
    private String quote;

    public Value() {
    }

    public Long getId() {
        return this.id;
    }

    public String getQuote() {
        return this.quote;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public void setQuote(String quote) {
        this.quote = quote;
    }

    @Override
    public String toString() {
        return "Value{" +
                "id=" + id +
                ", quote='" + quote + '\'' +
                '}';
    }
}
```

## Changing Our Application

Lastly, we will make changes to our main Application class.

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
public class Application {

    static final String API_URL = "http://gturnquist-quoters.cfapps.io/api/random";

    public static void main(String args[]) {
        SpringApplication.run(Application.class);
    }

    @Bean
    public String run () {
        RestTemplate restTemplate = new RestTemplate();
        RandomQuoteResponse quote =
            restTemplate.getForObject(API_URL, RandomQuoteResponse.class);
        System.out.println("\n******\n");
        System.out.println(quote);
        System.out.println("\n******\n");
        return "done";
    }
}
```

Spring is once again doing a lot of work in the background for us. As evidence, you should see that the `restTemplate()` and `run()` functions appear grey in IntelliJ - the IDE thinks these functions are never called. Run the program and see the output to show IntelliJ just how wrong it is. You should see a random Spring testimonial quote in between some information printed out by Spring.

What have we accomplished?

* Made a call to someone else's API

* Received JSON data back

* Deserialized the data into usable Java Objects

At this point, we can do whatever we need to do with these objects/data; we could store them in a database, use it in our program, or write it to a text file.

---

# REST APIs

Now that we know how to create web applications that render HTML we need to know how to support other types of applications that may want to use our data. REST APIs will allow us to do that.

REST stands for REpresentational State Transfer. (Reminder: API - Application Programming Interface). A REST API is an API that is accessible over the internet, which is used to transfer data from a server to a client and vice versa. The REST API can serve as an interface between the front and back end of an application. Our Java REST APIs will be using JSON.

## Our First REST API  

Our Spring REST API will make use of two annotations: the `@RequestMapping` (also used for non-REST/JSON endpoints) and the `@RestController` annotation which identifies the class that will be our REST controller. Let's look at an example:

```java
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;
import java.util.List;

@RestController //<- The @RestController annotation is crucial!
//Translation: "Spring: here be my JSON endpoints, yarr."
public class ExampleRestController {


    List<String> players = new ArrayList<>();
    List<Planet> planets = new ArrayList<>();
    List<Spaceship> ships = new ArrayList<>();

    //Here is our first endpoint
    @RequestMapping (path = "/api/players", method = RequestMethod.GET)
    public List<String> getPlayerNames () {
        return players;
    }

    @RequestMapping (path = "/api/planets", method = RequestMethod.GET)
    public List<Planet> getPlanets () {
        return planets;
    }

    @RequestMapping (path = "/api/ships", method = RequestMethod.GET)
    public List<Spaceship> getShips () {
        return ships;
    }

    @RequestMapping (path = "/api/planets/high-grav", method = RequestMethod.GET)
    public List<Planet> getHighGravPlanets () {
        List<Planet> highGravPlanets = new ArrayList<>();
        for (Planet planet : planets) {
            if (planet.getGravity() > 1.0) {
                highGravPlanets.add(planet);
            }
        }
        return highGravPlanets;

        /*Or functionally!:
        return planets.stream()
            .filter(e -> e.getGravity() > 1.0)
            .collect(Collectors.toList());
        */
    }
}
```

The code is very simple, but there's a lot going on here. Let's discuss the work Spring is doing for us in the background, starting with the first endpoint. We've asked Spring to map the path `/api/players` to our `getPlayerNames()` function, so when anyone running our web app makes a "GET" request to that endpoint, this method will be called.

Once the function is called, it will be executed as normal. Keep in mind that these are still functions, and they can still do all the things regular functions can do (call other functions, print to the console, read from files, interact with a database, etc). In this case we are simply returning a List of names. But there's an additional step that Spring performs for us; it serializes the data into JSON - that's what our endpoints are supposed to be providing.

* Function is mapped to the endpoint by Spring (@RequestMapping)

* Function is called and executes as normal

* Spring JSON serializes the return of the function

The last endpoint provides an example of an endpoint function that is a little bit more complex. For this endpoint, we want to provide only the "high gravity" planets (planets whose surface gravity is greater than Earth's 1.0). Our function searches through the existing list of planets and pulls out the ones we want. This list is then serialized and sent on its way.

## Function of a REST API  

REST APIs form a critical part of our Java back-end framework. They allow us to translate our "Java data" which is in the form of objects, and interfaces, and primitives (oh my!) into JSON (or other standard data formats) that can be accessed out on the web. This data can come from anywhere - local memory, file I/O, random generation - but most likely will come from querying a database.
