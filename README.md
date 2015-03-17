#Spring Boot STS Tutorial

Spring Tool Suite 3.6.4 was just released last week. This blog post is a tutorial demonstrating some of the new features STS provides to create and work with Spring Boot applications.

In this tutorial you'll learn how to:

   - create a Simple Spring Boot Application with STS
   - use the new STS Properties editor to edit configuration properties.
   - use @ConfigurationProperties in your code to get the same editor support for your own configuration properties.
   - launch and debug your boot application from STS   

## Getting Started: Hello Boot

We use the "New Spring Starter" wizard to create a basic spring boot app. 

![menu-new-starter]

Spring boot provides so called 'starters'. A set of classpath dependencies and autoconfiguration that lets you get started without lots of spring configuration. We pick the 'web' starter as we'll build a simple 'Hello' rest service.

![new-starter-wizard]

The wizard is GUI frontend that, under the hood, uses the webservice at http://start.spring.io to generate some basic scaffolding. You could use the webservice directly yourself, download the zip, unpack it, import it etc. Using the STS wizard does all of this at the click of a button and ensures the project is configured correctly so you can immediately start coding. 

After you click the finish button, your workspace will look something like this:

![workspace-ready]

The Java main generated by start.spring.io is all the code in our app at the moment. Thanks to the 'magic' of spring boot, and because we added the 'web' starter to our dependencies, this is already a fully functional web server! It just doesn't have any real content yet. Before adding content, let's verify the app runs.

## Running a Boot App in STS

Spring boot apps created by the wizard come in two flavors 'jar' or 'war'. The wizard let's you choose between them in its 'packaging' option. A great feature of spring-boot is that you can easily create standalone jars that contain a fully functional embedded web server. All you need to do to run your app is run its Java Main type like you do any other plain Java application. This is huge advantage as you don't have to mess around with setting up local or remote servers, war packaging and deploying. If you want to do things 'the hard way' you can still choose 'war' packaging, or [convert your 'jar' app to a 'war'][convert-jar-to-war] app later.

Now, if you understood what I just said, then you probably realize you don't need any 'special' tooling to run the app. Just click on the Java Main type 

[menu-new-starter]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/menu-new-spring-starter.png
[new-starter-wizard]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/new-starter-wizard.png
[workspace-ready]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/workspace-ready.png
[convert-jar-to-war]:https://spring.io/guides/gs/convert-jar-to-war/

