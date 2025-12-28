- Dependency Injection means an object does not create or look up the objects it depends on; instead, those 
  dependencies are provided to it by a container, which is why it’s called Inversion of Control.
- Dependency Injection makes code cleaner and more loosely coupled because objects don’t create or search for their
  dependencies, and this makes classes much easier to test by allowing mock or stub implementations to be injected.
- In Spring, DI exists in two major variants: Constructor-based dependency injection and Setter-based dependency
  injection.
- Constructor-based DI is accomplished by the container invoking a constructor with a number of arguments, 
  each representing a dependency.