Bean overriding occurs when a bean has the same name as another bean in a different configuration. Spring will save the
last bean it reads as the winner bean. We should note that bean overriding will be deprecated in future releases. We are
currently at spring 7.
```java
// first config class
package learn.bigotron.dev;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Con1 {
    @Bean(name = "bossName")
    public String bossName() {
        return "James from EvilCorp";
    }
}
```
```java
// second config class
package learn.bigotron.dev;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Con2 {
    @Bean(name = "bossName")
    public String bossName() {
        return "Mary from MegaCorp";
    }
}
```
```java
package learn.bigotron.dev;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Configuration;

import java.util.Arrays;

@Configuration
public class Main {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext("learn.bigotron.dev");
        Arrays.stream(context.getBeanDefinitionNames()).forEach(System.out::println);

        String bossName = context.getBean("bossName", String.class);
        System.out.println(bossName); // prints out 'Mary from MegaCorp' but it depends on the package scanning
    }
}
```
Well, it looks cool to know about this but this feature can lead to unexpected outcomes especially if you integrate 
your app with third-party libraries. So it is much better to turn this feature off.
```java
package learn.bigotron.dev;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
        context.setAllowBeanDefinitionOverriding(false);
        context.scan("learn.bigotron.dev");
        context.refresh();

        String bossName = context.getBean("bossName", String.class);
        System.out.println(bossName);
    }
}
```
Now, if you run this configuration, you will get a nice `BeanDefinitionOverrideException` which is kinda cool.

---
Ok, one very, very interesting feature of bean overriding is that if you have two beans with the same name, the one
annotated with `@Bean` will SILENTLY override the other bean even if you explicitly turned off bean overriding. JUST WOW!!!
```java
package learn.bigotron.dev;

import org.springframework.stereotype.Component;

@Component
public class UserService {
    private String whichDef = "component";

    public UserService() {
    }

    public UserService(String whichDef) {
        this.whichDef = whichDef;
    }

    public void sayHi() {
        System.out.printf("I am inside %s sayHi bean", whichDef);
    }
}
```
```java
package learn.bigotron.dev;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Con1 {
    @Bean
    public UserService userService() {
        return new UserService("configuration");
    }
}
```
Here, we have two beans with the same name, but one is defined as a component bean and the other using `@Bean` annotation.
```java
package learn.bigotron.dev;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
        context.setAllowBeanDefinitionOverriding(false);
        context.scan("learn.bigotron.dev");
        context.refresh();

        UserService service = context.getBean("userService", UserService.class);
        service.sayHi();
    }
}
```
Here, even if we explicitly turned off bean overriding, we don't get an exception, we get `I am inside configuration sayHi bean`
as an output. So, the next question is WHY??? Simply because, spring considers `@Bean` annotated beans as an INTENTIONAL
configuration, not an accident. So Java config is considered more explicit than component scanning.
