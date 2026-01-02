- Spring lets you configure your application using annotations placed directly in your Java classes.
  Behind the scenes, Spring uses BeanPostProcessors to detect these annotations and apply their behavior (like dependency injection) so the IoC container knows what to do.
```java
@Component // marks the class as a Spring bean
public class OrderService {

    @Autowired // marks a field that needs a dependency
    private PaymentService paymentService;
}
```
- In the above configuration, Spring does the following behind the scenes:
  - Spring creates an OrderService object
  - `AutowiredAnnotationBeanPostProcessor` runs
  - It scans the class for `@Autowired`
  - It finds `paymentService`
  - It injects the correct `PaymentService` bean into it
- @Autowired is Spring’s annotation for dependency injection, offering more control than traditional autowiring.
  Spring also supports standard Java annotations for dependency injection and lifecycle management, such as @PostConstruct, @PreDestroy, and @Inject, making your code less Spring-specific.
```java
@Named        // JSR-330 (standard Java DI)
public class OrderService {

    private final PaymentService paymentService;

    @Inject    // works like @Autowired
    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }

    @PostConstruct   // called after dependencies are injected
    public void init() {
        System.out.println("OrderService is ready");
    }

    @PreDestroy      // called before bean is destroyed
    public void cleanup() {
        System.out.println("OrderService is shutting down");
    }
}
```
- Spring’s Java-based configuration mainly uses `@Configuration` classes and `@Bean` methods.
  A `@Bean` method creates and configures an object that Spring manages—just like a `<bean>` in XML.
  While `@Bean` can be used in any `@Component`, it’s primarily intended for use inside `@Configuration` classes.
```java
@Configuration
public class AppConfig {

    @Bean
    public UserService userService() {
        return new UserService();
    }
}

@Configuration
public class AppConfig {

  @Bean
  public OrderService orderService(PaymentService paymentService) { // Spring will automatically inject PaymentService
    return new OrderService(paymentService);
  }
}
```
- The reason why Spring recommends using `@Configuration` not `@Component` with `@Bean` is that the first annotation
  guarantees the `@Bean` methods are called once if the scope is singleton.
```java
package learn.bigotron.dev;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.stereotype.Component;

@Configuration
public class AppConfig {

    @Bean
    public A getA() {
        return new A("hello from A");
    }

    @Bean
    public B getB() {
        return new B(getA());
    }

}
```
```java
package learn.bigotron.dev;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext("learn.bigotron.dev");

        A a = context.getBean(A.class);
        B b = context.getBean(B.class);
        // If the annotation is @Configuration, we get true but if it is @Component it prints false
        System.out.println(a == b.getA());
    }
}
```
- `@Configuration` causes Spring to proxy the class so that calls between `@Bean` methods go through the IoC container, 
  guaranteeing singleton reuse; without it, method calls are plain Java and can create multiple instances.
- When @Bean methods are declared within classes that are not annotated with @Configuration, or when 
  @Configuration(proxyBeanMethods=false) is declared, they are referred to as being processed in a "lite" mode. 
  In such scenarios, @Bean methods are effectively a general-purpose factory method mechanism without special 
  runtime processing (that is, without generating a CGLIB subclass for it). A custom Java call to such a method will 
  not get intercepted by the container and therefore behaves just like a regular method call, creating a new 
  instance every time rather than reusing an existing singleton (or scoped) instance for the given bean.
- 


