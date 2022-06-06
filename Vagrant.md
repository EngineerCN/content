# What does Vagrant do

### *Problems:*
+ 怎么快速部署虚拟机
+ 多台虚拟机部署
+ 如何令部署代码化(IAC = Infrastructure As Code)

### *Solution:*

![vagrant drawio](https://user-images.githubusercontent.com/9009522/172183965-4dc5b438-8c1c-415b-80d1-3fb05446dfde.png)

### *Ref:*

https://www.vagrantup.com

https://app.vagrantup.com/boxes/search

# Vagrant Installation

### *Install Vagrant In Windows*

+ Install Vmware
+ Install Vagrant plugin for vmware
+ Vagrant Install Ubuntu


### *Install Vagrant In Linux*

+ Install Virtualbox
  ```
  sudo apt update
  sudo apt install virtualbox
  sudo apt install vagrant
  ```
+ Install Vagrant plugin for virtualbox
  ```
  sudo vagrant plugin install virtualbox
  ```
+ Vagrant Install CentOS
  ```
  sudo vagrant init centos/7
  sudo vagrant up --provider virtualbox
  ```

### *Install Vagrant In MacOS(M1)*

+ Install Qemu
+ Install Vagrant plugin for qemu

# Vagrant Cammand
# Vagrantfile
