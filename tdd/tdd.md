# Testing Driven Development

TDD follows this process:

1. Write a test
2. Run a test and check that it fails as expected
3. Write relevant function/app/component
4. Run test again until it passes
5. Write a new test

## Best Practices

1. Each test should test one thing
2. One test file per application code source file
3. A test for every function/class
4. Don't test constants
5. Test behavior, not implementation
6. Always think about edge cases

## Functional Tests

Functional tests also go by the name *acceptance tests* or *end-to-end tests*. The purpose is to examine how an application functions, from the outside. For example, tests for a web app will pay attention to the sort of details that will be revealed to the user when they go to the site with their web browser.

`assert 'Shiny New Web Site' in browser.title`

## Python's `unittest` Library

Python has a `unittest` module included with its standard library. It supports tdd in an object-oriented way. [The Python docs](https://docs.python.org/3.6/library/unittest.html) use the following sample code to demonstrate how the module works.

```
import unittest

class TestStringMethods(unittest.TestCase):

    def test_upper(self):
        self.assertEqual('foo'.upper(), 'FOO')

    def test_isupper(self):
        self.assertTrue('FOO'.isupper())
        self.assertFalse('Foo'.isupper())

    def test_split(self):
        s = 'hello world'
        self.assertEqual(s.split(), ['hello', 'world'])
        # check that s.split fails when the separator is not a string
        with self.assertRaises(TypeError):
            s.split(2)

if __name__ == '__main__':
    unittest.main()
```

We can observe a couple of interesting details from this brief script.

1. Tests in Python are written in an object oriented way. You will write a testing class that inherits properties from `unittest.TestCase`.

2. Tests in Python are meant to be contained in a separate file that you run from the terminal. There are various flags you can run, for example, to increase verbosity, etc.

3. All test functions have a `test_` prefix that indicate they are test runners.

4. The `unittest` module has a lot of helper methods that expand the `assert` vocabulary so you can perform more precise tests.

5. Also, not covered in this example, but worth mentioning are the `setUp` and `tearDown` methods. These are methods that are run before and after a particular test to help create a particular set of conditions for a test. For example, for a web app it would be helpful to start and stop the process of running a headless browser.
