# OpenSource Framework Models

#### Start Model & Pipeline Model

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
### Docker run Index.js
```
docker run -itd -v $(pwd)/express:/express -p 3000:3000 node
```
# Express Feature
### app.use
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

### app.METHOD ( METHOD => get/post/put/delete/all )
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
### app.set
```
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

```
### router
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
# Express Model
https://expressjs.com
### Model Analysis
```
const express = require('express')
const app = express()
const mw1 = (req,res,next)=>{
	console.log('mw1 start..')
	next()

	console.log('mw1 end..')
}

const mw2= (req,res,next)=>{
	console.log('mw2 start..')
	next()
	console.log('mw2 end..')
}
const mw3 = (req,res,next)=>{
	console.log('mw3 start..')
	next()
	console.log('mw3 end..')
}

const mw4= (req,res,next)=>{
	console.log('mw4 start..')
	next()
	console.log('mw4 end..')
}
app.use(mw1)
app.use(mw2)
app.get('/',(req,res,next)=>{
	console.log('start...')
	res.end('ok')
	console.log('end...')
})
const users = express.Router()
users.get('/',[mw3,mw4],(req,res)=>{
	console.log('start...(from user)')
	res.send('users...')
	console.log('end...(from user)')
	})
app.use('/user',users)
app.listen(3000)
```
### Onion Model
![express Diagram drawio](https://user-images.githubusercontent.com/9009522/173109990-87f54a00-25ad-4501-92fe-2abbbb247e25.png)


# Express Onion Model V0.1
### express.js
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
### index.js
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
const fn3 = (next)=>{
        console.log('fn3 start...')
	next()
	console.log('fn3 end...')
}
express.use(fn1)
express.use(fn2)
express.use(fn3)
express.handle()
```
# Express Onion Model V0.2
### express.js
```
var express = {
	mws:[],
}
express.use = (fn)=>{
	express.mws.push(fn)
}
express.handle = (fn)=>{
	let idx = 0
	let len = express.mws.length
	function next(){
		if(idx<len){
			express.mws[idx++]({},{},next)
		}else{
			fn()
		}
	}
	next()
}
module.exports = express
```
### index.js
```
const express = require('./express.js')
const fn1 = (req,res,next)=>{
	console.log('fn1 start...')
	next()
	console.log('fn1 end...')
}
const fn2 = (req,res,next)=>{
	console.log('fn2 start...')
	next()
	console.log('fn2 end...')
}
express.use(fn1)
express.use(fn2)
express.handle((req,res,next)=>{
	console.log('<EXECUTE>')
})

```
# Express Onion Model V0.3
### express.js
```
```
### index.js
```
```
### Express Objects
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
