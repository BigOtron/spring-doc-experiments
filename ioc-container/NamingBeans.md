Ok, we must give a name to our beans so that spring can refer to them using that name. And that name should be unique
in the application context. Interestingly, you can give multiple names to one bean. And the first one will be called
the name of the bean and others will be called as aliases.
```java
package learn.bigotron.dev;

import org.springframework.context.annotation.Bean;

@org.springframework.context.annotation.Configuration
public class Con1 {
    @Bean(name = {"userService", "user_service"})
    public String userService() {
        return "configuration";
    }
}
```
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

        String service = context.getBean("user_service", String.class);
        System.out.println(service);

        String service2 = context.getBean("userService", String.class);
        System.out.println(service2);
    }
}
```
When you don't specify any bean name, spring will generate one for you. It will be the class/method name in camel-cased
formate with first letter in lowercase.
---
Another interesting feature of spring is that if two or more letters start with capital letters, then spring won't
make the first one lowercase, it just names the bean as it is. Now, it is better to learn from this example:
```java
package learn.bigotron.dev;

import org.springframework.context.annotation.Bean;

@org.springframework.context.annotation.Configuration
public class Con1 {
    @Bean(name = "UParser")
    public String urlParser() {
        return "parsed url is here";
    }
}
```
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
        
        // spring named the bean as 'UParser' not as 'uParser'
        // another example: IELTSBean will be named as 'IELTSBean' not as 'iELTSBean'
        String service = context.getBean("UParser", String.class);
        System.out.println(service);
    }
}
```