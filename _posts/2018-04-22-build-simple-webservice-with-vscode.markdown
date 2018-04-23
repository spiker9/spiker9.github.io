---
layout: post
title:  "Build Simple Webservice with Spring Boot in VScode"
date:   2018-04-22 10:20:32 -0500
categories: Java
---

**1. Configure VScode for Java & Spring Development**

First you'll need to install Java (Better to be 1.8 above), Maven and Visual Studio Code and I'm sure you know where to download them.
After you open it, you'll find the extensions icon on the left side bar which you can search and install all extensions from there. The extensions you'll need for this topic is *Language Support for Java(TM) by Red Hat*, *Maven for Java* and *Spring Inittializr Java Support*.
![image]({{"/assets/2018-4-22/1.JPG" | absolute_url}})

**2. Create your Spring Boot Project**
With *Spring Initializr Java Support*, you can create a spring project base on Maven or Gradle that has project structure and configuration files auto generated.
* Press <span style="color:Brown"> Ctrl + Shift + P </span>
* Choose *Spring Initializr: Generate a Maven Project* and follow the wizard.

**3. Customize the Project**

Since we are building a web service, the basic <code>spring-boot-starter</code> is not enough. We need to do some tuning for the auto generated pom file.

**Update**
{% highlight xml %}
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter</artifactId>
</dependency>
{% endhighlight %}
**TO**
{% highlight xml %}
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
{% endhighlight %}

After you change the dependencies, VScode will auto build the project and add the new dependency.

**4. Code Workd**
* Create a domain object that your web service will return.
<span style="color:Brown">Greeting.java</span>
{% highlight java %}
public class Greeting{

    private final Long id;

    private final String content;

    public Greeting(long id, String content) {
        this.id = id;
        this.content = content;
    }

    public long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }
}
{% endhighlight %}
* Create a Contoller
<span style="color:Brown">MSController.java</span>
{% highlight java %}
@RestController
public class MSController{
    
    private static final String template = "Hello, %s";

    private final AtomicLong count = new AtomicLong();

    @RequestMapping("/greeting")
    public Greeting greetings(@RequestParam(value = "name", defaultValue = "World") String name) {
        return new Greeting(count.incrementAndGet(), String.format(template, name));
    }
}
{% endhighlight %}

The <code>@RestController</code> annotation is used for Spring to detect this class as RESTful web service. The <code>@RequestMapping</code> will bind the HTTP request <code>/greeting</code> to the method <code>greetings()</code>. <code>@RequestParam</code> binds the string param <code>name</code> in the request to the <code>name</code> parameter of the method. If the param in request is empty, it will use the default value "World".

**5. Run Application**

Press <span style='color:Brown'>F5</span> and VS code will start your spring boot application. Now open your browser and type <code>localhost:8080/greeting</code>. You will get {"id":1,"content":"Hello, World"} in response. Type <code>localhost:8080/greeting?name=Ryan</code>. You will get {"id":2,"content":"Hello, Ryan"} in return.