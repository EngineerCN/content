# OpenSource Framework Modles

#### Start Modle & Pipeline Modle

![image](https://user-images.githubusercontent.com/9009522/172780013-b091d4d9-fd6c-40eb-a7c6-196201cafcd5.png)

# Simple Http Server Base on Nodejs
#### Nodejs Libarary

https://nodejs.org/dist/latest-v16.x/docs/api/http.html#httpcreateserveroptions-requestlistener

#### Index.js
```
const http = require("http")

const server = http.createServer((req,res)=>{
  console.log("new request")
  res.end("ok")
})

server.listen(3000,()=>{
  console.log("Server start on port 3000......")
})
```
#### Docker run Index.js
```
docker run -itd -v $(pwd)/express:/express -p 3000:3000 node
```
# Express Feature
#### app.use
```
const express = require('express')
const app = express()

const requestTime = function (req, res, next) {
  req.requestTime = Date.now()
  next()
}

app.use(requestTime)

app.get('/', (req, res) => {
  res.send(`TimeStamp = > ${req.requestTime} `)
})

app.listen(3000)
```

#### app.METHOD ( METHOD => get/post/put/delete/all )
```
const express = require('express')
const app = express()

app.post('/', (req, res) => {
  res.send('Got a POST request')
})
app.put('/user', (req, res) => {
  res.send('Got a PUT request')
})
app.delete('/user', (req, res) => {
  res.send('Got a DELETE request')
})
app.get('/', (req, res) => {
  res.send('Got a GET request')
})

app.listen(3000)
```
#### app.set
```
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

```
#### router
```
const express = require('express')
const app = express()

const users = express.Router()

users.get('/', function(req, res, next) {
  res.send('users.....')
})

const roles = express.Router()
roles.post('/',(req,res)=>{
  res.send('roles....')
})

app.use('/user',users)
app.use('/role',roles)

app.listen(3000)
```

#### Express Objects
+ Express
  + express.Router
  + express.static
+ Application
  + app.use
  + app.listen
  + app.set
+ Request
  + req.app
  + req.body
  + req.query
+ Response
  + res.app
  + res.end
  + res.json
+ Router
# Express V0.1
#### express.js
```
var express = {
	mws:[],
	idx:0
}
express.use = (fn)=>{
	express.mws.push(fn)
}
express.handle = ()=>{
	let idx = express.idx++
	let len = express.mws.length
	if(idx===len) return
	express.mws[idx](()=>{express.handle(idx)})
}
module.exports = express
```
#### index.js
```
const express = require('./express.js')
const fn1 = (next)=>{
	console.log('fn1 start...')
	next()
	console.log('fn1 end...')
}
const fn2 = (next)=>{
	console.log('fn2 start...')
	next()
	console.log('fn2 end...')
}
express.use(fn1)
express.use(fn2)
express.handle()
```
