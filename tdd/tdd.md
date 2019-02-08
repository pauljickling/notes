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

# Functional Tests

Functional tests also go by the name *acceptance tests* or *end-to-end tests*. The purpose is to examine how an application functions, from the outside. For example, tests for a web app will pay attention to the sort of details that will be revealed to the user when they go to the site with their web browser.

`assert 'Shiny New Web Site' in browser.title`
