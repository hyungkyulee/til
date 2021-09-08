# Software Design Patters in OOP

## SOLID Principle
### Single Responsibility
- Tom DeMarco, "There should never be more than one reason for a class to change."
- E. Cobham Brewer, "None but Buddha himself must take the responsibility of giving out occult secrets."

> Separate con-joined responsibilities to a single one which has its own intension or purpose with characters and behaves.

### Open/Close
"Software entities ... should be open for extension, but closed for modification."

> Abstract common methods as a base class to close modifications, but this will open to add and extend more children classes to have the methods.

### Liskof Substitution

Objects of a superclass shall be replaceable with objects of its subclasses without breaking the application.
Interface can play a role to support this principle because an implementation of a specification of interface can be replaced without breaking a business logic.
e.g. Repository Interface to bind with the various database contexts.

### Interface Segregation
"no client should be forced to depend on methods it does not use."

> irrelavant interface's methos should not be linked with a proper child class.
> separate the interface in a small piece to meet the purpose of subclass group rather than don't be a bigger superclass

[bad example]
```
interface ISmartDevice
{
  ...
  void Print();
  void Scan();
}

class SmartWatch : ISmartDevice
{
  ...
  public void Fax()
  {
      throw new NotSupportedException();
  }
}
```

### Dependency Inversion
Robert C. Martin
- High-level modules should not depend on low-level modules. Both should depend on abstractions.
- Abstractions should not depend on details. Details should depend on abstractions.

> Down-Top dependency.

## Other Patterns
### Factory Pattern
we create object without exposing the creation logic to the client and refer to newly created object using a common interface.
In class-based programming, the factory method pattern is a creational pattern that uses factory methods to deal with the problem of creating objects without having to specify the exact class of the object that will be created.

Two main benefits
- have a builder class (or provider or wrapper) : if the class has many parameter, the factory class is useful to do the same instance creation repeatedly.
- manage easily a variation of different business logics : e.g. class CaculatorFactory() -> class PriceCalc() : ICalc or class SpecialOfferCalc() : ICalc
Factory class will return an interface of the related classes according to the business logic
