- To enhance BeanFactory functionality in a more framework-oriented style, the context package also provides the 
  following functionality:
  - Access to messages in i18n-style, through the MessageSource interface.
  - Access to resources, such as URLs and files, through the ResourceLoader interface.
  - Event publication, namely to beans that implement the ApplicationListener interface, through the use of the 
    ApplicationEventPublisher interface.
  - Loading of multiple (hierarchical) contexts, letting each be focused on one particular layer, such as the web layer 
   of an application, through the HierarchicalBeanFactory interface.
- 