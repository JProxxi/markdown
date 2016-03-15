# Drupal 7 Testing Module (SimpleTest) | Knowledge Transfer
> Jackson Keenan | March 16<sup>th</sup>, 2016

## What is SimpleTest?
SimpleTest: The original name of the module used for testing in Drupal. Though it's now called just "Testing", the terms are used interchangeably. Based on the PHP SimpleTest library.

"SimpleTest is an open source unit test framework for the PHP programming language. The test structure is similar to JUnit/PHPUnit. SimpleTest supports mock objects and can be used to automate the regression testing of web applications with a scriptable HTTP Client that can parse HTML pages and simulate things like clicking on links and submitting forms." - Wikipedia
## Types of Tests
### Functional *
AKA Integration tests. These tests test the functionality of a specific aspect of an application by simulating regular human interaction with the system (Front End Tests).

"Drupal focuses on functional testing because it is more effective for the way Drupal is written. Functional testing ends up being just as effective if not more effective than unit testing and especially so due to the code style used in Drupal."
### Unit
Unit testing involves taking a small portion of the code (the unit), and subjecting it to programmatic tests which prove its 'correctness' (Back End Tests)
## Terminology

### Test Case
A Test Case is the set of conditions and environment variables under which a test or multi tests are be run.
### Test
A Test checks one discreet piece of functionality of the system. This is done by preforming the necessary actions using various pre-defined methods for navigating and manipulating Drupal, and then verifying the results using Assertions.
### Assertions
Assertions are the smallest unit of a test and simply tests whether or not a given condition is true or false. [[...]](https://www.drupal.org/node/265828)
## How it Works
Simpletest creates a complete Drupal installation and a virtual web browser and then uses the virtual web browser to walk the Drupal install through a series of tests, just like you would do if you were doing it by hand.

It is important to understand that each individual test runs in a completely new instance or a Drupal installation. This means that none of your configuration settings or users setup in previous tests will be wiped out as soon as the test is completed.
## Writing Tests
### File Setup
When writing tests for a particular Drupal module, we will mainly be dealing with 3 specific files. These are the `.module`, `.info`, and `.test` files of the specific module.

The `.module` file is simply the file defining the module we will be testing and shouldn't ever really need direct editing while developing tests. however it is still good to know about.

The `.info` file is used for various things however it's main purpose is notifying Drupal about the existence of a module;

In order for tests to be run on a module Drupal needs to know where those tests live. Add the following line to your modules `.info` file so Drupal can locate the test file:
```php
files[] = tests/someModule.test
```

The `.test` file is the test file for our module and is where all of the tests cases, and individual tests for that particular module live.
### Code
Each `.test` file is composed of 1 or more Drupal Test Case Classes which all extend either `DrupalWebTestCase` (Functional Tests) or `DrupalUnitTestCase` (Unit Tests). For now we will be focusing on `DrupalWebTestCase`. You can declare your class like so:
```
class TestTests extends DrupalWebTestCase{}
```
At this point there are a few things that need to be taken care of before actual test writing begins. First you should declare the Classes `getInfo()` function. This gives Drupal information about your tests that will be displayed in the test list.
```php
public static function getInfo() {
    return array(
      'name' => 'Some Name',
      'description' => 'Some Description',
      'group' => 'Some Group',
    );
  }
```
Next you need to define the `setUp()` function for your test case. This method will vary in complexity, but it's general purpose is to prepare the new Drupal installation (created by the test) for the upcoming tests by changing any necessary settings and enabling any needed modules. For example if your were testing the some custom module named `aodatest` you would need to enable it during the setup like so:
```php
public function setUp() {
    parent::setUp(array('aodatest'));
  }
```

At this point you should be ready to start writing the individual tests of the test case. there are simply standard public functions prefaced by the word 'test' Ex. `public function testFunction()`

At this point you should be setup with a bare bones test case that looks somthing like this.

```php
class TestTests extends DrupalWebTestCase {
  /**
   * Metadata about our test case.
   */

  public static function getInfo() {
    return array(
      'name' => 'Aoda Test: Test',
      'description' => 'Tests for the Aoda Test Module',
      'group' => 'Aoda Test: Test',
    );
  }

  /**
   * Perform any setup tasks for our test case.
   */

  public function setUp() {
    parent::setUp(array('aodatest'));
  }

  public function testAodaTest() {
    $this->drupalGet('');
    $this->assertText('Aoda Test', 'The link is present.');
    $this->clickLink('Aoda Test');
    $this->assertText('This is a test to demonstrate the basic functionallity of the Drupal Testing Module.', 'The page content is present.');
  }
}
```

## Running Tests
### Browser
### Command Line
### Failing Tests
