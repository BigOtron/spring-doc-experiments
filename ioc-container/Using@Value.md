- @Value is a Spring annotation that injects a value into a field, constructor parameter, or setter method.
```java
@Value("${catalog.name}")
private String catalog;
```
```java
@Component
public class MovieRecommender {

    private final String catalog;

    public MovieRecommender(@Value("${catalog.name}") String catalog) {
        this.catalog = catalog;
    }
}
```
- @PropertySource("classpath:application.properties") → tells Spring to load application.properties from the classpath
```java
@Configuration
@PropertySource("classpath:application.properties")
public class AppConfig { }
```
- By default, Spring silently injects unresolved placeholders as literal strings; defining a 
  PropertySourcesPlaceholderConfigurer makes placeholder resolution strict, causing the application to fail fast 
  when a property is missing.
```java
@Bean
    public static PropertySourcesPlaceholderConfigurer placeholderConfigurer() { // must be static
        return new PropertySourcesPlaceholderConfigurer();
    }
```
- Spring Boot configures by default a PropertySourcesPlaceholderConfigurer bean that will get properties from 
  application.properties and application.yml files.
- Built-in converter support provided by Spring allows simple type conversion (to Integer or int for example) to be 
  automatically handled. Multiple comma-separated values can be automatically converted to String array without extra 
  effort.
```java
package learn.bigotron.dev;

import org.springframework.beans.factory.annotation.Value;
import java.util.Arrays;

public class A {
    @Value("${app.supported.languages}")
    private final String[] supportedLanguages;

    public void printSupportedLanguages() {
        Arrays
                .stream(supportedLanguages)
                .forEach(System.out::println);
    }
}
// inside application.properties:
// app.supported.languages=en,uz,de,fr
```
- It is possible to provide a default value as following:
```java
@Component
public class MovieRecommender {

	private final String catalog;

	public MovieRecommender(@Value("${catalog.name:defaultCatalog}") String catalog) {
		this.catalog = catalog;
	}
}
```
- A Spring BeanPostProcessor uses a ConversionService behind the scenes to handle the process for converting the 
  String value in @Value to the target type. If you want to provide conversion support for your own custom type, 
  you can provide your own ConversionService bean instance as the following example shows:
```java
@Configuration
public class AppConfig {

	@Bean
	public ConversionService conversionService() {
		DefaultFormattingConversionService conversionService = new DefaultFormattingConversionService();
		conversionService.addConverter(new MyCustomConverter());
		return conversionService;
	}
}
```