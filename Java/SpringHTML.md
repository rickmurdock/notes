# Annotations

Annotations are short statements that provide additional information about our code, either to the compiler or a framework. An example of an annotation is the `@Override` notation.

```java
public class Animal {
  String name;
  String type;

  @Override //<- The annotation
  public String toString () {
    return "I'm " + name + " the " + type + ".";
  }
}
```

Annotations begin with the `@` (at sign) and end with the annotation text (`Override`), but can also include parenthesis, as we'll see later. Annotations refer to whatever code immediately follows the annotation. In this case, the `@Override` annotation refers to the `toString()` method. Annotations can refer to variables and classes as well.

But what do annotations *do*? The code above will run the same with or without that annotation. In this case, the annotation passes information to the compiler: that we intend to override a method from our superclass (in this case Object). In this case, the annotation serves as a form of documentation. It explicitly reminds the reader that we didn't declare this method; we are, in fact, overriding a method.

Another nice feature of annotations is that they will cause the compiler to throw an error if they aren't true. For instance, if we provide an `@Override` annotation for a method *that doesn't override a superclass method*, the compiler will throw and error.

For example:

```java
public class Animal {
    String name;
    String type;

    @Override //<- The annotation
    public String toString (int x) {
        return "";
    }
}
```

If your code has deeply nested classes or a complex structure, annotations can provide some assurance that you are managing class methods appropriately.

## Metadata  

Annotations are a form of "metadata." "Meta" is a prefix from Greek that means "about." So metadata is "data about data." In other words, annotations are extra information about our code.

## @RequestMapping  

In addition to acting as documentation and metadata, annotations provide information to any frameworks included in a project.

For example, Spring MVC uses the `@RequestMapping` annotation. This annotation tells the framework that when a user of the app goes to a particular URL "route" or path, we want Spring to call this method. For example:

```java
@RequestMapping(path = "/login", method = RequestMethod.POST)
public String login (HttpSession session, String email, String password) {
    //Method body
}
```

In this case, the annotation uses **elements**. Elements are similar to arguments in functions, but their syntax is different. The syntax is:

`<element_name> = <element_value>, <element2_name> = <element2_value> ...`

Let's look at the two arguments used in this case:

* `path = "/login"` - This element describes the path mapped to the method. Whenever someone reaches `/login`, Spring will call `login()`.

* `method = RequestMethod.POST` - This element describes the type of HTTP request for the path. In this case, the annotation specifies the "Post" method.

# @Autowired  

Spring also uses the `@Autowired` annotation. Every Spring application contains *many* moving parts including Controllers, Repositories, and other resources.

Imagine you're building a robot. Each of these moving parts is a component on your robot; you have motors, sensors, cameras, pneumatics, etc. Each component must be connected to other components to work. Motors need to be plugged into a source of power. Sensors will need to be *wired* to your robot's processor.

In Spring, the `@Autowired` annotation essentially says, "Spring, I want you to connect this component up for me."

```java
public interface UserRepo extends CrudRepository<User, Integer> {
    User findFirstByEmail (String email);
    List<User> findByLobbyStatus (LobbyStatus status);
}
//In a different file:
    @Autowired
    UserRepo users;
```

Here we are using the `org.springframework.data.repository.CrudRepository`. UserRepo is a repository which holds "User" objects. In a different file somewhere (a controller) we are declaring the `@Autowired UserRepo users`. Note that we never declare `UserRepo users = new UserRepo()`. Spring takes care of setting up the repository for us.

---

# Basic Spring @Controller

Static websites are nothing more than a collection of *unchanging* HTML files. These files have all of the content built into the markup. Users who visit the site can browse through the different files using links and anchors.

In contrast, web *apps* are comprised of HTML **templates** and **controllers** that inject content into those templates. The architecture of dynamic sites is more complicated than static sites, but also far more powerful.

The function of the "Controller" is to determine what content gets injected into the template and ultimately serving up as HTML to the user.

To map controllers to templates, Spring uses two annotations: `@Controller` and `@RequestMapping`.

@Controller - Defines the class as a Controller.

@RequestMapping - Maps a URL route to a method

Let's say our web app is a simple game. We're not going to worry about creating any logic for the game; right now we just want to provide endpoints to demonstrate how Controllers work.

This game will have three HTML views:

* A login page

* A page for playing the game

* A page for the game settings.

Each view will have a corresponding HTML template: `login.html`, `game.html`, and `settings.html` for these views.

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class GameController {

    @RequestMapping(path = "/login", method = RequestMethod.GET)
    public String login () {
        return "login";
    }

    @RequestMapping(path = "/game", method = RequestMethod.GET)
    public String game () {
        return "game";
    }

    @RequestMapping(path = "/settings", method = RequestMethod.GET)
    public String settings () {
        return "settings";
    }
}
```

What does this code do? Within our web app, whenever a user attempts to access the paths defined above (either by clicking a link or perhaps by manually typing in the URL), Spring will look for an HTML file associated with the String we are returning; for example, `game.html`.

Why is this better than simply making those HTML files available directly? Again, control. Let's define a simple static password for our app that will be shared by all users (not a good practice) and modify our "login" and "game" endpoints to reflect this.

A user should only be able to log in if they know the password, and a user can only play the game if they are logged in. If the user's password is incorrect, we will redirect them to a page ("bad-pw.html") that explains this.

```java
@RequestMapping(path = "/login", method = RequestMethod.POST)
public String login2 (HttpSession session, String username, String password) {
    if (password.equals(CORRECT_PASSWORD)) {
        session.setAttribute("username", username);
        return "redirect:/game.html";
    } else {
        return "bad-pw";
    }
}

@RequestMapping(path = "/game", method = RequestMethod.GET)
public String game2 (HttpSession session) {
    if (session.getAttribute("username") != null) {
        return "game";
    } else {
        return "redirect:/login.html";
    }
}
```

Notes:

* The login end-point has been changed to a POST since we need to be able to receive information from a form (the username and password).

* We are using the `HttpSession` class here.

  * When a user first connects to our app, they start a new "session." This class allows us to keep track of various information about the user while they are active on the site.

  * In our app, this might include their current score or other information about the state of the game.

  * Storing data in the session: we use the `setAttribute(String, Object`) method, which creates a key-value pairing between the given String/Object pairing. You can think of the session as a `Map<String, Object>`.

  * Retrieving data from the session: use the `getAttribute(String`) method. Like `Map`, the method returns `null` if there is no association.
  
* `return "redirect:/game.html"`: Spring recognizes this as a different type of instruction. Instead of looking for an HTML file, it will redirect the user to the given route. In this case, since the user successfully logged in, we went ahead and directed them to the main game page.
