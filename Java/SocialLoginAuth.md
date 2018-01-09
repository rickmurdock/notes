# Spring Social Login (OAuth2)

We see [**OAuth2**](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) in action anytime we use a third-party login (Facebook, Google, GitHub) to access another website. The result is that the user is authenticated, and the client receives a token to call back into the server. In this lesson we are only interested in the user being authenticated.

## Spring Boot and OAuth2

The **Spring Boot and OAuth2** tutorial was used as a basis for this lesson.

*Note* that Spring has introduced the [**Spring Social**](http://projects.spring.io/spring-social/) project to make the steps simpler. Spring Social is not used in this lesson.

## Setup

This code to follow along with on this lesson can be found on the `social-login` branch in the [**A&A Repository**](https://github.com/gpratte-tiy/a-and-a).

**Note this will not work with with version** `1.5.6.RELEASE` **of Spring Boot. Make sure the version for** `spring-boot-starter-parent` **in the pom.xml file is** `<version>1.5.4.RELEASE</version>`**. Remember to Reimport the pom.xml after making this change.**

I would also recommend running `mvn clean` from the command line at the root of the project to remove the target directory to get a clean build the next time it runs.

## Create Applications

The next steps will be creating applications on GitHub, Facebook and Google. Each will require a callback URL into your application. You will be creating apps for running in "dev mode" with a callback on `localhost:8080`. You'll have to create another with a different callback if/when you deploy to Heroku.

### GitHub app 

You must register an application with GitHub.

* Log into github.com

* Click on user icon and then settings

* Under the "Developer settings" on the left panel click on OAuth applications

* Click "Register a new app"

  * For Application name call it something like "Demo local"
  
  * For Homepage URL use "http://example.com"

  * For Application description use "Demo Local"

  * For Authorization callback URL use "http://localhost:8080"
  
After registering your app, you will see a "Client ID" and a "Client Secret" (they should be strings of hex code, like `68923a2be2faa12`). You will use your Client ID and Client Secret in the application properties file.

### Google app

Register an application with Google. The following is from [**Setting up API keys**](https://support.google.com/googleapi/answer/6158862). (If you don't already have a Google account, you should create one now)

* Go to the [**API Console**](https://console.developers.google.com/apis/dashboard).

* If the API Manager page isn't already open, open the left side menu and select API Manager

* On the left, choose Credentials

* Before you can create credentials you will need to create a project. You can accept the defaults.

* After creating a project, click Create credentials and then select "API key"

* Click on Credentials on the left pane

* You will have one entry under OAuth 2.0 client IDs, click on the name

* Set the Authorized JavaScript origins to `http://localhost:8080`

* Set the Authorized redirect URIs to `http://localhost:8080` and `http://localhost:8080/login/google`

* Save the changes


### Facebook app

* Log into https://developers.facebook.com/

* My Apps -> Add a new app

  * Give it a name
  
  * Click Create App ID
  
  * Click Dashboard in left pane to see App ID
  
  * Click Show to show App Secret
  
  * Click Add Product in left pane
  
  * Click Setup on Facebook Login
  
  * Choose Web (no need to give it a name)
  
  * Click on Facebook Login in the left panel
  
  * Set the Valid OAuth redirect URIs to http://localhost:8080/ and save changes

### Cookbook  

This lesson is a cookbook on how to setup social login in a Spring server.

The pom.xml has a new dependency:

```xml
<dependency>
    <groupId>org.springframework.security.oauth</groupId>
    <artifactId>spring-security-oauth2</artifactId>
</dependency>
```

The main class `AnaApplication` has a new annotation

* `@EnableOAuth2Sso`

Changed `application.properties` to `application.yml` which is the same thing but in markup form. Spring supports both.

There are entries for GitHub, Facebook and Google. The entries are boilerplate and require the Id and secret from the apps created above.

The `login.html` web page now has three links to login. For example the GitHub link is:

```html
<a href="/login/github">click here</a>
```

The biggest changes occurred in the `WebSecurityConfig` class.

We now have:

```java
@Autowired
OAuth2ClientContext oauth2ClientContext;
```

which is used in the `ssoFilter()` method we will see later.

The biggest change to the `configure()` method is to add the sso filter:

```java
.and().addFilterBefore(ssoFilter(), BasicAuthenticationFilter.class);
```

`The ssoFilter()` filter method is mostly boilerplate with the only difference being the URL (e.g. `/login/facebook`).

Then you see three pair of methods (e.g. `facebook()` and `facebookResource()`) that pull the settings from the `application.yml` file.

## dev mode Restriction  

When running in dev mode you can only log in with the user that created the apps (see "GitHub app", "Facebook app" and "Google app" sections above).

## Confirming User Authentication

You will probably need to confirm that the user has been authenticated (whether social login or username/password). See how to get the authenticated in the `showPrinciple` method. Here is a snippet:

```java
Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
System.out.println("name " + authentication.getName());
```

Obviously the name is unique. This should be enforced in the login path that authenticates against the database by making the name a unique column.
