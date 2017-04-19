# npmdoc-shoe

#### api documentation for  [shoe (v0.0.15)](https://github.com/substack/shoe)  [![npm package](https://img.shields.io/npm/v/npmdoc-shoe.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-shoe) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-shoe.svg)](https://travis-ci.org/npmdoc/node-npmdoc-shoe)

#### streaming sockjs for node and the browser

[![NPM](https://nodei.co/npm/shoe.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/shoe)

- [https://npmdoc.github.io/node-npmdoc-shoe/build/apidoc.html](https://npmdoc.github.io/node-npmdoc-shoe/build/apidoc.html)

[![apidoc](https://npmdoc.github.io/node-npmdoc-shoe/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-shoe/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-shoe/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-shoe/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "James Halliday",
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
            "name": "substack"
        }
    ],
    "name": "shoe",
    "optionalDependencies": {},
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



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
