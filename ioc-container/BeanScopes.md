- The spring framework supports 6 scopes, four of which are available only if you use a web-aware `ApplicationContext`.
  We can also create our own custom scopes. The default scope is singleton.
```java
@Component
public class UserService {
    public String getUser() {
        return "queried user returned";
    }
}

public class Main {
  public static void main(String[] args) {
    ApplicationContext context = new AnnotationConfigApplicationContext("learn.bigotron.dev");
    UserService service = context.getBean(UserService.class);
    UserService service1 = context.getBean(UserService.class);
    System.out.println(service1 == service); // prints true
  }
}
```
If the scope was a prototype, we would get false:
```java
@Component
@Scope("prototype")
public class UserService {
    public String getUser() {
        return "queried user returned";
    }
}
```
- When a spring bean is scoped as singleton, spring creates exactly one object, keeps it in memory, and returns the 
  same object every time it is requested. Important note is that since singleton beans are shared, they must be thread-safe
  and avoid mutable state.
```java
@Service
public class VisitCounterService {

  private int counter = 0;
  
  public int incrementAndGet() {
    counter++;
    return counter;
  }
} 
```
One solution to the above problem can be:
```java
@Service
public class VisitCounterService {

    private final AtomicInteger counter = new AtomicInteger(0);

    public int incrementAndGet() {
        return counter.incrementAndGet();
    }
}
```