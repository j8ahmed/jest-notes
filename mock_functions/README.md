# Jest Mock Functions

This section covers details and examples regarding the Jest Mock Function, Mocking modules, ```jest.spyOn```, etc.

---

## The ```mockFunction.mock``` Property

<br/>

Every mock function has access to unique properties and methods that help us keep track of it's usage during tests. Specifically, the ```.mock``` property is an object that holds information related each respective mock function's call history. This information includes:

- The parameter's it was called with (```.calls```),
- The instances derived from the mock function (```.instances```), e.g., ```new mockFn()```
- The order in which it was called (```.invocationCallOrder```)
- The values returned each time the mock function was called (```.results```)


```javascript
const mockFn = jest.fn()
mockFn("Hello")
console.log(mockFn.mock)
<br/>
```

Prints:

```
{
    calls: [ [ 'Hello' ] ],
    instances: [ undefined ],
    invocationCallOrder: [ 2 ],
    results: [ { type: 'return', value: undefined } ]
}
```

- ```.calls```
    - An array of arrays which hold all the arguments passed in as parameters for each call of the mock function.

- ```.instances```
    - An array of all the object instances that have been instantiated from this mock function using 'new'. When a mock function is called (i.e. ```mockFn()```), the corresponding 'instances' value in the array will be ```undefined``` since it was not a new instance of the mock function being built?

- ```.invocationCallOrder```
    - An array of integers indicating in what order was this particular mock function was called relative to all the other mock functions that may exist in the test sutie (test file). This replaced the previous 'mock timestamps' because multiple mock functions could potentially share the same timestamp which presented a problem when trying to test any callorders between mocked functions.

- ```.results```
    - An array containing the results of all calls that have been made to this mock function. Each entry is an object with 2 properties. 
    <br/>
    Ex: ```{type: 'return', value: 25}```<br/><br/>
        - ```.type```<br/>
                1. 'return' - call completed by returning normally.<br/>
                2. 'throw' - call completed by throwing a value as an Error.</br>
                3. 'incomplete' - call has not yet completed.<br/> 
                    *This occurs if you test the result from within the 
                    mock function itself, or from within a function that was
                    called by the mock.*<br/><br/>
        - ```.value```<br/>
                The value that was thrown or returned. 
                    value is ```undefined``` when ```type === 'incomplete'```.