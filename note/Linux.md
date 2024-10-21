# Linux

- [Linux](#linux)
	- [Settings](#settings)
		- [VMware NAT 虚拟机走主机代理](#vmware-nat-虚拟机走主机代理)
	- [Tools](#tools)
		- [Git](#git)
		- [Socket Statistics](#socket-statistics)

## Settings

### VMware NAT 虚拟机走主机代理

主机 clash 开启允许局域网连接, 代理端口为 7897

```shell
# .bashrc

alias setproxy='export http_proxy=http://192.168.8.1:7897; export https_proxy=http://192.168.8.1:7897'
alias unsetproxy='unset http_proxy; unset https_proxy'

# 查看是否设置成功
$ setproxy
$ echo $http_proxy
$ echo $https_proxy
$ unsetproxy
```

## Tools

### Git

> git settings

```sh
$ git config --global user.name "Scov-Wxz"
$ git config --global user.email "3874005664@qq.com"
git config --list

# git 走代理 192.168.8.1:7897
$ git config --global http.proxy "http://192.168.8.1:7897"
$ git config --global https.proxy "http://192.168.8.1:7897"
```

<!-- git 走主机代理 pull&push

```.gitconfig
[user]
	name = Scov-Wxz
	email = 3874005664@qq.com
[http]
	sslVerify = false

[http]
	proxy = http://192.168.8.1:7897
[https]
	proxy = http://192.168.8.1:7897
``` -->

> .gitconfig

```.gitconfig
[user]
	name = Scov-Wxz
	email = 3874005664@qq.com
[http]
	proxy = http://192.168.8.1:7897
[https]
	proxy = http://192.168.8.1:7897
```

### Socket Statistics

> `ss`

- l : Listening
- p : Processes
- t : TCP
- u : UDP
