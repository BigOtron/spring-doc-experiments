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

