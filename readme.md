#JSDev wrapper for node

[JSDev](https://github.com/douglascrockford/JSDev) is a JavaScript Development Tool written by Doglas Crockford <douglas@crockford.com>:

    JSDev is a filter that activates selected comments, making them executable.  This makes it possible to put development, performance, and testing scaffolding into a source file. The scaffolding is removed by minification, but is activated by JSDev.

You can refer to [his original project repository](https://github.com/douglascrockford/JSDev) to learn how to use it.

This module change the require() function behavior so that you could easily use JSDev in your node projects.

##Example
a.js:

    require('jsdev').modifyRequire()
    require('./b')
    /*log 'tag in script which require jsdev will not be opened'*/

b.js:

    var util = require('util')
    //jsdev(test) tag2 log:console.log
    //jsdev(test,production) tag1
    //jsdev tag1 log:util.log
    /*tag1 console.log('tag1 opened')*/
    /*tag2 console.log('tag2 opened')*/
    /*log 'function tag'*/

default(development) environment:

    $ node a.js
    tag1 opened
    10 Feb 13:23:53 - function tag

test environment:

    $ NODE_ENV=test node a.js 
    tag1 opened
    tag2 opened
    function tag

production environment:

    $ NODE_ENV=production node a.js 
    tag1 opened

##Usage

1. Add `jsdev@>=0.0.1` into devDependencies in the package.json file of your project.
2. `require('jsdev')`.
3. Any futher require()'d file will apply the rules specified in the file as a comment line start with `//jsdev`
3. Use `//jsdev(test,production) tag1` to turn on tag3 in test and production environment.  If you don't specify the environment, it will default to be development(`//jsdev tag2` turn tag2 on in only the development environment).
