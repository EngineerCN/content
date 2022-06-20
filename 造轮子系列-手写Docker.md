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


### fork/clone/exec区别

https://blog.csdn.net/wdjhzw/article/details/25614969

https://blog.csdn.net/jianchi88/article/details/6985326

https://blog.csdn.net/ljianhui/article/details/10089345

http://www.360doc.com/content/11/0502/11/6580811_113691501.shtml

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

### Docker V0.2 (Add UTS Namespace)
```
package main
import(
	"os"
	"fmt"
	"os/exec"
	"syscall"
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
	cmd.SysProcAttr = &syscall.SysProcAttr{
		Cloneflags: syscall.CLONE_NEWUTS,
	}
	cmd.Stdin = os.Stdin
	cmd.Stdout = os.Stdout
	cmd.Stderr = os.Stderr
	if err:=cmd.Run(); err!=nil{
		panic(err)
	}
}
```
#### Build in Container with err
```
panic: fork/exec /bin/sh: operation not permitted

goroutine 1 [running]:
main.run()
	/go/src/main.go:28 +0x15c
main.main()
	/go/src/main.go:13 +0xcc
```
#### Fix issues
```
docker run -itd --privileged golang
```
#### Ref
exec.Command: https://pkg.go.dev/os/exec@go1.18.3#Command

SysProcAttr: https://pkg.go.dev/syscall@go1.18.3#SysProcAttr

# Docker V0.3
### Use make && makefile
```
CMD=go
BIN_PATH=bin
SRC_PATH=src

all: clean build install

build: 
	$(CMD) build -o $(BIN_PATH)/docker $(SRC_PATH)/*

install:
	cp bin/docker /usr/bin/docker
	cp bin/docker /usr/local/bin/docker

uninstall:
	rm -rf /usr/bin/docker /usr/local/bin/docker
	rm -rf bin/docker

clean: uninstall
```
### Docker V0.3
```
package main
import(
	"os"
	"fmt"
	"os/exec"
	"syscall"
)
func main(){
	fmt.Printf("Process => %v [%d]\n",os.Args,os.Getpid())
	switch os.Args[1]{
		case "run":
			run()
		case "child":
			child()
		default:
			panic("have not defined.")
	}
}
func run(){
	cmd:=exec.Command(os.Args[0],append([]string{"child"},os.Args[2])...)
	cmd.SysProcAttr = &syscall.SysProcAttr{
		Cloneflags:syscall.CLONE_NEWUTS|syscall.CLONE_NEWPID,
	}
	cmd.Stdin = os.Stdin
	cmd.Stdout = os.Stdout
	cmd.Stderr = os.Stderr
	if err:=cmd.Run();err!=nil{
		panic(err)
	}
}
func child(){
	cmd:=exec.Command(os.Args[2])
	syscall.Sethostname([]byte("container"))
	cmd.Stdin = os.Stdin
	cmd.Stdout = os.Stdout
	cmd.Stderr = os.Stderr
	cmd.Run()
}
```


# Docker V0.4
### proc

/proc是一个虚拟文件系统,非真实文件而是开机后系统各项信息综合挂载，其中/proc/PID形式命名目录可以查看系统运行中各进程相关信息
| 标识（N为PID） | 用途 |
|:--:|:--:|
| /proc/N | pid为N的进程信息 |
|/proc/N/cmdline|进程启动命令|
|/proc/N/cwd|链接到进程当前工作目录|
|/proc/N/environ|进程环境变量列表|
|/proc/N/exe|链接到进程的执行命令文件|
|/proc/N/fd|包含进程相关的所有的文件描述符|
|/proc/N/maps|与进程相关的内存映射信息|
|/proc/N/mem|指代进程持有的内存，不可读|
|/proc/N/root|链接到进程的根目录|
|/proc/N/stat|进程的状态|
|/proc/N/statm|进程使用的内存的状态|
|/proc/N/status|进程状态信息，比stat/statm更具可读性|
|/proc/self|链接到当前正在运行的进程|
### Mount proc
top,ps 读取/proc信息
##### shell
```
mount -t proc proc /proc
```
##### go syscall.Mount
```
func Mount(source string, target string, fstype string, flags uintptr, data string) (err error)
```
##### mount flags
```
public enum MountFlags : ulong
{
    MS_RDONLY = 1,         // Mount read-only.
    MS_NOSUID = 2,         // Ignore suid and sgid bits.
    MS_NODEV = 4,         // Disallow access to device special files.
    MS_NOEXEC = 8,         // Disallow program execution.
    MS_SYNCHRONOUS = 16,    // Writes are synced at once.
    MS_REMOUNT = 32,    // Alter flags of a mounted FS.
    MS_MANDLOCK = 64,    // Allow mandatory locks on an FS.
    S_WRITE = 128,   // Write on file/directory/symlink.
    S_APPEND = 256,   // Append-only file.
    S_IMMUTABLE = 512,   // Immutable file.
    MS_NOATIME = 1024,  // Do not update access times.
    MS_NODIRATIME = 2048,  // Do not update directory access times.
    MS_BIND = 4096,  // Bind directory at different place.
}; // End Enum MountFlags : ulong
```
##### Ref 
https://zhuanlan.zhihu.com/p/36268333

### Docker V0.4(Add NEWPID Namespace)
```
package main
import(
	"os"
	"fmt"
	"os/exec"
	"syscall"
)
func main(){
	fmt.Printf("Process => %v [%d]\n",os.Args,os.Getpid())
	switch os.Args[1]{
		case "run":
			run()
		case "child":
			child()
		default:
			panic("have not defined.")
	}
}
func run(){
	cmd:=exec.Command(os.Args[0],append([]string{"child"},os.Args[2])...)
	cmd.SysProcAttr = &syscall.SysProcAttr{
		Cloneflags:syscall.CLONE_NEWUTS|syscall.CLONE_NEWPID,
	}
	cmd.Stdin = os.Stdin
	cmd.Stdout = os.Stdout
	cmd.Stderr = os.Stderr
	if err:=cmd.Run();err!=nil{
		panic(err)
	}
}
func child(){
	cmd:=exec.Command(os.Args[2])
	syscall.Sethostname([]byte("container"))
	// MS_NOEXEC：在本文件系统中不允许运行其他程序
	// MS_NOSUID: 在本系统中运行程序的时候，不允许set-user-id和set-group-id
	// MS_NODEV: 从Linux 2.4以来，所有mount的系统都会默认设定的参数
	defaultMountFlags := syscall.MS_NOEXEC | syscall.MS_NOSUID | syscall.MS_NODEV
	syscall.Mount("proc","/proc","proc",uintptr(defaultMountFlags),"")
	cmd.Stdin = os.Stdin
	cmd.Stdout = os.Stdout
	cmd.Stderr = os.Stderr
	if err:=cmd.Run();err!=nil{
		panic(err)
	}
	syscall.Unmount("/proc",0)
}
```
Need to fix issues
```
Error, do this: mount -t proc proc /proc
```


# Docker V0.5

```
package main
import(
	"os"
	"fmt"
	"os/exec"
	"syscall"
)
func main(){
	fmt.Printf("Process => %v [%d]\n",os.Args,os.Getpid())
	switch os.Args[1]{
		case "run":
			Run()
		case "init":
			Init()
		default:
			panic("have not defined.")
	}
}
func Run(){
	cmd:=exec.Command(os.Args[0],init,os.Args[2])
	cmd.SysProcAttr = &syscall.SysProcAttr{
		Cloneflags:syscall.CLONE_NEWUTS|syscall.CLONE_NEWPID,
	}
	cmd.Stdin = os.Stdin
	cmd.Stdout = os.Stdout
	cmd.Stderr = os.Stderr
	if err:=cmd.Run();err!=nil{
		panic(err)
	}
}
func Init(){
	syscall.Sethostname([]byte("container"))
	syscall.Mount("proc","/proc","proc",0,"")
	syscall.Exec(os.Args[2],os.Args[2:],os.Environ())
	syscall.Unmount("/proc",0)
}
```

