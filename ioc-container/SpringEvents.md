Let's first talk about creating custom events in spring. There are two ways to create events in spring. First one is
kinda legacy now, and the second one, well you guessed it right, more modern and encouraged way. Let's go with the
old way first.
```java
package learn.bigotron.dev;

import org.springframework.context.ApplicationEvent;

public class NewUserSignUpEvent extends ApplicationEvent {
    private final String userName;
    private final String userEmail;

    public NewUserSignUpEvent(Object source, String userName, String userEmail) {
        super(source);
        this.userName = userName;
        this.userEmail = userEmail;
    }

    public String getUserName() {
        return userName;
    }

    public String getUserEmail() {
        return userEmail;
    }
}
```
As you can see above, to create a custom event in spring, you simply extend `ApplicationEvent` abstract class. Now,
let's go a bit deeper into `ApplicationEvent`. It has two constructors which basically do the same thing. 
```java
public ApplicationEvent(Object source) {
    super(source);
    this.timestamp = System.currentTimeMillis();
}
// and the second one
public ApplicationEvent(Object source, Clock clock) {
    super(source);
    this.timestamp = clock.millis();
}
```
So, the `source` parameter, as its name suggests, represents the object on which this particular event occurred and 
sets the `timestamp` field to the time when this event fired.
Do you still remember that we have a newer way of defining our custom events? Ok, you just define a simple POJO and you
are done. Yeah, I know, it is very easy. So the above event will be like this in newer way:
```java
package learn.bigotron.dev;

public class NewUserSignUpEvent {
    private final String userName;
    private final String userEmail;

    public NewUserSignUpEvent(String userName, String userEmail) {
        this.userName = userName;
        this.userEmail = userEmail;
    }

    public String getUserName() {
        return userName;
    }

    public String getUserEmail() {
        return userEmail;
    }
}
```
As it is a simple POJO, we can even turn it into a `record` type.
```java
package learn.bigotron.dev;

public record NewUserSignUpEvent(
        String userName, 
        String userEmail
) {
}
// looks nice, doesn't it?
```
---
Ok, now the exciting part: we will learn how to define our publishers and of course, listeners. Now, let's talk about
`ApplicationEventPublisher` interface. Spring uses this interface to publish events. So any bean that implements this 
interface can publish events. Good thing is that `ApplicationContext` implements this interface. This interface has
`publishEvent()` method that, yeah again you guessed it, publishes events your app creates.
```java
package learn.bigotron.dev;

import org.springframework.context.ApplicationEventPublisher;
import org.springframework.stereotype.Component;

@Component
public class UserRegisterService {

    private final ApplicationEventPublisher publisher;

    public UserRegisterService(ApplicationEventPublisher publisher) {
        this.publisher = publisher;
    }

    public void registerNewUser(String username, String email) {
        System.out.println("⏳⌛Registering is in progress...");
        publisher.publishEvent(new NewUserSignUpEvent(username, email));
    }
}
```
Wow, we have just published an event. But sad thing is that we don't have any listeners who actually care about this
event. So why not create them right?
```java
package learn.bigotron.dev;

import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Component;

@Component
public class NewSignUpListener {
    @EventListener
    public void handleNewUserSignUp(NewUserSignUpEvent event) {
        System.out.printf("\uD83C\uDF89Congrats, %s on singing up on our platform\n", event.userName());
        System.out.printf("\uD83C\uDF81As you are new here, we have sent a valuable coupon to your email at %s\n", event.userEmail());
    }
}
```
