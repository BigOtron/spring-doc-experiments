- The Environment interface is a Spring abstraction that represents information about where and how your application 
  is running. It is integrated into the IoC container so every Spring ApplicationContext already has an Environment.
- The Environment interface has two main responsibilities, namely profiles and properties:
  - Profiles allow you to group beans and configurations and activate them conditionally. A profile is a named, 
    logical group of bean definitions to be registered with the container only if the given profile is active. 
    Beans may be assigned to a profile whether defined in XML or with annotations. The role of the Environment 
    object with relation to profiles is in determining which profiles (if any) are currently active, and which 
    profiles (if any) should be active by default.
  - Properties play an important role in almost all applications and may originate from a variety of sources: 
    properties files, JVM system properties, system environment variables, JNDI, servlet context parameters, 
    ad-hoc Properties objects, Map objects, and so on. The role of the Environment object with relation to 
    properties is to provide the user with a convenient service interface for configuring property sources and 
    resolving properties from them.
- Bean definition profiles provide a mechanism in the core container that allows for registration of different beans 
  in different environments.
- The @Profile annotation lets you indicate that a component is eligible for registration when one or more specified 
  profiles are active. We can write the dataSource configuration as follows:
```java
@Configuration
@Profile("development") // gets registered only if development profile is active
public class StandaloneDataConfig {

	@Bean
	public DataSource dataSource() {
		return new EmbeddedDatabaseBuilder()
			.setType(EmbeddedDatabaseType.HSQL)
			.addScript("classpath:com/bank/config/sql/schema.sql")
			.addScript("classpath:com/bank/config/sql/test-data.sql")
			.build();
	}
}
```
```java
@Configuration
@Profile("production") // gets registered only if production profile is active
public class JndiDataConfig {

	@Bean(destroyMethod = "")
	public DataSource dataSource() throws Exception {
		Context ctx = new InitialContext();
		return (DataSource) ctx.lookup("java:comp/env/jdbc/datasource");
	}
}
```
- You can use @Profile as a meta-annotation for the purpose of creating a custom composed annotation. The following 
  example defines a custom @Production annotation that you can use as a drop-in replacement for @Profile("production"):
```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Profile("production")
public @interface Production {
}
```
- If a @Configuration class is marked with @Profile, all the @Bean methods and @Import annotations associated with 
  that class are bypassed unless one or more of the specified profiles are active. 
- @Profile can also be declared at the method level to include only one particular bean of a configuration class 
  (for example, for alternative variants of a particular bean), as the following example shows:
```java
@Configuration
public class AppConfig {

	@Bean("dataSource")
	@Profile("development")
	public DataSource standaloneDataSource() {
		return new EmbeddedDatabaseBuilder()
			.setType(EmbeddedDatabaseType.HSQL)
			.addScript("classpath:com/bank/config/sql/schema.sql")
			.addScript("classpath:com/bank/config/sql/test-data.sql")
			.build();
	}

	@Bean("dataSource")
	@Profile("production")
	public DataSource jndiDataSource() throws Exception {
		Context ctx = new InitialContext();
		return (DataSource) ctx.lookup("java:comp/env/jdbc/datasource");
	}
}
```
- There might be problems with overloaded methods that create beans and that are annotated with @Profile. So the
  solution is like above; use different method names and mark the bean names as the same.