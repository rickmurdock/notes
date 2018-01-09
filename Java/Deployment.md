# Deployment With Heroku

Up to this point, we've only really run our apps in two ways:

* In the command prompt

* Inside of IntelliJ

Running inside of IntelliJ has given us the flexibility to run a lot of different types of applications, including command line apps and Spring apps.

This is still limiting in two main ways though:

* Not everyone who wants to run our programs will have IntelliJ

* Running a web-based app on our own laptops isn't practical for making the app available on the internet to other users

Introducing...

## Heroku

Heroku is a hosting platform that focuses on making it easy to spool up instances of an application. For Java applications, Heroku knows how to handle Maven build scripts and can actually let you deploy an application online straight from a pom.xml build file.

We couldn't possibly put together a better "getting started" guide that what Heroku themselves have done, so let's follow Heroku's guide to deploy one of their example projects.

Note: this project is a Spark-based project, which is a framework we have not covered, but the basic principles we have covered are the same and the code changes they ask us to make are spelled out in detail.

Follow these instructions to deploy your first app on the public web: https://devcenter.heroku.com/articles/getting-started-with-java#introduction

# Deploy An App

Now let's take one of our previous Spring apps and deploy it to Heroku.

* Go to Heroku dashboard -> start a new app

* Add Heroku remote for your app

```sh
heroku git:remote -a <your app>
```

* Check with `git remote -v`

* Push to heroku:

```sh
git push heroku master
```

Additional Reading: https://devcenter.heroku.com/articles/deploying-spring-boot-apps-to-heroku

## Working With Heroku  

Once the deployment messages stop you will want to watch your application start up. You can "tail" the logs with the following command:

```sh
heroku logs --tail
```

To run a bash terminal on the remote system

```sh
heroku run bash
```

**Note** that all files are read-only, so you cannot change them. Even if you could, they would get overwritten next time you deployed.

Note: on free Heroku instances, they will "go to sleep" after a relatively short period of inactivity (30-60 minutes). When you first try to access your sleeping instance, it will take a while to "wake up", around a minute. This is normal, but you will need to **account for it when you are using free Heroku instances for presentations**. Free Heroku instances are also limited on processing power and will tend to run more slowly.

There are other commands. Other than deploying (via git) all the commands mimic what you can do from the Heroku website.

## Procfile 

A `Procfile` is what Heroku uses to start up the application. If you do not provide one Heroku will create one for you each time you deploy.

You can see the Procfile that Heroku generates

1. Log into the Heroku website

2. Navigate to your application

3. Click on the Resources tab

4. You will see something like `web java -Dserver.port=$PORT $JAVA_OPTS -jar target/abc-0.0.1-SNAPSHOT.jar`

A "Procfile" belongs in the project root. For a Spring Boot project it should look like

```sh
web: java -Dserver.port=$PORT $JAVA_OPTS -jar target/<name of your jar>
```

where "" is the JAR file in the target directory. And yes, the `:` after the word `web` is required.

Use the maven command to find the name of a Spring Boot JAR. In a terminal at the root of the project type

* `mvn clean`

* `mvn package`

Now the JAR is in the `target` directory.

## Profiles 

You can define different profiles for different environments. The first step is to have a different `application-<profile name>.properties` for each profile.

Assuming a project will be run locally (which we call the `dev` environment) and on Heroku (`prod` environment) then the following property files are required

* `application-dev.properties`

* `application-prod.properties`

Provide the following property when running locally:

```sh
-Dspring.profiles.active=dev
```

There are three ways to run a Spring Boot project locally.

The first way is using maven from the command line

* `mvn -Dspring.profiles.active=dev spring-boot:run`

The second way, when running in IntelliJ, is to set the property by doing the following

* Click the Run file menu and then click Run...

* Click Edit Configurations... in the pop up window

* Make sure the class that has the main method is highlighted in the left panel

* Add the property as a VM option.

* Click Apply and then click Run

The third is to run the JAR from the command line by typing `java -jar -Dspring.profiles.active=dev <jar name>.jar`

## Procfile again

To set the profile property when running the app on Heroku add the flag to the Procfile. For example

```sh
web: java -Dspring.profiles.active=prod -Dserver.port=$PORT $JAVA_OPTS -jar target/<name of your jar>
```

and then

```sh
git add .
git commit -m "set profile in procfile"
git push heroku master
```

## Social Login

If you are using social login then the app you are configured to use is the same for the `dev` and `prod` application properties. You need to create another app for each social login and set the redirect URL to your Heroku URL (instead of `localhost:8080`).
