# Priciples of TDD

## The cycle of TDD
### The Three Laws of TDD
- rule
  - failed test first
  - not more than a test
  - not more than production code
  
- R-G-B(R) cycle : Red, Green, Blue(Refactor)
  - write a failing test
  - pass a production test on code
  - refactor to make it faster

- Taglines
  - Make it work, Make it right, Make it fast

### F.I.R.S.T (by Robert C. Martin - 'Clean code')
#### FAST
A test should work fast and effective. Don’t make a test complicated and make it to be run slowly. Using a fast test developer will be helped to detect problem faster
#### INDEPENDENT
A test should not dependent to other test or determine other test conditions. That way, we can avoid downstream problem and other complicated and difficult problem.
#### REPEATABLE
A test should carried out in all environments in all conditions that can occur. Repeatable tests can avoid the occurrence of cases of a successful test in an environment, but fail in another environment.
#### SELF-VALIDATING
a test must be able to evaluate whether the expected output is appropriate or not by returning the output in a boolean form. The self-validating test can avoid the need to do an evaluation manually by us.
#### TIMELY
a test should create before we implement the code, because if we create the test after creating the code, there will be a tendency for developers to have difficulty testing the code.

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








