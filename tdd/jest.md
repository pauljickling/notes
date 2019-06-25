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
