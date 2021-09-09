# Priciples of TDD

## The cycle of TDD
### The Three Laws of TDD
- rule
  - failed test first
  - not more than a test
  - not more than production code
  
- R-G-R cycle : Red, Gree, Refactor
  - write a failing test
  - pass a production test on code
  - refactor to make it faster

- Taglines
  - Make it work, Make it right, Make it fast

## Types of Software Testing
- Unit testing : single(isolated) case or group of code, and input to output matching test
- Itegration testing : group of components togeter under a specific hardware
- Performance testing : speed of system
- Usability testing : user perspectives w.r.t. how to satisfy them or their needs
- Acceptance testing : a requirement from users
- Regression testing : a modification of the existing service/product

### Unit Testing 
#### Template
- Arrange
- Act
- Assert

#### Characteristics of good test
- clean, readable, and maintainable
- isolated / single responsibility
- no logic (no conditional statement)
- not too specific or too general

#### Test or not
Do
- Query : each execution part shows right response
- Command : making a sytem or change a state like messege queue / external dependency

Don't
- launguage feature
- 3rd party code

#### Naming convention and structure
- [Class name] + ['Tests' post-fix]
  e.g. MyClass -> MyClassTests
- Number of Tests >= Number of Execution Paths
  e.g.

- Clearly test business rules
- Test Methods : [MethodName]_[Scenario]_[ExpectedBehaviour]
  e.g. CanBeBookedBy() -> CanBeBookedBy_AdminBook_ReturnTrue()
  e.g. Add() -> Add_WhenCalled_ReturnTheSumOfArgument() (Scenario can be a generic term when no specific scenario is not required.

#### Technicques

#### Reliable Tests








