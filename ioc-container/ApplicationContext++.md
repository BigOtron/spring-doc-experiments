- To enhance BeanFactory functionality in a more framework-oriented style, the context package also provides the 
  following functionality:
  - Access to messages in i18n-style, through the MessageSource interface.
  - Access to resources, such as URLs and files, through the ResourceLoader interface.
  - Event publication, namely to beans that implement the ApplicationListener interface, through the use of the 
    ApplicationEventPublisher interface.
  - Loading of multiple (hierarchical) contexts, letting each be focused on one particular layer, such as the web layer 
   of an application, through the HierarchicalBeanFactory interface.
---
### Internationalization using MessageSource
- In Spring, the MessageSource interface is used for internationalization (i18n) and externalizing user-facing messages.
- The ApplicationContext interface extends an interface called MessageSource and, therefore, provides 
  internationalization (“i18n”) functionality. Spring also provides the HierarchicalMessageSource interface, which 
  can resolve messages hierarchically. Together, these interfaces provide the foundation upon which Spring effects 
  message resolution. The methods defined on these interfaces include:
  - `String getMessage(String code, Object[] args, String default, Locale loc)`: The basic method used to retrieve a 
    message from the MessageSource. When no message is found for the specified locale, the default message is used. 
    Any arguments passed in become replacement values, using the MessageFormat functionality provided by the standard
    library.
  - `String getMessage(String code, Object[] args, Locale loc)`: Essentially the same as the previous method but with 
    one difference: No default message can be specified. If the message cannot be found, a NoSuchMessageException is 
    thrown.
- When an ApplicationContext is loaded, it automatically searches for a MessageSource bean defined in the context. 
  The bean must have the name messageSource. If such a bean is found, all calls to the preceding methods are 
  delegated to the message source. If no message source is found, the ApplicationContext attempts to find a parent 
  containing a bean with the same name. If it does, it uses that bean as the MessageSource. If the ApplicationContext
  cannot find any source for messages, an empty DelegatingMessageSource is instantiated in order to be able to accept
  calls to the methods defined above.
- Spring provides three MessageSource implementations, ResourceBundleMessageSource, 
  ReloadableResourceBundleMessageSource and StaticMessageSource. All of them implement 
  HierarchicalMessageSource in order to do nested messaging. The StaticMessageSource is rarely used but provides 
  programmatic ways to add messages to the source. The following example shows ResourceBundleMessageSource:
```java
@Bean
public MessageSource messageSource() {
    ResourceBundleMessageSource ms = new ResourceBundleMessageSource();
    ms.setBasename("messages", "errors", "validation");
    ms.setDefaultEncoding("UTF-8");
    return ms;
}

// in messages.properties file: message=Hallo
// in errors.properties file: argument.required=The {0} argument is required.
```
- The next example shows a program to run the MessageSource functionality. Remember that all ApplicationContext 
  implementations are also MessageSource implementations and so can be cast to the MessageSource interface.
```java
public static void main(String[] args) {
	MessageSource resources = new ClassPathXmlApplicationContext("beans.xml");
	String message = resources.getMessage("message", null, "Default", Locale.ENGLISH);
	System.out.println(message); // prints Hallo
}
```
- The next example shows arguments passed to the message lookup. These arguments are converted into String objects and 
  inserted into placeholders in the lookup message.
```java
public class Example {

	private MessageSource messages;

	public void setMessages(MessageSource messages) {
		this.messages = messages;
	}

	public void execute() {
		String message = this.messages.getMessage("argument.required",
			new Object [] {"userDao"}, "Required", Locale.ENGLISH);
		System.out.println(message);
	}
}
```