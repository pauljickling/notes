# Jest

Jest is a JavaScript testing framework. Install with npm

`npm install --save-dev jest`

## Config

Test file names have the format of `{name}.test.js`

The `package.json` should contain the following section:

```
{
  "scripts": {
    "test": "jest"
  }
}
```

The test is run through the command line via `npm run test` or `npm test`

## Format

A typical test file will look like this:

```
const sum = require('./sum');

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

`expect` evaluates its parameter with the `.toBe` method's parameter.

## Matchers

Whenever our test expects something out of a parameter, it is tested against a matcher like `toBe`. `toBe` uses the `Object.is` method to test exact equality. Here are some other matchers:

- `toEqual` unlike `toBe`, `toEqual` tests the value of an object, thus if you had two separate objects with a value of 4, it would fail a `toBe` test, but pass a `toEqual` test.
- `toBeNull` checks for matches to `null`
- `toBeUndefined` checks for matches to `undefined`
- `toBeDefined` checks for any matches that are not `undefined`
- `toBeTruthy` checks for anything that an `if` statement would evaluate as true
- `toBeFalsy` checks for anything that an `if` statement would evaluate as false
- `toBeCloseTo` is useful for checking float values where perfect precision would be impossible
- `toMatch` is used to evaluate strings against regular expressions
- `toContain` is used in the way in Python you would check things with the statement `if x in arr`
- `toThrow` is used to test that a function throws an error when its called

## Asynchronous Code

Asynchronous code is a core aspect of JavaScript, and this can pose specific problems for your test suite. Jest has several tools for handling these difficulties.

First, their is a `test` ... `done =>` pattern to handle callbacks.

```
test('data should be 100', done => {
  function callback(data) {
    expect(data).toBe(100);
    done();
  }
});
```

If `done()` is never called, the test fails.

In the case of promises, simply return the promise, and Jest waits for the promise to resolve. In cases where you expect the promise to fail use the `.catch` method, and `expect.assertions` to verify how many assertions are called.

The `.resolves` matcher also resolves promises. For promises expected to fail there is the `.rejects` matcher.

```
test('data should be 100', () => {
  return expect(fetchData()).resolves.toBe(100);
});

test('fetch fails with an error', () => {
  return expect(fetchData()).rejects.toMatch('error');
});
```

Jest also supports the `async`/`await` syntax

```
test('the data is 100', async() => {
  const data = await fetchData();
  expect(data).toBe(100);
});
```

## Setup and Teardown

If there is something that needs to be done for many tests, you can use `beforeEach` and `afterEach`.

```
beforeEach(() => {
  initDb();
});

afterEach(() => {
  clearDb();
});
```

For cases where setup is only needed once there are the `beforeAll` and `afterAll` methods.

By default `before` and `after` blocks apply to every test in a file. Tests can also be grouped up together with a `describe` block.
