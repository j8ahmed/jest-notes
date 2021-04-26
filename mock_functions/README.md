# Jest Mock Functions

This section covers details and examples regarding the Jest Mock Function, Mocking modules with ```jest.mock()``` & manual mocks, ```jest.spyOn```,  etc.

---

## ```jest.fn()``` (Mock Function Properties / Methods)
<br/>

Every mock function has access to unique properties and methods that help us keep track of it's usage during tests. 
<br/>
<br/>

### ```MockFunction.mock``` property
---

Specifically, the ```.mock``` property is an object that holds information related each respective mock function's call history. This information includes:

- The parameter's it was called with (```.calls```),
- The instances derived from the mock function (```.instances```), e.g., ```new mockFn()```
- The order in which it was called (```.invocationCallOrder```)
- The values returned each time the mock function was called (```.results```)


```javascript
const mockFn = jest.fn()
mockFn("Hello")
console.log(mockFn.mock)
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
<br/>
<br/>
### Other Mock Function Properties / Methods
---

When we build a mock function and then console log it, we can see that it is a JavaScript Function object with unique properties. A lot of these functions are fairly self-explanatory so we will only highlight the most frequently used methods / properties.

```javascript
const mockFn = jest.fn()
console.log(mockFn)
```

Prints:
```
 console.log
    [Function: app$load$anim] {
      _isMockFunction: true,
      getMockImplementation: [Function (anonymous)],
      mock: [Getter/Setter],
      mockClear: [Function (anonymous)],
      mockReset: [Function (anonymous)],
      mockRestore: [Function (anonymous)],
      mockReturnValueOnce: [Function (anonymous)],
      mockResolvedValueOnce: [Function (anonymous)],
      mockRejectedValueOnce: [Function (anonymous)],
      mockReturnValue: [Function (anonymous)],
      mockResolvedValue: [Function (anonymous)],
      mockRejectedValue: [Function (anonymous)],
      mockImplementationOnce: [Function (anonymous)],
      mockImplementation: [Function (anonymous)],
      mockReturnThis: [Function (anonymous)],
      mockName: [Function (anonymous)],
      getMockName: [Function (anonymous)]
    }
```

```._isMockFunction``` true,

```.getMockImplementation``` [Function (anonymous)],

```.mock``` [Getter/Setter],

```.mockClear``` [Function (anonymous)],

```.mockReset``` [Function (anonymous)],

```.mockRestore``` [Function (anonymous)],

```.mockReturnValueOnce``` [Function (anonymous)],

```.mockResolvedValueOnce``` [Function (anonymous)],

```.mockRejectedValueOnce``` [Function (anonymous)],

```.mockReturnValue``` [Function (anonymous)],

```.mockResolvedValue``` [Function (anonymous)],

```.mockRejectedValue``` [Function (anonymous)],

```.mockImplementationOnce``` [Function (anonymous)],

```.mockImplementation``` [Function (anonymous)],

```.mockReturnThis``` [Function (anonymous)],

```.mockName``` [Function (anonymous)],

```.getMockName``` [Function (anonymous)]
<br/>
<br/>

---
<br/>

### ```jest.mock( modulePathName, factory?, options? )```




### ```jest.spyOn( object, methodName, accessType? )```

- object = The ES6 module object that we want to 


<br/>

---

<br/>
<br/>

### TypeScript considerations

There are multiple little quirks involved when using TypeScript with Jest since it only recently started to support TypeScript.
<br/><br/>

#### **Access Mocked function properties / methods on a mocked module object or mocked class without errors in TypeScript**

Example:
<br/>
```typescript
import ModuleName from './moduleName'
jest.mock('./moduleName')

const mockedModule = ModuleName as jest.Mocked<typeof ModuleName>
```
<br/><br/>

#### **Access Mocked function properties / methods on a mocked function without errors in TypeScript**

Example:
<br/>
```typescript
import ModuleName from './moduleName'
jest.mock('./moduleName')

const mockedFunction = ModuleName.functionName as jest.MockedFunction<typeof ModuleName.functionName>
```
