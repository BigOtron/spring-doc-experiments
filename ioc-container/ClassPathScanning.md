- You know the option for implicitly detecting the candidate components by scanning the classpath. Candidate components
  are classes that match against filter criteria and have a corresponding bean definition registered with the container. 
  This removes the need to use XML to perform bean registration. Instead, you can use annotations 
  (for example, @Component), AspectJ type expressions, or your own custom filter criteria to select which classes have 
  bean definitions registered with the container.
- The @Repository annotation is a marker for any class that fulfills the role or stereotype of a repository 
  (also known as Data Access Object or DAO). Among the uses of this marker is the automatic translation of exceptions.
- Spring provides further stereotype annotations: @Component, @Service, and @Controller. @Component is a generic 
  stereotype for any Spring-managed component. @Repository, @Service, and @Controller are specializations of 
  @Component for more specific use cases (in the persistence, service, and presentation layers, respectively). 
  Therefore, you can annotate your component classes with @Component, but, by annotating them with @Repository, 
  @Service, or @Controller instead, your classes are more properly suited for processing by tools or associating 
  with aspects. For example, these stereotype annotations make ideal targets for pointcuts. @Repository, @Service, 
  and @Controller may also carry additional semantics in future releases of the Spring Framework. Thus, if you are 
  choosing between using @Component or @Service for your service layer, @Service is clearly the better choice. 
  Similarly, as stated earlier, @Repository is already supported as a marker for automatic exception translation 
  in your persistence layer.
- Many of the annotations provided by Spring can be used as meta-annotations in your own code. A meta-annotation is 
  an annotation that can be applied to another annotation. For example, the @Service annotation mentioned earlier 
  is meta-annotated with @Component, as the following example shows:
```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Service {

	// ...
}
```
- You can also combine meta-annotations to create “composed annotations”. For example, the @RestController annotation 
  from Spring MVC is composed of @Controller and 
- Spring can automatically detect stereotyped classes and register corresponding BeanDefinition instances with 
  the ApplicationContext. To autodetect these classes and register the corresponding beans, you need to add 
  @ComponentScan to your @Configuration class, where the basePackages attribute is configured with a common 
  parent package for the two classes. 
```java
@Configuration
@ComponentScan(basePackages = "org.example")
public class AppConfig  {
	// ...
}
```

