# Build a p2p demo
以下是对UDP三种通信方式概念说明

单播，一对一的通信方式，一个客户端和一个服务端之间的消息通信
广播，一对多的通信方式，会将消息分发给整个局域网内的所有主机，
组播，一对多的通信方式，将网络上的主机进行逻辑上的分组，通信只会在同一个分组上面进行收发消息。

# 多播
```

const dgram = require('dgram')

const server = dgram.createSocket('udp4');
const port = 41234
const multicastAddr = '225.0.0.100';

const broadcast = (data) => {
    server.send("",port,multicastAddr);
};

const start = ()=>{
    server.on('close',()=>{
        console.log(`Server closed`);
    })

    server.on('error', (err) => {
      console.log(`server error:\n${err.stack}`);
      server.close();
    });

    server.on('message', (msg, rinfo) => {
        console.log(`server got: ${msg} from ${rinfo.address}:${rinfo.port}`);
    });

    server.on('listening', () => {
      server.setBroadcast(true);
      server.setTTL(255);
      server.addMembership(multicastAddr);
      const address = server.address();
  
      // 定时发消息
      setInterval(()=>{
        broadcast()
      },1000)
      
      console.log(`server listening ${address.address}:${address.port}`);
    });
    
    server.bind(port);

}
start()

```
