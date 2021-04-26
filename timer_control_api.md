# Jest Timer Control API

Jest provides a timer control API as well as timer mocks to help us run tests on asynchronous code set to specific times i.e., using ```setTimeout```, ```setInterval```, etc.

Examples:
```javascript

// Not sure what this does exactly yet. It might jsut be self explanatory lol. I guess I haven't confirmed what are the implications of using fakeTimers in tests yet.
jest.useFakeTimers();

// Fast-forward until all timers have been executed
jest.runAllTimers();

// Fast forward and exhaust only currently pending timers
// (but not any new timers that get created during that process)
jest.runOnlyPendingTimers();


// Fast-forward until all timers have been executed
jest.advanceTimersByTime(1000);

//Lastly, it may occasionally be useful in some tests to be able to clear all of the pending timers. For this, we have:
jest.clearAllTimers().
```

## ```.useFakeTimers(mode?: 'modern' | 'legacy')```

Instructs Jest to use fake versions of the standard timer functions: 
- ```setTimeout```
- ```setInterval```
- ```clearTimeout```
- ```clearInterval```
- ```nextTick```
- ```setImmediate```
- ```clearImmediate```

If you pass ```'modern'``` as an argument, ```'@sinonjs/fake-timers'``` will be used as ```mode``` instead of Jest's own fake timers. This also mocks additional timers like ```Date```. ```'modern'``` will be the default behavior in **Jest 27**.

The method returns the ```jest``` object for chaining.

*We might use this method at the beginning of the test file to implement the fake timers on all our tests in the file.*

Example:
```javascript
// timerGame.js
'use strict';

function timerGame(callback) {
  console.log('Ready....go!');
  setTimeout(() => {
    console.log("Time's up -- stop!");
    callback && callback();
  }, 1000);
}

module.exports = timerGame;
```


```javascript
// __tests__/timerGame-test.js
'use strict';

jest.useFakeTimers();

test('waits 1 second before ending the game', () => {
  const timerGame = require('../timerGame');
  timerGame();

  expect(setTimeout).toHaveBeenCalledTimes(1);
  expect(setTimeout).toHaveBeenLastCalledWith(expect.any(Function), 1000);
});
```
<br/><br/>

## ```.advanceTimersByTime( milliseconds )```

```jest.advanceTimersByTime( milliseconds )``` formerly referred to as ```.runTimersToTime(milli...)``` prior to Jest v.22.0.0.
<br/><br/>
Allows us to advance the time in a mock timer by a value in milliseconds. (I think)
