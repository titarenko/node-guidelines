# node-guidelines
My set of rules and best practices for writing systems on top of node.js stack.

## Errors

Always throw specific (custom) errors. Always catch specific errors.

For modern node.js versions:

```js
const { format } = require('util')

class MyError extends Error {
  constructor(myprop) {
    super(format('oh no! my error happened, here are some details: %s', myprop))
    this.myprop = myprop
  }
}
```

```js
myFunc().catch(MyError, error => console.error(error))

try {
  myFuncSync()
} catch (error) {
  if (error instanceof MyError) {
    console.error(error)
  } else {
    throw error
  }
}
```

For older node.js versions:

```js
var util = require('util')

function MyError (myprop) {
  Error.call(this, this.message = util.format('oh no! my error happened, here are some details: %s', myprop))
  Error.captureStackTrace(this, MyError)
  this.myprop = myprop
}

util.inherits(MyError, Error)
```
