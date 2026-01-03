- Because autowiring by type may lead to multiple candidates, it is often necessary to have more control over the 
  selection process. One way to accomplish this is with Spring’s @Primary annotation. @Primary indicates that a 
  particular bean should be given preference when multiple beans are candidates to be autowired to a 
  single-valued dependency.
```java
package learn.bigotron.dev;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Primary;
import org.springframework.stereotype.Component;

@Component
public class AppConfig {

    @Bean
    public A getA() {
        return new A("hello from A");
    }

    @Bean
    @Primary
    public A getAnotherA() {
        return new A("hello from another A");
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
        System.out.println(a.getName()); // prints hello from another A
    }
}
```
- Alternatively, as of 6.2, there is a @Fallback annotation for demarcating any beans other than the regular 
  ones to be injected. So basically, @Fallback is the lowest priority bean injection.
```java
package learn.bigotron.dev;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Fallback;
import org.springframework.stereotype.Component;

@Component
public class AppConfig {

    @Bean
    public A customA() {
        return new A("hello from A"); // prints hello from A
    }

    @Bean
    @Fallback
    public A defaultA() {
        return new A("hello from another A"); // would print this line if we didn't have any other bean of type A
    }

}
```