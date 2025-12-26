A bean definition tells spring how to create an object, but it is not the object itself. So spring stores bean 
definitions in the container and creates objects only when needed. Spring basically has two ways to create beans 
based on bean definitions using reflection. First one is `new Bean()`, yeah that is simple and the second one is to
call a static factory method `MyFactory.createBean()`.
```java
package learn.bigotron.dev;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;

class Connection {
    private final String connectionType;

    public Connection(String connectionType) {
        this.connectionType = connectionType;
    }

    public String getConnectionType() {
        return connectionType;
    }
}

class PostgreConnection extends Connection {
    public PostgreConnection(String connectionType) {
        super(connectionType);
    }
}

class MySQLConnection extends Connection {
    public MySQLConnection(String connectionType) {
        super(connectionType);
    }
}

class ConnectionFactory {
    public static Connection createConnection(String connectionType) {
        if (connectionType.equals("postgresql")) {
            return new PostgreConnection(connectionType);
        } else if (connectionType.equals("mysql")) {
            return new MySQLConnection(connectionType);
        }
        return new Connection("generic connection");
    }
}

@org.springframework.context.annotation.Configuration
public class Con1 {
    @Value("${db.type:postgresql}") // this defaults to 'postgresql' if no such property matches
    private String dbType;
    @Bean
    public Connection getConnection() {
        return ConnectionFactory.createConnection(dbType);
    }
}
```
In the above code, we are creating a `Connection` bean using a static factory method. The benefit of doing so is that 
we can decide which subclass to use during runtime.
---
Ok, now let's talk more about spring way of creating bean objects using constructors. One best feature of spring is 
that spring doesn't ask you to implement a particular framework related interface or class. So basically, it doesn't 
pollute your code with its extensions. A no-arg constructor is needed when spring creates an object first then injects
its dependencies. It happens when using setter injection or field injection.
According to spring documentation, a true java bean has these features:
- has a public no-arg constructor
- has private fields
- has getters and setters
- follows naming conventions
