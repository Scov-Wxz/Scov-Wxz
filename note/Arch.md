# Arch

> in VMware Workstation

- [Arch](#arch)
  - [安装 \& NAT 配置静态 IP](#安装--nat-配置静态-ip)
  - [.bashrc](#bashrc)
  - [记录](#记录)
    - [vim](#vim)
    - [ssh](#ssh)
    - [docker](#docker)
    - [python](#python)
    - [java](#java)
      - [jdk17](#jdk17)
      - [Maven](#maven)
    - [rust](#rust)
    - [golang](#golang)
    - [node.js](#nodejs)

## 安装 & NAT 配置静态 IP

> VMware NAT 虚拟机, 设置 IP 地址为 192.168.8.8

Windows 10 (主机): 设置 -> 网络和 Internet -> 状态 -> 更改适配器选项 -> 右键 VMnet8 -> 属性 -> Internet 协议版本 4 -> 属性

> 使用下面的 IP 地址 & DNS 服务器地址

|   NAME   |       IP        |      INFO      |
| :------: | :-------------: | :------------: |
| IP 地址  |   192.168.8.1   |    主机 IP     |
| 子网掩码 |  255.255.255.0  |    子网掩码    |
| 默认网关 |   192.168.8.2   |   虚拟机网关   |
| 首选 DNS |     8.8.8.8     | 虚拟机首选 DNS |
| 备用 DNS | 114.114.114.114 | 虚拟机备用 DNS |

> VMware

VMware Workstation: 编辑 -> 虚拟网络编辑器 -> VMnet8 -> 更改设置 -> VMnet8 -> 子网 IP: 192.168.8.0 -> 勾选使用本地 DHCP 服务将 IP 地址分配给虚拟机 -> DHCP 设置 -> 起始 IP 地址: 192.168.8.8 -> 确定

进入 Arch Linux 后使用 archinstall 进行安装, 选择手动配置网络

## .bashrc

```bash
#
# ~/.bashrc
#

# If not running interactively, don't do anything
[[ $- != *i* ]] && return

alias ls='ls --color=auto'
alias grep='grep --color=auto'
PS1='[\u@\h \W]\$ '

# ---------- ADD ---------- #
alias la='ls -al'
alias py='python'
alias setproxy='export http_proxy=http://192.168.8.1:7897; export https_proxy=http://192.168.8.1:7897'
alias unsetproxy='unset http_proxy; unset https_proxy'

export JAVA_HOME=/usr/lib/jvm/java-17-openjdk
export PATH=$JAVA_HOME/bin:$PATH
# ---------- source ~/.bashrc ---------- #
```

## 记录

### vim

```shell
$ sudo pacman -S vim
```

### ssh

```shell
$ sudo pacman -S openssh
$ sudo systemctl enable sshd
$ sudo systemctl start sshd
$ sudo systemctl status sshd

# 允许以 root 连接 ssh
$ sudo vim /etc/ssh/sshd_config
# 找到行 #PermitRootLogin prohibit-password
[+] PermitRootLogin yes

$ sudo systemctl restart sshd
```

### docker

```shell
$ sudo pacman -S docker

# 当前用户添加到 docker 组 ; 重启
$ sudo usermod -aG docker $USER
$ sudo reboot

$ sudo systemctl enable docker
$ sudo systemctl start docker
$ sudo systemctl status docker

# 添加镜像源
$ sudo mkdir -p /etc/docker
$ sudo touch /etc/docker/daemon.json
$ echo '{
  "registry-mirrors": [
    "https://registry.docker-cn.com",
    "http://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn",
    "https://dockerhub.azk8s.cn",
    "https://mirror.ccs.tencentyun.com",
    "https://registry.cn-hangzhou.aliyuncs.com",
    "https://docker.mirrors.ustc.edu.cn",
    "https://docker.1panel.live",
    "https://atomhub.openatom.cn/",
    "https://hub.uuuadc.top",
    "https://docker.anyhub.us.kg",
    "https://dockerhub.jobcher.com",
    "https://dockerhub.icu",
    "https://docker.ckyl.me",
    "https://docker.awsl9527.cn"
  ],
  "live-restore": true
}' | sudo tee /etc/docker/daemon.json
$ sudo systemctl restart docker
$ docker info
```

### python

```shell
$ sudo pacman -S python python-pip
$ python --version
$ pip --version
```

### java

#### jdk17

```shell
$ sudo pacman -S jdk17-openjdk

$ readlink -f $(which java) | sed "s:bin/java::"
/usr/lib/jvm/java-17-openjdk/

# 修改 ~/.bashrc
[+] export JAVA_HOME=/usr/lib/jvm/java-17-openjdk
[+] export PATH=$JAVA_HOME/bin:$PATH

# 重新加载配置文件
$ source ~/.bashrc

# 若 VSCode Spring Initializr 失效, 修改 setting.json: "spring.initializr.serviceUrl": ["https://start.aliyun.com/","https://start.spring.io"]
```

#### Maven

```shell
$ sudo pacman -S maven
$ mvn -v
```

### rust

```shell
$ sudo pacman -S rustup
# 安装工具链
$ rustup default stable
$ rustc --version

# VSCode 中, 在 setting.json 中添加项目的 Cargo.toml 路径: "rust-analyzer.linkedProjects": ["xxx/Cargo.toml","xxx/Cargo.toml"]
```

### golang

```shell
$ sudo pacman -S go
$ go version
```

### node.js

```sh
$ sudo pacman -S nodejs npm
$ node -v
$ npm -v
```
