#Spring Boot STS Tutorial

Spring Tool Suite 3.6.4 was just released last week. This blog post is a tutorial demonstrating some of the new features STS provides to create and work with Spring Boot applications.

In this tutorial you'll learn how to:

   - create a Simple Spring Boot Application with STS
   - launch and debug your boot application from STS
   - use the new STS Properties editor to edit configuration properties.
   - use @ConfigurationProperties in your code to get the same editor support for your own configuration properties.
   
## Creating a Boot App

We use the "New Spring Starter" wizard to create a basic spring boot app. 

![menu-new-starter]

Spring boot provides so called 'starters'. A starter is set of classpath dependencies, which, together with Spring Boot autoconfiguration and lets you get started with an app without needing to do lots of spring configuration. We pick the 'web' starter as we'll build a simple 'Hello' rest service.

![new-starter-wizard]

The wizard is GUI frontend that, under the hood, uses the webservice at [start.spring.io] to generate some basic scaffolding. You could use the webservice directly yourself, download the zip it generates, unpack it, import it etc. Using the STS wizard does all of this at the click of a button and ensures the project is configured correctly so you can immediately start coding. 

After you click the finish button, your workspace will look something like this:

![workspace-ready]

The `HelloBootApplication` Java-main class generated by [start.spring.io] is the only code in our app at the moment. Thanks to the 'magic' of spring boot, and because we added the 'web' starter to our dependencies, this tiny piece of code is already a fully functional web server! It just doesn't have any real content yet. Before adding some content, let's learn how to run the app, and verify it *actually* runs in the process.

## Running a Boot App in STS

Spring boot apps created by the wizard come in two flavors 'jar' or 'war'. The Starter wizard let's you choose between them in its 'packaging' option. A great feature of spring-boot is that you can easily create standalone 'jar' packaged projects that contain a fully functional embedded web server. All you need to do to run your app, is run its Java Main type, just like you do any other plain Java application. This is a huge advantage as you don't have to mess around with setting up local or remote Tomcat servers, war-packaging and deploying. If you really want to do things 'the hard way' you can still choose 'war' packaging. However there's really no need to do so because:

 1. you can [convert your 'jar' app to a 'war'][convert-jar-to-war] app at any time 
 2. the [Cloud Foundry] platform directly supports deploying standalone boot jars

Now, if you understood what I just said, then you probably realize you don't actually need any 'special' tooling from STS to run the app locally. Just click on the Java Main type and select "Run As >> Java Application" and voila. Also all of your standard Eclipse Java debugging tools will 'just work'. However, STS provides a dedicated launcher that does basically the same thing but adds a few useful bells and whistles. So let's use that instead.

![run-as-boot]

Your app should start and you should see some output in the console view:

![console-output]

You can open your app running locally at [http://localhost:8080][http://localhost:8080]. All you'll get is a `404` error page, but that is exactly as expected since we haven't yet added any real content to our app.

Now, what about the bells and whistles I promised? "Run As >> Boot App" is pretty much a plain Java launcher but provides some extra options to customize the launch configurations it creates. To see those options we need to open the "Launch Configuration Editor", accesible from the ![Debug] or ![Run] toolbar button:

![run-conf-menu]

If you've used the Java Launch Config Editor in Eclipse, this should look familiar. For a Boot Launch Configuration, the 'Main' tab is a little different and has some extra stuff. I won't discuss all of the extras, you can find out more in the [STS 3.6.4 release notes]. So let's just do something simple, for example, override the default http port '8080' to something else, like '8888'. You can probably guess that this can be done by setting a system property. In the 'pure' Java launcher you can set such properties via commandline arguments. But what, you might wonder, is the name of that property exactly "spring.port", "http.port", "spring.server.port"? Fortunately, the launch config editor helps. The "Override Properties" component provides some basic content assist. You just type 'port' and it makes a few suggestions:

![override-property]

Select `server.port` add the value '8888' in the right column and click "Run". 

If you followed the steps exactly upto this point, your launch probably terminates immediately with an exception:

    Error: Exception thrown by the agent : java.rmi.server.ExportException: Port already in use: 31196; nested exception is: 
	    java.net.BindException: Address already in use
	   
This may be a bit of a surprise, since we just changed our port didn't we? Actually the port conflict here is not from the http port but a JMX port used to enable "Live Bean Graph Support" (I won't discuss this feature in this Blog post, see [STS 3.6.4 release notes]).

There are a few things we could do to avoid the error. We could open the editor again and change the JMX port as well, or we could disable 'Live Bean Support'. But probably we don't *really* want to run more than one copy of our app in this scenario. So we should just stop the already running instance before launching a new one. As this is such a common thing to do, STS provides a ![Relaunch] Toolbar Button for just this purpose. Click the Button, the running app is stopped and restarted with the changes you just made to the Launch Config now taking effect. If it worked you should now have a `404` error page at `http://localhost:8888` instead of 8080. (Note: the Relaunch button won't work if you haven't launched anything yet because it works from your current session's launch history. However if you've launched an app at least once, it is okay to 'Relaunch' an app that is already terminated)

##Editing Configuration Poperties in 'application.properties'

Overriding default property values from the Launch Config editor is convenient for a 'quick override', but it probably isn't a great idea to rely on this to configure many properties and manage more complex configurations for the longer term. For this it is better to manage properties in an 'application.properties' file which you can commit to your SCM for versioning and safe-keeping. The starter Wizard already created a empty 'application.properties' for us.

To help you edit such properties files STS 3.6.4 provides a brand new Spring Properties Editor. The editor provides nice content assist and error checking:

![props-editor]

The above screenshot shows a bit of 'messing around' with the content assist and error checking. The only propery shown that's really meaningful for our very simple 'error page App' right now is `server.port`. Try changing the port in the properties file and it should be picked up automatically when you run the app again. However be mindful that **properties overridden in the Launch Configuration take priority over `application.properties`**. So you'll have to uncheck or delete the `server.port` property in the Launch Configuration to see the effect.

##Making Our App more Interesting

Let's make our app more interesting. Here's what we'll do:
 
1. Create a 'Hello' rest service that returns a 'greeting' message. 
2. Make the greeting message configurable via Spring properties.
3. Set up the project so user-defined properties get nice editor support.

### Create a simple Hello Rest Service

To create the rest service you can follow [this guide][gs-rest]. For this guide we're doing something even simpler and more direct.
Create a controller class with this code:

```java
package demo;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
	@RequestMapping("/hello")
	public String hello(@RequestParam String name) {
		return "Hello "+name;
	}
}
```

You might want to this out by Relaunching (![Relaunch]) your app. The url `http://localhost:${port}/hello?name=kris` should return a text message "Hello Kris".

###Making the Greeting Configurable

This is actually quite easy to do, and you might be familiar with Spring's [@Value][ValueAnnotation] annotation. However, using `@Value` you won't be able get nice content assist. Spring Properties Editor won't be aware of properties you define that way. To understand why, it is useful to understand a little bit about how the Spring Properties Editor gets its information about the 'known properties'.

Some of the Spring Boot Jars starting from version 1.2.x contain special json metadata files that the editor looks for on your project's classpath and parses. These files contain information about the 'known properties'. If you dig for them a little, you can find these files from STS. For example, open "spring-boot-autoconfigure-1.2.2.RELEASE.jar" (under "Maven Dependencies") and browse to "META-INF/spring-configuration-metadata.json". You'll find properties like `server.port` being documented there.

![meta-data]

For our own user-defined properties to be picked-up by the editor we have to create this metadata. Fortunately this can be automated easily provided you define your properties using Spring Boot [@ConfigurationProperties]. So define a class like this:

```java
package demo;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties("hello")
public class HelloProperties {

    /**
     * Greeting message returned by the Hello Rest service.
     */
	private String greeting = "Welcome ";
	public String getGreeting() {
		return greeting;
	}
	public void setGreeting(String greeting) {
		this.greeting = greeting;
	}
}
```

The `@ConfigurationProperties("hello")` tells Boot to take configuration properties starting with "hello." and try to inject them into the BeanProperties of the `HelloProperties` Bean. The `@Component` annotation marks this class as a Component so that Spring Boot will pick up on it scanning the classpath and turn it into a Bean. Thus, if a configuration file (or another property source) contains a property `hello.greeting` then the value of that property will be injected to `setGreeting`.

Now, to actually use this property all we need is a `HelloProperties` bean. For example to customize the message returned by the rest service, we add a `@Autowired` field to the HelloController and call its `getGreeting` method:

```java
@RestController
public class HelloController {
	
	@Autowired
	HelloProperties props;
	
	@RequestMapping("/hello")
	public String hello(@RequestParam String name) {
		return props.getGreeting()+name;
	}
}
```

Relaunch your app again and try to access http://localhost:${port}/hello?name=yourname. You should get the default "Welcome yourname" message. 

Now go ahead and try editing `application.properties` and change the greeting to something else. We already have everything in place to correctly define the property, but, you'll notice that the editor is still unaware of our newly minted property:

![unknown-prop]

What's still missing to make the editor aware is the `spring-configuration-metadata.json` file documenting our property. This file is created at build-time by the `spring-boot-configuration-processor` which is a Java Annotation Processor. We have to add this processor to our project and make sure it is executed during project builds.

Add this to the `pom.xml`:

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-configuration-processor</artifactId>
</dependency>
```

Then perform a "Maven >> Update Project" to trigger a project configuration update, which should cause the processor to be activated.
If everything worked correctly. The warning from the editor will now disapear, and you'll also get proper Hove Info:

![hover-info]

Now that the annotation processor has been activated, any future changes to your `HelloProperties` class will trigger an automatic update of the json metadata. You can try it out by adding some extra properties, or renaming your `greeting` property to `message`. Warmings will appear / disapear as appropriate. If you are curious where your metadata file is, you can find it in `target/classes/META-INF`. The file is there, even though Eclipse does its best to hide it from you. It does this with all files in a project's output folder (thank you Eclipse :-). You can get around this by using the `Navigator` view which doesn't filter files as much and show you a more direct view on the actual resources in your workspace. Open this view via "Window >> Show View >> Other >> Navigator" and...

![navigator]

Note: We know that the manual step of adding the processor seems like an unnessary complication. We have plans to [automate this further][https://issuetracker.springsource.com/browse/STS-4040] in the future.

## The End

I hope you enjoyed this Tutorial. Comments and questions are welcome.

[menu-new-starter]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/menu-new-spring-starter.png
[new-starter-wizard]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/new-starter-wizard.png
[workspace-ready]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/workspace-ready.png
[run-as-boot]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/run-as-boot.png
[console-output]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/console-output.png
[Debug]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/debug-button.png
[Run]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/run-button.png
[Relaunch]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/relaunch-button.png
[run-conf-menu]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/run-conf-menu.png
[override-property]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/override-property.png
[props-editor]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/props-editor.png
[unknown-prop]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/unknown-prop.png
[navigator]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/navigator.png
[meta-data]:https://raw.githubusercontent.com/kdvolder/spring-blog-2015-03/master/img/meta-data.png


[Cloud Foundry]:http://cloudfoundry.org
[convert-jar-to-war]:https://spring.io/guides/gs/convert-jar-to-war/
[start.spring.io]:http://start.spring.io
[STS 3.6.4 release notes]:http://docs.spring.io/sts/nan/v364/NewAndNoteworthy.html
[Spring Profiles]:http://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties
[gs-rest]:https://spring.io/guides/gs/rest-service/
[ValueAnnotation]:http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Value.html
[@ConfigurationProperties]:http://docs.spring.io/autorepo/docs/spring-boot/1.2.1.RELEASE/api/org/springframework/boot/context/properties/ConfigurationProperties.html
