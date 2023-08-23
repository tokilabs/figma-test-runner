Did you take into account that the user of the test runner will call functions named `suite` and `test` that  only define a title for the suite/test and the function that the runner will have to execute later using.

- the `suite` will signal the runner to create and register a `Suite`
- the same goes for `test` and `Test`
- a call to `suite` or `test` inside another `suite` call must register the Suite/Test it creates as a child of the Suite create by the parent `suite` call

Example:

```js
suite('A', () => {
  suite('B', () => {
    test('T', () => {})
  });
});
```

should create a structure like this:

```js
Suite(A) {
    name: 'A',
    scope: #ref:TestRunner,
    suites: [
        Suite(B) {
        name: 'B',
        scope: #ref:Suite(A),
        tests: [
            Test {
            name: 'T',
            scope: #ref: Suite(B)
            }
        ]
        }
    ]
}
```

That structuring of suites and tests based on the calls to suite() and test() is a common pattern for test runners. It should be built and stored in the test runner instance to keep track of the hierarchy, before executing the tests.
