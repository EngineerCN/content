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
### Linux Namespace
| Namespace类型 | 系统调用参数 | 内核版本 | 用途 |
|:--:|:--:|:--:|:--:|
| Mount Namespace | CLONE NEWNS | 2.4.19 |隔离进程看到挂载点视图|
| UTS Namespace|CLONE NEWUTS|2.6.19|隔离nodename和domainname|
| IPC Namespace|CLONE NEWIPC|2.6.19|隔离System V IPC 和 POSIX Message Queues|
| PID Namespace|CLONE NEWPID|2.6.24|隔离进程ID|
| Network Namespace|CLONE NEWNET|2.6.29|隔离网络设备，IP地址端口等网络栈|
| User Namespace|CLONE NEWUSER|3.8|隔离用户组ID|

Linux kernel Clone flags https://man7.org/linux/man-pages/man2/clone.2.html

### Shell cmd to create a process with own namespace
shell in linux
```
sudo unshare --fork --pid --mount-proc bash
```
run cmd in chirld & parent shell
```
sleep 1000 &
```
compare the /proc
```
ls -l /proc/<pid>/ns
```
### unshare --help
```
Usage:
 unshare [options] [<program> [<argument>...]]

Run a program with some namespaces unshared from the parent.

Options:
 -m, --mount[=<file>]      unshare mounts namespace
 -u, --uts[=<file>]        unshare UTS namespace (hostname etc)
 -i, --ipc[=<file>]        unshare System V IPC namespace
 -n, --net[=<file>]        unshare network namespace
 -p, --pid[=<file>]        unshare pid namespace
 -U, --user[=<file>]       unshare user namespace
 -C, --cgroup[=<file>]     unshare cgroup namespace

 -f, --fork                fork before launching <program>
 -r, --map-root-user       map current user to root (implies --user)

 --kill-child[=<signame>]  when dying, kill the forked child (implies --fork)
                             defaults to SIGKILL
 --mount-proc[=<dir>]      mount proc filesystem first (implies --mount)
 --propagation slave|shared|private|unchanged
                           modify mount propagation in mount namespace
 --setgroups allow|deny    control the setgroups syscall in user namespaces

 -R, --root=<dir>	    run the command with root directory set to <dir>
 -w, --wd=<dir>	    change working directory to <dir>
 -S, --setuid <uid>	    set uid in entered namespace
 -G, --setgid <gid>	    set gid in entered namespace

 -h, --help                display this help
 -V, --version             display version

For more details see unshare(1)
```

### Docker V0.2
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

