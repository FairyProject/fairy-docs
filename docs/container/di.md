---
sidebar_position: 3
---

# Dependency Injection

Dependency injection is a core concept of fairy container system. It is used to inject the dependencies of a component into the component itself.

## Constructor Injection

The most common and recommended way to inject dependencies is through constructor injection.

**✅ Advantages**
- It makes the dependencies of a component explicit.
- Full control over the dependencies of a component, where they are injected and how they will be stored.
- It makes the component immutable.
- It makes the component easier to test.

**❌ Disadvantages**
- It can make the constructor signature large and hard to read. (This can be solved by using Lombok, but some people may dislike it.)

### Pure Java
```java
@InjectableComponent
public class MyComponent {
    
    private final MyDependency myDependency;
    
    public MyComponent(MyDependency myDependency) {
        this.myDependency = myDependency;
    }
    
}
```

### With Lombok
```java
@InjectableComponent
@RequiredArgsConstructor
public class MyComponent {
    
    private final MyDependency myDependency;
    
}
```

---
## Field Injection

Field injection is not recommended, but it is supported by fairy.

**✅ Advantages**
- Straightforward and easy to use.

**❌ Disadvantages**
- It makes the component mutable.
- It makes the component harder to test.
- It makes the dependencies of a component implicit.

```java
@InjectableComponent
public class MyComponent {
    
    @Autowired
    private MyDependency myDependency;
    
    @Autowired
    private MyOtherDependency myOtherDependency;
    
}
```

---
## Configuration Injection

Configuration injection is the way to inject dependencies in a configuration-based approach.
This approach is similar to the constructor injection, but you can make it so that the component is not aware of its dependencies.

**✅ Advantages**
- It makes the dependencies of a component explicit.
- Full control over the dependencies of a component, where they are injected and how they will be **injected**. the actual component class doesn't need to be aware of its dependencies.
- It makes the component easier to test.

**❌ Disadvantages**
- You need to manually create the component instance in the configuration class.

```java
@Configuration
public class MyConfiguration {
    
    @InjectableComponent
    public MyComponent myComponent(MyDependency myDependency) {
        return new MyComponent(myDependency);
    }
    
}

public class MyComponent {
    
    private final MyDependency myDependency;
    
    public MyComponent(MyDependency myDependency) {
        this.myDependency = myDependency;
    }
    
}
```

### Cool Feature
```java
@Configuration
public class MyConfiguration {
    
    @InjectableComponent
    public MyComponent myComponent(MyDependency myDependency) {
        return new MyComponent(myDependency.getSomething());
    }
    
}

public class MyComponent {
    
    private final String something;
    
    // You can inject the actual value of the dependency while not exposing the dependency itself.
    public MyComponent(String something) {
        this.something = something;
    }
    
}

@InjectableComponent
public class MyDependency {
    
    public String getSomething() {
        return "something";
    }
    
}
```

---
## Conclusion

Personally, I'd recommend using both constructor injection and configuration injection. Constructor injection is the most recommended way to inject dependencies, but configuration injection can be useful in some cases. Field injection is not recommended, but it is supported by fairy.

There isn't really a strict rule on how injection can be done, you can mix and match them as you like!