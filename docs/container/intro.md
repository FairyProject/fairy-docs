---
sidebar_position: 1
---

# Introduction 
Here let's talk about what is fairy IOC container and how it can be used to manage the dependencies of your application.

## What is it?
Fairy container is a lightweight and easy-to-use dependency injection container for Java. 
It is designed to be simple and easy to use, yet powerful and flexible, 
Built on top of the Java Reflection API and uses annotations to define components and their dependencies.
It is heavy inspired by [Spring Framework](https://spring.io/).

## Why use it?
Fairy container is designed to make your code more modular, testable, and maintainable.
Let's see a very simple example to understand why you should use fairy container.

---
### Pure Java
```java
public class MyJavaPlugin extends JavaPlugin {
    
    private final MyService myService;
    private final MySecondService mySecondService;
    private final MyThirdService myThirdService;
    
    @Override
    public void onEnable() {
        myService = new MyService();
        mySecondService = new MySecondService(myService);
        myThirdService = new MyThirdService(myService, mySecondService);
        
        myService.onPostInitialize();
        mySecondService.onPostInitialize();
        myThirdService.onPostInitialize();
        
        this.getServer().getPluginManager().registerEvents(new MyListener(myService, mySecondService, myThirdService), this);
    }
    
    @Override
    public void onDisable() {
        myService.onPreDisable();
        mySecondService.onPreDisable();
        myThirdService.onPreDisable();
    }
}

public class MyService {
    public void onPostInitialize() {
        // ...
    }
    
    public void onPreDisable() {
        // ...
    }
}

public class MySecondService {
    private final MyService myService;

    public MySecondService(MyService myService) {
        this.myService = myService;
    }
    
    public void onPostInitialize() {
        // ...
    }
    
    public void onPreDisable() {
        // ...
    }
}

public class MyThirdService {
    private final MyService myService;
    private final MySecondService mySecondService;

    public MyThirdService(MyService myService, MySecondService mySecondService) {
        this.myService = myService;
        this.mySecondService = mySecondService;
    }

    public void onPostInitialize() {
        // ...
    }

    public void onPreDisable() {
        // ...
    }
}

public class MyListener implements Listener {
    private final MyService myService;
    private final MySecondService mySecondService;
    private final MyThirdService myThirdService;

    public MyListener(MyService myService, MySecondService mySecondService, MyThirdService myThirdService) {
        this.myService = myService;
        this.mySecondService = mySecondService;
        this.myThirdService = myThirdService;
    }
}
```

---
### With Fairy

```java
@InjectableComponent
public class MyService {
    @PostInitialize
    public void onPostInitialize() {
        // ...
    }

    @PreDestroy
    public void onPreDestroy() {
        // ...
    }
}

@InjectableComponent
public class MySecondService {
    private final MyService myService;

    public MySecondService(MyService myService) {
        this.myService = myService;
    }

    @PostInitialize
    public void onPostInitialize() {
        // ...
    }

    @PreDestroy
    public void onPreDestroy() {
        // ...
    }
}

@InjectableComponent
public class MyThirdService {
    private final MyService myService;
    private final MySecondService mySecondService;

    public MyThirdService(MyService myService, MySecondService mySecondService) {
        this.myService = myService;
        this.mySecondService = mySecondService;
    }

    @PostInitialize
    public void onPostInitialize() {
        // ...
    }

    @PreDestroy
    public void onPreDestroy() {
        // ...
    }
}

@InjectableComponent
@RegisterAsListener
public class MyListener implements Listener {
    private final MyService myService;
    private final MySecondService mySecondService;
    private final MyThirdService myThirdService;

    public MyListener(MyService myService, MySecondService mySecondService, MyThirdService myThirdService) {
        this.myService = myService;
        this.mySecondService = mySecondService;
        this.myThirdService = myThirdService;
    }
}
```

Essentially, fairy container allows you to define your components and their dependencies using annotations.
This example is very small, but as your project grows, the benefits of using fairy container will become more and more apparent.

Fairy solves the problem of managing the dependencies of your application, and it does so in a way that is easy to use and understand.
If you are familiar with Spring Framework, you will feel right at home with fairy container.

Look at the [next section](/container/configuration) to get started with fairy container.