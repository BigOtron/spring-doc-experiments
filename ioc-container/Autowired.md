- You can apply the @Autowired annotation to constructors, as the following example shows:
```java
public class MovieRecommender {

	private final CustomerPreferenceDao customerPreferenceDao;

	@Autowired
	public MovieRecommender(CustomerPreferenceDao customerPreferenceDao) {
		this.customerPreferenceDao = customerPreferenceDao;
	}
    
}
```
- An @Autowired annotation on such a constructor is not necessary if the target bean defines only one constructor. 
  However, if several constructors are available and there is no primary or default constructor, at least one of 
  the constructors must be annotated with @Autowired in order to instruct the container which one to use.
- You can apply the @Autowired annotation to traditional setter methods, as the following example shows:
```java
public class SimpleMovieLister {

	private MovieFinder movieFinder;

	@Autowired
	public void setMovieFinder(MovieFinder movieFinder) {
		this.movieFinder = movieFinder;
	}
    
}
```
- You can apply @Autowired to methods with arbitrary names and multiple arguments, as the following example shows:
```java
public class MovieRecommender {

	private MovieCatalog movieCatalog;

	private CustomerPreferenceDao customerPreferenceDao;

	@Autowired
	public void prepare(MovieCatalog movieCatalog,
			CustomerPreferenceDao customerPreferenceDao) {
		this.movieCatalog = movieCatalog;
		this.customerPreferenceDao = customerPreferenceDao;
	}
    
}
```