# OpenSource Framework Modules

#### Start Modle & Pipeline Modle

![image](https://user-images.githubusercontent.com/9009522/172780013-b091d4d9-fd6c-40eb-a7c6-196201cafcd5.png)

# Simple Http Server Base on Nodejs
#### Index.js
```
const http = require("http")

const server = http.createServer((req,res)=>{
  console.log("new request")
  res.end("ok")
})

server.lishten(3000,()=>{
  console.log("Server start on port 3000......")
})
```
#### Docker run Index.js
```
docker run -itd -v $(pwd)/express:/express -p 3000:3000 node
```
# Express Feature
# 
