# Why Docker
+ 组件依赖与部署便利性
+ 资源使用成效
+ 应用可移植性

# Docker Architecture
#### 虚拟技术是系统颗粒度，容器技术是进程颗粒度。
![pasted image 0](https://user-images.githubusercontent.com/9009522/173328229-19585544-4e9c-4661-8fc5-ca08254eba81.png)
![576507-docker1](https://user-images.githubusercontent.com/9009522/173328283-bde7fc74-f731-4dd9-8372-2b16b88103de.png)

# Docker Technology
+ Namespace (隔离)
+ Cgroup (限制)
+ AUFS (联合文件系统)

# Docker V0.1
```
docker run --rm -it ubuntu /bin/bash
```
