- If you have multiple beans of the same type and want to choose exactly which one to inject, you use @Qualifier 
  with a name. This is more precise than @Primary because it explicitly selects the bean.
```java
@Autowired
@Qualifier("main")
private MovieCatalog movieCatalog;
```
- You can also specify the @Qualifier annotation on individual constructor arguments or method parameters, 
  as shown in the following example:
```java
public class MovieRecommender {

	private final MovieCatalog movieCatalog;

	private final CustomerPreferenceDao customerPreferenceDao;

	@Autowired
	public void prepare(@Qualifier("main") MovieCatalog movieCatalog,
			CustomerPreferenceDao customerPreferenceDao) {
		this.movieCatalog = movieCatalog;
		this.customerPreferenceDao = customerPreferenceDao;
	}
}
```
- Sometimes, you want a more semantic way than just bean names. This is where custom qualifier annotations come in.
```java
@Target({ElementType.FIELD, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Qualifier
public @interface Genre {
    String value();
}
```
```java
@Component
@Genre("fiction")
public class FictionBookService implements BookService {
}

@Component
@Genre("non-fiction")
public class NonFictionBookService implements BookService { 
}
```
```java
@Autowired
@Genre("fiction")
private BookService myBooks;
```