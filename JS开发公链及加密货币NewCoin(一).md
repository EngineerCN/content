# Archeture Diagram
* Block = 区块
* Chain = 链
* P2P = 对等网络/网形网络
* Common = 公共模块

 ![image](https://github.com/chankamlam/js-blockchain/blob/main/doc/architecture.drawio.png?raw=true)
 
 
# P2P = Peer to Peer
 ![image](https://steemitimages.com/1280x0/https://cdn.steemitimages.com/DQmZAFtWv5imtESqJdab7kZiCycT5FMKutoda2yvtBR6Ds2/peer-to-peer-network-1.png)
## Ref Paper
https://steemit.com/networking/@alikhan91/what-is-p2p-peer-to-peer-network
 
### UDP
https://blog.csdn.net/c_base_jin/article/details/97616341

以下是对UDP三种通信方式概念说明

* 单播，一对一的通信方式，一个客户端和一个服务端之间的消息通信

* 广播，一对多的通信方式，会将消息分发给整个局域网内的所有主机

* 组播，一对多的通信方式，将网络上的主机进行逻辑上的分组，通信只会在同一个分组上面进行收发消息

#### 多播
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

#### 广播
```

const dgram = require('dgram')

const server = dgram.createSocket('udp4');
const port = 41234
const multicastAddr = '225.255.255.255';

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

# BlockChain
* 区块链本质上就是一个链表
* 下区块存有上区块hash

 ## Block
 ![image](https://user-images.githubusercontent.com/9009522/147846833-24aed366-2a4e-4d9c-9986-b3bf0a043423.png)
 ## Chain
 ![image](https://user-images.githubusercontent.com/9009522/147846836-f9e96ccf-957e-4cbc-958c-2c0accbe6c9b.png)
