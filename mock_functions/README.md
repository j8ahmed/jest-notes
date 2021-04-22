# Jest Mock Functions

## The ```mockFunction.mock``` Property

<br/>

```Console.log(test.mock)```
{
    calls: [ [ 'Hello' ] ],
    instances: [ undefined ],
    invocationCallOrder: [ 2 ],
    results: [ { type: 'return', value: undefined } ]
}

    calls = An array of arrays which hold all the arguments 
            passed in as parameters for each call of the mock function.

    instances = An array of all the objects instances that have been 
                instantiated from this mock function using 'new'. When 
                a mock function is called (i.e. mockFn()), the corresponding 
                'instances' value in the array will be undefined since 
                it was not a new instance of the mock function being built?

    invocationCallOrder = An array of integers indicating in what order was 
                          this particular mock function was called relative to 
                          all the other mock functions that may exist in the 
                          test sutie (test file). This replaced the previous 
                          'mock timestamps' because multiple mock functions 
                          could potentially share the same timestamp which 
                          presented a problem when trying to test any
                          callorders between mocked functions.

    results = An array containing the results of all calls that have been made 
              to this mock function. Each entry is an object with 2 properties. 
              Ex: {type: 'return', value: 25}
              
            type = 
                'return' - call completed by returning normally.
                'throw' - call completed by throwing a value as an Error.
                'incomplete' - call has not yet completed. 
                    This occurs if you test the result from within the 
                    mock function itself, or from within a function that was
                    called by the mock.
              
            value = The value that was thrown or returned. 
                    value is undefined when type === 'incomplete'.
 */