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

The wizard is GUI frontend that, under the hood, uses the webservice at [start.spring.io] to generate some basic scaffolding. You could use the webservice directly yourself, download the zip it generates, unpack it, import it etc. Using the STS wizard does all of this at the click of a button and ensures the project is configured correctly so you can immediately start coding. 

After you click the finish button, your workspace will look something like this:

![workspace-ready]

The `HelloBootApplication` Java-main class generated by [start.spring.io] is the only code in our app at the moment. Thanks to the 'magic' of spring boot, and because we added the 'web' starter to our dependencies, this tiny piece of code is already a fully functional web server! It just doesn't have any real content yet. Before adding some content, let's learn how the to run the app, and verify it actually runs in the process.

## Running a Boot App in STS

Spring boot apps created by the wizard come in two flavors 'jar' or 'war'. The wizard let's you choose between them in its 'packaging' option. A great feature of spring-boot is that you can easily create standalone 'jar' packaged projecs that contain a fully functional embedded web server. All you need to do to run your app then, is run its Java Main type, just like you do any other plain Java application. This is a huge advantage as you don't have to mess around with setting up local or remote Tomcat servers, war-packaging and deploying. If you really want to do things 'the hard way' you can still choose 'war' packaging. However there's really no need to do so because: 

 1. you can [convert your 'jar' app to a 'war'][convert-jar-to-war] app at any time 
 2. the [Cloud Foundry] platform directly supports deploying standalone boot jars

Now, if you understood what I just said, then you probably realize you don't actually need any 'special' tooling from STS to run the app locally. Just click on the Java Main type and select "Run As >> Java Application" and voila. Also all of your standard Eclipse Java debugging tools will 'just work'. However, STS provides a dedicated launcher that does basically the same thing but adds a few useful bells and whistles. So let's use that instead.

![run-as-boot]

Your app should start and you should see some output in the console view:

![console-output]

You can open your app running locally at http://localhost:8080. All you'll get is a `404` error page, but that is exactly as expected since we haven't yet added any real content to our app.

Now what about the bells and whistles I promised? "Run As >> Boot App" is pretty much a plain Java launcher but provides some extra options to customize the launch configurations it creates. To see those options we need to open the "Launch Configuration Editor", accesible from the ![Debug] or ![Run] toolbar buttont

[menu-new-starter]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/menu-new-spring-starter.png
[new-starter-wizard]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/new-starter-wizard.png
[workspace-ready]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/workspace-ready.png
[run-as-boot]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/run-as-boot.png
[console-output]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/console-output.png
[Debug]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/debug-button.png
[Run]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/run-button.png

[Cloud Foundry]:http://cloudfoundry.org
[convert-jar-to-war]:https://spring.io/guides/gs/convert-jar-to-war/
[start.spring.io]:http://start.spring.io
