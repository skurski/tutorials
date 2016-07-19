# Test Driven Development (Test Driven Design)
___Development approach keeping tests one step ahead of your code___

===

## 3 Steps of TDD (TDD Cycle):
1. __Red__ - write the test, test will fail because there is no implementation
2. __Green__ - write just enought implementation code to make the test pass
3. __Refactor__ - test pass so clean the code and __Repeat Cycle__

## TDD best practices:
* use descriptive names for test methods
* only write new code when test is failing
* only refactor after all tests are passing
* rerun all tests every time implementation code changes
* all tests should pass before new test is written
* write the simplest code to pass the test
* minimize assertions in each test

===

## JUnit core constructs:
1. __Setup__
  * @Before - will be executed before every test
  * @BeforeClass - will be executed before first test in the class
2. __Execution__
  * @Test
  * @Ignore - ignore the test
3. __Verification__ (methods:)
  * __org.junit.Assert.*__
  ```java 
  Assert.assertEquals("Brown", customer.lastName()); 
  ```
  * __HamcrestMatcherAssert__ - addition library provides extensions for junit assertion mechanism, more readable
  ```java 
  MatcherAssert.assertThat(customer.lastName(), equalTo("Brown")); 
  ```
4. __Teardown__
  * @After - will be executed after every test
  * @AfterClass - will be executed after last test in the class

===

## Mockito concepts
* implement the mocked functionality for a dependency
* uses proxy pattern
* we test only class under test, not its dependecies

### Stages:
* __Creating the mock__ - we create a mock object
 ```
 OrderDao mockOrderDao = Mockito.mock(OrderDao.class);
 ```
* __Method stubbing__ - we indicate how the mock object should behave in concreate situation
 ```
 Mockito.when(mockOrderDao.findById(idValue)).thenReturn(orderFixture);
 ```
