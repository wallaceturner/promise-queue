# promise-queue [![NPM Version](https://badge.fury.io/js/promise-queue.png)](https://npmjs.org/package/promise-queue) [![Build Status](https://travis-ci.org/azproduction/promise-queue.png?branch=master)](https://travis-ci.org/azproduction/promise-queue) [![Coverage Status](https://coveralls.io/repos/azproduction/promise-queue/badge.png?branch=master)](https://coveralls.io/r/azproduction/promise-queue) [![Dependency Status](https://gemnasium.com/azproduction/promise-queue.png)](https://gemnasium.com/azproduction/promise-queue)

Promise-based queue

## Installation

`promise-queue` can be installed using `npm`:

```
npm install promise-queue
```

## Example

### Configure queue

By default `Queue` tries to use global Promises, but you can specify your own promises.

```js
Queue.configure(require('vow').Promise);
```

Or use old-style promises approach:

```js
Queue.configure(function (handler) {
    var dfd = $.Deferred();
    try {
        handler(dfd.resolve, dfd.reject, dfd.notify);
    } catch (e) {
        dfd.reject(e);
    }
    return dfd.promise();
});
```

### Queue one by one example

```js
var maxConcurrent = 1;
var maxQueue = Infinity;
var queue = new Queue(maxConcurrent, maxQueue);

app.get('/version/:user/:repo', function (req, res, next) {
    queue.add(function () {
        // Assume that this action is a way too expensive
        // Call of this function will be delayed on second request
        return downloadTarballFromGithub(req.params);
    })
    .then(parseJson('package.json'))
    .then(function (package) {
        res.send(package.version);
    })
    .catch(next);
});
```

[Live example](http://jsfiddle.net/RVuEU/1/)
