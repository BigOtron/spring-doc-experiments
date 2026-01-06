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