# OOP (Object Oriented Programming) related design patterns

### 1. Flyweight Pattern
- Design pattern in which instead of creating instances every time, create and save the instance somewhere and share it among clients. The instance is only created when its property is different.
- Useful for efficient memory management if same type of instance is being created multiple times inside the program.
- The following factors exist inside Flyweight pattern:
    - Client: Part of system that uses the object.
    - Concrete Class: Class which is actually used to create instances
    - Factory: Where client requests for getting an instance of the object. Factory will look for its inside (eg. hashmap) to find if there's already an instance created with the same properties and return the pre-made instance if it finds one. Otherwise, it will create one and store the instance for reusability.
- Example use case:
    - Trees in minecraft (most of them would have same colors. Creating only one per color and sharing is more efficient)


### 2. Singletone Pattern
- Design pattern restricting the maximum number of created instance per class to 1.
- The 'single' instance is accessible from global area.
- Has some issues like testing difficulties, tight coupling, limited extendability, ..
- Can be utilized if:
    - Single instance rule needs to be preserved (eg. Multiple instances could require conflicts, No need for extra)
    - Global access is required for an object (especially if the object is expensive)
- Example use cases:
    - logger
    - DB connection
    - Cache management

##### Flyweight vs Singletone
- They both have limitations on number of instances that can be created.
- Flyweight allows multiple instances as long as they have different properties.
- Singletone always guarantees that there is one instance.