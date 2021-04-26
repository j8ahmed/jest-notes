## ```.any(constuctor)```

```expect.any(constuctor)``` matches anything that was created with the given ```constructor```. You can use it inside toEqual or toBeCalledWith instead of a literal value. For example, if you want to check that a mock function is called with a number:

```javascript
function randocall(fn) {
  return fn(Math.floor(Math.random() * 6 + 1));
}

test('randocall calls its callback with a number', () => {
  const mock = jest.fn();
  randocall(mock);
  expect(mock).toBeCalledWith(expect.any(Number));
});
```
<br/>

In this example the mock function is called with ```Math.floor(Math.random() * 6 + 1)```, so ```expect.any(Number)``` asserts true.
<br/><br/>

## ```.toHaveBeenLastCalledWith(arg1, arg2, ...)```

```expect.toHaveBeenLastCalledWith(arg1, arg2, ...)``` A.K.A ```expect.lastCalledWith(arg1, arg2, ...)```. We can use this method on a **mock function** to test what arguments it was last called with.