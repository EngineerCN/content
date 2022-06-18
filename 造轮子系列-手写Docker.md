# Why Docker
+ 组件依赖与部署便利性
+ 资源使用成效
+ 应用可移植性
#### _虚拟技术是系统颗粒度，容器技术是进程颗粒度。_

# Docker Architecture


![pasted image 0](https://user-images.githubusercontent.com/9009522/173328229-19585544-4e9c-4661-8fc5-ca08254eba81.png)

![image](https://user-images.githubusercontent.com/9009522/174225521-9ec38af5-3074-406a-a00f-7322ed5d252a.png)

![576507-docker1](https://user-images.githubusercontent.com/9009522/173328283-bde7fc74-f731-4dd9-8372-2b16b88103de.png)

# Container Technology
+ Namespace (隔离)
+ Cgroup (限制)
+ AUFS (联合文件系统)

# Docker V0.1

### Layer Diagram

![image](https://user-images.githubusercontent.com/9009522/174298433-8b73ffa2-a5a4-4d94-a7f3-9cb7dd043cae.png)

### Docker Command
```
docker run --rm -it ubuntu /bin/bash
```
### main.go
```
package main
import(
  "os"
  "fmt"
)
func main(){
  fmt.Println(os.Args)
}
```
```
import(
	"os"
	"fmt"
	"os/exec"
)

func main(){
	fmt.Println(os.Args)
	switch os.Args[1]{
		case "run":
			run()
		default:
			panic("have not define")
	}
}
func run(){
	cmd := exec.Command(os.Args[2])
	cmd.Stdin = os.Stdin
	cmd.Stdout = os.Stdout
	cmd.Stderr = os.Stderr
	cmd.Run()
}

```
# Docker V0.2

| Namespace类型 | 系统调用参数 | 内核版本 |
|:--:|:--:|:--:|
| Mount Namespace | CLONE NEWNS | 2.4.19 |
| UTS Namespace|CLONE NEWUTS|2.6.19|
| IPC Namespace|CLONE NEWIPC|2.6.19|
| PID Namespace|CLONE NEWPID|2.6.24|
| Network Namespace|CLONE NEWNET|2.6.29|
| User Namespace|CLONE NEWUSER|3.8|

Linux kernel Clone flags https://man7.org/linux/man-pages/man2/clone.2.html

