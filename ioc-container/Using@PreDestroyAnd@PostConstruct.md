- What is CommonAnnotationBeanPostProcessor?
  - It’s a Spring class that automatically detects certain annotations on your beans.
  - Specifically, it recognizes:
  - @Resource → for dependency injection
  - @PostConstruct → a method to run after the bean is created and initialized
  - @PreDestroy → a method to run before the bean is destroyed
  - It has been available since Spring 2.5.
```java
public class CachingMovieLister {

	@PostConstruct
	public void populateMovieCache() {
		// populates the movie cache upon initialization...
	}

	@PreDestroy
	public void clearMovieCache() {
		// clears the movie cache upon destruction...
	}
}
```
