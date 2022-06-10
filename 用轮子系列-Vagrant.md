# Why Vagrant

### *Problems:*
+ 怎么快速部署虚拟机
+ 多台虚拟机部署
+ 如何令部署代码化(IAC = Infrastructure As Code)

### *Solution:*

![image](https://user-images.githubusercontent.com/9009522/172375429-358df76e-3a33-4386-b536-b5d5edaf7f12.png)


### *Ref:*

https://www.vagrantup.com

https://app.vagrantup.com/boxes/search

# Vagrant Installation

### *Install Vagrant In Windows*

for winget, u need to install "App Installer" in App Store of Windows, after that u can run winget in "cmd" or "powershell"

+ Install Vmware
  ```
  winget install VMware Wokstation
  winget install vagrant
  ```
+ Install Vagrant plugin for vmware
  ```
  vagrant plugin install vagrant-vmware-desktop
  ```
+ Manual Install vagrant-vmware-utility

  https://www.vagrantup.com/vmware/downloads
  
+ Vagrant Install Ubuntu
  ```
  vagrant init generic/ubuntu2010
  vagrant up --provider vmware_desktop
  ```


### *Install Vagrant In Linux*
i use ubuntu here, if u use other linux system that u need to switch to different linux package mangament tools like "yum","dnf",etc...

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
for MacOS, we can use "Homebrew" or "Macports" for package management.

+ Install Qemu
  ```
  brew install qemu
  brew install vagrant
  ```
+ Config File Sharing of MacOS

+ Install Vagrant plugin for qemu
  ```
  vagrant plugin install vagrant-qemu 
  ```
# Vagrant Cammand
# Vagrantfile
# Create Your Own Box
