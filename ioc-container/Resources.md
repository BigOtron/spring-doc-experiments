- Java’s URL is fine for the internet, but too weak and inflexible for accessing application resources like classpath 
  files or web-app resources, so Spring had to design a better abstraction.
- Java’s standard java.net.URL class and standard handlers for various URL prefixes, unfortunately, are not quite 
  adequate for all access to low-level resources. For example, there is no standardized URL implementation that may be 
  used to access a resource that needs to be obtained from the classpath or relative to a ServletContext. While it is 
  possible to register new handlers for specialized URL prefixes (similar to existing handlers for prefixes such 
  as http:), this is generally quite complicated, and the URL interface still lacks some desirable functionality, 
  such as a method to check for the existence of the resource being pointed to.
- Spring’s Resource interface located in the org.springframework.core.io. package is meant to be a more capable 
  interface for abstracting access to low-level resources. The following listing provides an overview of the 
  Resource interface.
```java
public interface Resource extends InputStreamSource {

	boolean exists();

	boolean isReadable();

	boolean isOpen();

	boolean isFile();

	URL getURL() throws IOException;

	URI getURI() throws IOException;

	File getFile() throws IOException;

	ReadableByteChannel readableChannel() throws IOException;

	long contentLength() throws IOException;

	long lastModified() throws IOException;

	Resource createRelative(String relativePath) throws IOException;

	String getFilename();

	String getDescription();
}
```
- Some implementations of the Resource interface also implement the extended WritableResource interface for a resource 
  that supports writing to it.
- Spring includes several built-in Resource implementations:
  - UrlResource
  - ClassPathResource
  - FileSystemResource
  - PathResource
  - ServletContextResource
  - InputStreamResource
  - ByteArrayResource