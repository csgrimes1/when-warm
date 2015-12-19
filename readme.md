###Introduction

This tool is intended to be used either in bash or a node.js project. It waits for a resource
(such as a database) to become available, then it starts another process. Let's illustrate with
an example. Say you have a Docker Compose environment that starts containers _S_ and _D_ (for _database_).
_S_ depends on _D_, but _D_ can be slow to start up. _S_ can use _when-warm_ in either of two ways.

1. The NodeJS startup script can be modified to use the _when-warm_ NPM package.
2. A startup bash script can utilize the _when-warm_ CLI.

###NodeJS Example

```javascript
const parser = require('./commandline-parser')
    , resolver = require('./waiter-resolver')
    , cmdLine = parser.parse(process.argv)
    , moduleName = resolver.resolve(cmdLine._.module)
    , waiterModule = require(moduleName)
    , moduleOptions = waiterModule.adaptCommandLine
        ? waiterModule.adaptCommandLine(cmdLine, process.argv)
        : cmdLine
    , promise = waiterModule.wait(moduleOptions)

promise.then(
    function(){
        process.exit(0)
    },
    function(e){
        console.error(e)
        process.exit(1)
    }
)
```

###CLI Example

