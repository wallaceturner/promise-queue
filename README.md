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
var queue = new Queue(1, Infinity, { timeout: 2000 });
```

### Add a promise that does not resolve
```js            
queue.add(function () {
        return new Promise(function (resolve, reject) {
            //does not resolve
        });
    })
    .then(function () {
        //does not reach here
    }, function (error) {
        console.error(error);//logs 'message did not complete execution after timeout of 2000'
    })
```






