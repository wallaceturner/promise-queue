Promise-based queue with timeout
This project is a fork of [promise-queue](https://github.com/promise-queue/promise-queue)

## Installation

`promise-queue-timeout` can be installed using `npm`:

```
npm install promise-queue-timeout
```

## Example

### Add a timeout of 2 seconds

```js
import PromiseQueue = require('promise-queue-timeout');
var queue = new PromiseQueue(1, Infinity, { timeout: 2000 });
```

### Add two functions to the queue. The first does not resolve and will time out.
```js            
let func1 = () => {  return new Promise(function (resolve, reject) {   }); } //does not resolve

queue.add(func1)
    .then((result:any)=> {
        
    }, function (error) {
        console.error(error);//logs 'message did not complete execution after timeout of 2000'
    })

let func2 = () => {  return new Promise(function (resolve, reject) {  resolve('func2')  }); } //resolves
queue.add(func2)
    .then((result:any)=> {
        console.log('complete with result: ' + result)
    }, function (error) {
        //no error expected
    })
```

### Output
```js 
message did not complete execution after timeout of 2000
complete with result: func2
```






