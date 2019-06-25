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
