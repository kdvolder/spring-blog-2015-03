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
  
[menu-new-starter]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/menu-new-spring-starter.png
[new-starter-wizard]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/new-starter-wizard.png
[workspace-ready]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/workspace-ready.png

