- Data binding is useful for binding user input to a target object where user input is a map with property paths as 
  keys, following JavaBeans conventions. DataBinder is the main class that supports this, and it provides two ways to 
  bind user input:
    - Constructor binding - bind user input to a public data constructor, looking up constructor argument values 
      in the user input.
    - Property binding - bind user input to setters, matching keys from the user input to properties of the target 
      object structure.
- You can apply both constructor and property binding or only one.
- To use constructor binding:
  - Create a DataBinder with null as the target object.
  - Set targetType to the target class.
  - Call construct.
- The target class should have a single public constructor or a single non-public constructor with arguments. 
  If there are multiple constructors, then a default constructor if present is used.
- Type conversion is applied as needed to convert user input. If the constructor parameter is an object, it is 
  constructed recursively in the same manner, but through a nested property path. That means constructor binding 
  creates both the target object and any objects it contains.
- 

