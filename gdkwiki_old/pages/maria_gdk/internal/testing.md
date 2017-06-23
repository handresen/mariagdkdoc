# Unit and Integration Tests

Writing tests is not about finding bugs, it is a robust way of designing software components (“units”) interactively so that their behaviour is specified through tests.

Maria GDK uses [Nunit](http://www.nunit.org/index.php?p=home) as the test framework and [FakeItEasy](https///github.com/FakeItEasy/FakeItEasy/wiki) as the mocking library.

`<WRAP important round>`
To be able to run all tests locally in Visual Studio:

*  Visual Studio 2013 Update 4 or higher must be installed

*  Resharper version 8.2.3 or higher must be installed

*  Use a separate AppDomain for each assembly with tests. To enable this:
      * Open Resharper options from the Visual Studio main menu (RESHARPER->Options...)
      * On the Options dialog goto Tools->Unit Testing
      * Check **Use separate AppDomain for each assembly with tests**
      * Save Resharper options
`</WRAP>`

## Guidelines when writing tests


*  **Test only one code unit at a time**: When you write unit tests (not integration tests) try to test one unit of code, this unit can have multiple use cases. You should always test each use case in a separate test case.


*  **Make each test independent to all the others**: Do not make a chain of test cases. It will prevent you from identifying the root cause of test case failures and you will have to debug the code. Also, it creates dependencies, meaning if you change one test case then you have to make changes to multiple test cases unnecessarily. 


*  **Make each test assembly independent to all the others**: Do not create references between test assemblies, common code must be put in separate test-utility assemblies.


*  **Mock out all external services and state**: Otherwise, behavior in those external services overlaps multiple tests, and state data means that different tests can influence each other’s outcome. 


*  **Do not use data members in a test class**: Each test case should be independent of each other, so there shall never be a need to have data members (fields, properties, static or non-static) in the class.
    

*  **Do not change access modifiers in a class for testability**: You should not test private members. Either test the private members through another test or refactor your code so that the private member becomes part of a new public interface which you can test.
    

*  **Don’t make unnecessary assertions**: Tests are a design specification of how a certain behavior should work, not a list of observations of everything the code happens to do. Do not try to assert everything just focus on what you are testing otherwise you will end up having multiple test case failures for a single reason.


*  **Don’t test configuration settings**: Your configuration settings aren’t part of any unit of code. 


*  **Use the AAA pattern**: Separated blocks ("ARRANGE", "ACT" and "ASSERT") help to improve maintainability. Consider using comments to mark these blocks.


*  **Put assertion parameters in the proper order**: Assert methods takes usually two parameters. One is expected value and second is actual value. Pass them in sequence as they are needed. This will help in correct message parsing if something goes wrong.


*  **Use descriptive messages in assert methods**: You can write a message in assert methods that will be displayed if the assert fails. Describe WHY the test failed and not WHAT went wrong.


*  **A test must be deterministic**: Do not use random data or make assumptions about task completion order or duration in a test. It will result in non-deterministic test results.


*  **A unit test must not lock global resources**: When you lock global resources such as ports, your unit tests cannot run in parallel. If you need to lock global resources the test needs to be an integration test instead.


*  **Structure all test cases**: Use [Nunit Test Categories](http://www.nunit.org/index.php?p=category&r=2.6.3). These are used by the CI to filter tests to run on checkin and is crusial to fast build time. Use one or more from the following categories:
    * "LongRunning" longer than 1 sec
    * "Long" longer than 100 msec
    * "Unit" unit test
    * "Integration" Integration test

## What to test

FIXME When writing tests you should aim at true unit tests or integration tests. The hybrid inbetween unit and integration tests tend to have an unclear goal and have high maintenance. Integration tests are what you should write when writing tests for existing, refactored, or modified code. Unit tests are what you should write when writing tests for a new pice of code where you can start by writing a failing test and then implement the code that makes the test pass (TDD process).

The key is to write meaningful tests not as many tests as possible. That is quality over quantity. The more meaningful tests you have, the more confident you'll feel working with, and using, the entire system. Use code coverage as a means of identifying where the testing effort should be focused.


*  **Test the basic cases (core function)**: This will tell you when the code breaks after you make some changes.

*  **Test the unexpected cases**: This will ensure that the code can handle error situations.
      * **Input values**: Test values such as Null, NaN, index outside a collection's range and others depending on the unit of code being tested
      * **Edge-cases**: For numbers, test 0, smallest, largest, etc. For collections test empty, first, last, etc. Again the unit of code being tested will suggest the edge-cases.

`<WRAP important round>`

**Whenever you find a bug**: If possible write a test case to cover it before fixing it. You should have a good reason for not writing a test for the bug.
`</WRAP>`

## Test structure and naming convention

In the Maria GDK solution all test assemblies are organized under the **Test** folder at the root level of the solution tree. 

Under the **Test** folder test assemblies are organized into folders according to the layer the tests belong to. If tests are added for a layer that has no folder, create a folder under **Tests** with the appropriate layer name and add the new test assembly to the new folder. 

There are two folders with tests that don't map to a specific layer:

*  If tests do not belong to a specific layer, or the Maria Facade, they are put in the **Common** folder

*  If tests belong to the Maria Facade they are put in the **Maria** folder

All test assembly names must end with **.Tests**. Example: TPG.GeoFramework.SomeName.Tests.dll

Name your tests clearly and consistently. This helps to avoid comments and increases the maintainability. Use the convention: **UnitOfWork_StateUnderTest_ExpectedBehavior**. Example: ParseXml_EmptyString_ExceptionThrown(). 

## Test code quality

You should write test code as you would any other piece of code. The tests should be easy to maintain and understand, duplication should be minimized, and common code with value should be placed in utility test assemblies. Remeber that each time you change the code being tested the code that tests that functionality will most likely have to be changed.


## Further reading


*  [Writing Great Unit Tests: Best and Worst Practices](http://blog.stevensanderson.com/2009/08/24/writing-great-unit-tests-best-and-worst-practises/)

*  [Naming standards for unit tests](http://osherove.com/blog/2005/4/3/naming-standards-for-unit-tests.html/)
