---
sidebar_position: 2
---

# Container Configuration

To configure fairy's container, we provide an annotation-based approach to configure the container. The container configuration can be done in multiple ways.

## Annotations

### @InjectableComponent
The `@InjectableComponent` is one of the most important annotations in fairy. It is used to mark a class as a component that can be injected into other components. The `@InjectableComponent` annotation can be used to mark a class as a component that can be injected into other components.

```java
@InjectableComponent
public class MyComponent {
    // ...
}
```

---
### @Configuration
The `@Configuration` annotation is used to mark a class as a configuration class.
Configuration classes are used to define components and their dependencies using annotated method approach.
You can define multiple components in a single configuration class.

```java
@Configuration
public class MyConfiguration {
    
    @InjectableComponent
    public MyComponent myComponent() {
        return new MyComponent();
    }
    
}

// No longer need to use @InjectableComponent annotation when it's defined in a configuration class.
public class MyComponent {
    // ...
}
```
