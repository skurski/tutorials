# Test Driven Development (Test Driven Design)
___Development approach keeping tests one step ahead of your code___

===

## 3 Steps of TDD (TDD Cycle):
1. __Red__ - write the test, test will fail because there is no implementation
2. __Green__ - write just enought implementation code to make the test pass
3. __Refactor__ - test pass so clean the code and __Repeat Cycle__

===

## JUnit core constructs:
1. __Setup__
  * @Before - will be executed before every test
  * @BeforeClass - will be executed before first test in the class
2. __Execution__
  * @Test
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
