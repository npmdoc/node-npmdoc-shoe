# api documentation for  [shoe (v0.0.15)](https://github.com/substack/shoe)  [![npm package](https://img.shields.io/npm/v/npmdoc-shoe.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-shoe) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-shoe.svg)](https://travis-ci.org/npmdoc/node-npmdoc-shoe)
#### streaming sockjs for node and the browser

[![NPM](https://nodei.co/npm/shoe.png?downloads=true)](https://www.npmjs.com/package/shoe)

[![apidoc](https://npmdoc.github.io/node-npmdoc-shoe/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-shoe_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-shoe/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-shoe/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-shoe/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "James Halliday",
        "email": "mail@substack.net",
        "url": "http://substack.net"
    },
    "browserify": "browser.js",
    "bugs": {
        "url": "https://github.com/substack/shoe/issues"
    },
    "bundleDependencies": [
        "sockjs-client"
    ],
    "dependencies": {
        "sockjs": "0.3.7",
        "sockjs-client": "*"
    },
    "description": "streaming sockjs for node and the browser",
    "devDependencies": {
        "tape": "~1.0.4",
        "testling": "~1.4.1",
        "through": "~2.3.4"
    },
    "directories": {
        "example": "example"
    },
    "dist": {
        "shasum": "baed8f1a7f08f530b66f0914287fcaa65b12443a",
        "tarball": "https://registry.npmjs.org/shoe/-/shoe-0.0.15.tgz"
    },
    "engine": {
        "node": ">=0.6"
    },
    "homepage": "https://github.com/substack/shoe",
    "keywords": [
        "websocket",
        "stream",
        "sock",
        "browserify"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "substack",
            "email": "mail@substack.net"
        }
    ],
    "name": "shoe",
    "optionalDependencies": {},
    "readme": "shoe\n====\n\nStreaming sockjs for node and the browser.\n\nThis is a more streaming,\n[more unixy](http://www.faqs.org/docs/artu/ch01s06.html)\ntake on [sockjs](https://github.com/sockjs/sockjs-node).\n\n* works with [browserify](https://github.com/substack/node-browserify)\n([modularity](http://www.faqs.org/docs/artu/ch01s06.html#id2877537))\n* stream all the things\n([composition](http://www.faqs.org/docs/artu/ch01s06.html#id2877684))\n* emits a ''log'' event instead of spamming stdout\n([silence](http://www.faqs.org/docs/artu/ch01s06.html#id2878450))\n\n![shoe point javascript](http://substack.net/images/shoe.png)\n\nexample\n=======\n\nstream all the things\n---------------------\n\nBrowser code that takes in a stream of 0s and 1s from the server and inverts\nthem:\n\n''' js\nvar shoe = require('shoe');\nvar through = require('through');\n\nvar result = document.getElementById('result');\n\nvar stream = shoe('/invert');\nstream.pipe(through(function (msg) {\n    result.appendChild(document.createTextNode(msg));\n    this.queue(String(Number(msg)^1));\n})).pipe(stream);\n'''\n\nServer code that hosts some static files and emits 0s and 1s:\n\n''' js\nvar shoe = require('shoe');\nvar http = require('http');\n\nvar ecstatic = require('ecstatic')(__dirname + '/static');\n\nvar server = http.createServer(ecstatic);\nserver.listen(9999);\n\nvar sock = shoe(function (stream) {\n    var iv = setInterval(function () {\n        stream.write(Math.floor(Math.random() * 2));\n    }, 250);\n    \n    stream.on('end', function () {\n        clearInterval(iv);\n    });\n    \n    stream.pipe(process.stdout, { end : false });\n});\nsock.install(server, '/invert');\n'''\n\nThe server emits 0s and 1s to the browser, the browser inverts them and sends\nthem back, and the server dumps the binary digits to stdout.\n\nBy default, there's no logspam on stdout to clutter the output, which is a\nfrustrating trend in realtimey websocket libraries that violates the\n[rule of silence](http://www.faqs.org/docs/artu/ch01s06.html#id2878450).\n\nJust wait for a client to connect and you'll see:\n\n'''\n$ node server.js\n001011010101101000101110010000100\n'''\n\nwith dnode\n----------\n\nSince dnode has a simple streaming api it's very simple to plug into shoe.\n\nJust hack up some browser code:\n\n''' js\nvar shoe = require('shoe');\nvar dnode = require('dnode');\n\nvar result = document.getElementById('result');\nvar stream = shoe('/dnode');\n\nvar d = dnode();\nd.on('remote', function (remote) {\n    remote.transform('beep', function (s) {\n        result.textContent = 'beep => ' + s;\n    });\n});\nd.pipe(stream).pipe(d);\n'''\nand hack up a server piping shoe streams into dnode:\n\n''' js\nvar shoe = require('shoe');\nvar dnode = require('dnode');\n\nvar http = require('http');\nvar ecstatic = require('ecstatic')(__dirname + '/static');\n\nvar server = http.createServer(ecstatic);\nserver.listen(9999);\n\nvar sock = shoe(function (stream) {\n    var d = dnode({\n        transform : function (s, cb) {\n            var res = s.replace(/[aeiou]{2,}/, 'oo').toUpperCase();\n            cb(res);\n        }\n    });\n    d.pipe(stream).pipe(d);\n});\nsock.install(server, '/dnode');\n'''\n\nThen open up 'localhost:9999' in your browser and you should see:\n\n'''\nbeep => BOOP\n'''\n\nwith express or connect\n-----------------------\n\nyou must pass the return value of 'app.listen(port)'\n\n''' js\nvar shoe = require('shoe');\n\nvar express = require('express')\nvar app = express()\n\nvar sock = shoe(function (stream) { ... });\n\n// *** must pass expcess/connect apps like this:\nsock.install(app.listen(9999), '/dnode');\n'''\n\nwith reconnect\n--------------\n\nyou can use [reconnect](https://github.com/dominictarr/reconnect) just in case your sock ends or gets disconnected.\n\n''' js\nvar shoe = require('shoe');\nvar reconnect = require('reconnect');\nvar es = require('event-stream');\nvar result = document.getElementById('result');\n\nvar r = reconnect(function (stream) {\n\n  var s = es.mapSync(function (msg) {\n      result.appendChild(document.createTextNode(msg));\n      return String(Number(msg)^1);\n  });\n  s.pipe(stream).pipe(s);\n\n}).connect('/invert')\n\n'''\n\nbrowser methods\n===============\n\n''' js\nvar shoe = require('shoe')\n'''\n\nvar stream = shoe(uri, cb)\n--------------------------\n\nReturn a readable/writable stream from the sockjs path 'uri'.\n'uri' may be a full uri or just a path.\n\n'shoe' will emit a ''connect'' event when the connection is actually open,\n(just like in [net](http://nodejs.org/api/net.html#net_net_connect_options_connectionlistener)).\nwrites performed before the ''connect'' event will be buffered. passing in 'cb' to \nshoe is a shortcut for 'shoe(uri).on('connect', cb)'\n\nserver methods\n==============\n\n''' js\nvar shoe = require('shoe')\n'''\n\nAll the methods from the sockjs exports objects are attached onto the 'shoe'\nfunction, but the 'shoe()' function itself is special.\n\nvar sock = shoe(opts, cb)\n-------------------------\n\nCreate a server with 'sockjs.createServer(opts)' except this function also adds\nthe '.install()' function below.\n\nIf 'cb' is specified, it fires 'cb(stream)' on ''connection'' events.\n\nsock.install(server, opts)\n--------------------------\n\nCall 'sock.installHandler()' with the default option of spamming stdout with log\nmessages switched off in place of just emitting ''log'' messages\non the 'sock' object instead. This is a much less spammy default that gets out\nof your way.\n\nIf 'opts' is a string, use it as the 'opts.prefix'.\n\nserver events\n=============\n\nAll the messages that sockjs normally emits will be available on the 'sock'\nobject plus the events below:\n\nsock.on('log', function (severity, msg) { ... })\n------------------------------------------------\n\nUsing the default logger with 'sock.install()' will cause these ''log'' messages\nto be emitted instead of spamming stdout.\n\ninstall\n=======\n\nWith [npm](http://npmjs.org) do:\n\n'''\nnpm install shoe\n'''\n\nlicense\n=======\n\nMIT\n",
    "readmeFilename": "readme.markdown",
    "repository": {
        "type": "git",
        "url": "git://github.com/substack/shoe.git"
    },
    "scripts": {
        "test": "testling ."
    },
    "testling": {
        "files": "test/browser.js",
        "server": "test/server.js",
        "browsers": [
            "ie/8..latest",
            "chrome/latest",
            "firefox/latest",
            "safari/latest",
            "opera/latest",
            "iphone/latest",
            "ipad/latest",
            "android/latest"
        ]
    },
    "version": "0.0.15"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module shoe](#apidoc.module.shoe)
1.  [function <span class="apidocSignatureSpan">shoe.</span>createServer (options)](#apidoc.element.shoe.createServer)
1.  [function <span class="apidocSignatureSpan">shoe.</span>listen (http_server, options)](#apidoc.element.shoe.listen)



# <a name="apidoc.module.shoe"></a>[module shoe](#apidoc.module.shoe)

#### <a name="apidoc.element.shoe.createServer"></a>[function <span class="apidocSignatureSpan">shoe.</span>createServer (options)](#apidoc.element.shoe.createServer)
- description and source-code
```javascript
createServer = function (options) {
  return new Server(options);
}
```
- example usage
```shell
...
var sockjs = require('sockjs');

exports = module.exports = function (opts, cb) {
    if (typeof opts === 'function') {
cb = opts;
opts = {};
    }
    var server = sockjs.createServer();
    var handler = function (stream) {
var _didTimeout = stream._session.didTimeout
var _didClose = stream._session.didClose

stream._session.didTimeout = function () {
    cleanup()
    _didTimeout.apply(this, arguments)
...
```

#### <a name="apidoc.element.shoe.listen"></a>[function <span class="apidocSignatureSpan">shoe.</span>listen (http_server, options)](#apidoc.element.shoe.listen)
- description and source-code
```javascript
listen = function (http_server, options) {
  var srv;
  srv = exports.createServer(options);
  if (http_server) srv.installHandlers(http_server);
  return srv;
}
```
- example usage
```shell
...
''' js
var shoe = require('shoe');
var http = require('http');

var ecstatic = require('ecstatic')(__dirname + '/static');

var server = http.createServer(ecstatic);
server.listen(9999);

var sock = shoe(function (stream) {
var iv = setInterval(function () {
    stream.write(Math.floor(Math.random() * 2));
}, 250);

stream.on('end', function () {
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
