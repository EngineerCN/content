# Github actions
  *  github actions = gitlab ci = jenkin
  *  DevOps = CI(持续集成) + CD(持续发布)          // What is SRE? What is the different between SRE and DevOps?
  *  DevSecOps = CI + SEC + CD
  *  Security Tool = SAST, DAST, IAST, And RASP
  *  https://www.softwaretestinghelp.com/differences-between-sast-dast-iast-and-rasp/
      ## SAST
      * NodeJsScan.
      * SonarQube.
      * GitGuardian.
      * Snyk.
      * WhiteSource Bolt for GitHub.
      * OWASP ZAP.
      * Contrast Security - Community.
      * Sqreen.
      ## DAST
      * ZED Attack Proxy or ZAP. It is an open source tool which is offered by OWASP for performing security testing. ...
      * Nikto. ...
      * GoLismero.
      ## IAST
      * OpenRASP



# Publish to npm
  * Register a npm account
  * npm login that account
  * configure the package file
  * run npm publish
  
# Package
```
{
  "name": "@chankamlam/js-tmpl",
  "version": "1.0.8",
  "description": "A javascript template engine for frontend and backend",
  "main": "lib/index.js",             // entrypoint
  "directories": {
    "lib": "lib",
    "test": "test"
  },
  "scripts": {
    "test": "npx jest",
    "coverage": "npx jest --coverage",
    "pub": "npm publish --access public"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/chankamlam/js-tmpl.git"
  },
   "author": {
    "name": "chankamlam",
    "email": "chankamlam@icloud.com"
  },
  "keywords": [                    // important
    "js template",
    "javascript template",
    "template engine"
  ],
  "repository": {
    "type": "git",
    "url": "https://github.com/chankamlam/chinese-search"
  },
  "license": "MIT",                  // important
  "bugs": {
    "url": "https://github.com/chankamlam/js-tmpl/issues"
  },
  "homepage": "https://github.com/chankamlam/js-tmpl#readme",
  "devDependencies": {
    "express": "^4.17.2",
    "jest": "^27.4.5",
    "@chankamlam/js-tmpl": "^1.0.7"
  }
}
```

# How to write a template middleware of express
https://www.expressjs.com.cn/advanced/developing-template-engines.html
```
var fs = require('fs') // this engine requires the fs module
app.engine('ntl', function (filePath, options, callback) { // define the template engine
  fs.readFile(filePath, function (err, content) {
    if (err) return callback(err)
    // this is an extremely simple template engine
    var rendered = content.toString()
      .replace('#title#', '<title>' + options.title + '</title>')
      .replace('#message#', '<h1>' + options.message + '</h1>')
    return callback(null, rendered)
  })
})
app.set('views', './views') // specify the views directory
app.set('view engine', 'ntl') // register the template engine
```
# License
https://www.sohu.com/na/414543010_120781628

![3e95c3ad527a4cc7b4fd32422af59780](https://user-images.githubusercontent.com/9009522/147854103-c12a67df-765f-43c4-b883-291d84f5e6fe.png)
