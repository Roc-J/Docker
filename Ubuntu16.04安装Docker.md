# Ubuntu16.04 安装Docker #

## Docker是什么 ##

Docker是一个开源的应用容器引擎，让开发者可以打包他们的应用已经依赖到一个可移植的容器中，然后发布到任何流行的Linux机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。

## 安装Docker ##

### Linux版本 ###
Docker需要使用Linux中内核的CGroups和Namespace功能，所以必须使用包含这两项功能的Linux内核。因此，Linux内核必须是高于3.10的64位版本，通过命令

    uname -r 

来查看当前系统的内核版本。

![](http://i.imgur.com/rkLC51n.png)

### 更新apt源 ###
apt是Ubuntu默认的包管理系统，apt在安装时会根据实际apt在安装时根据实际apt配置文件搜索安装源。一个系统可以包含多个不同的安装源，安装时apt会逐个进行搜索，Docker官方的apt仓库只包含Docker-engine的安装源，对于其依赖的包并不在内。因此，在设置Docker源前，需要针对国内的环境设置apt源。

简答的说，就是先更新apt

	sudo apt-get update

### 安装CA证书 ###
因为访问Docker用的是https协议

	sudo apt-get install apt-transport-https ca-certificates

![](http://i.imgur.com/YNttzmE.png)

### 添加GPC key ###
这是访问Docker源的公钥

	sudo apt-key adv \
               --keyserver hkp://ha.pool.sks-keyservers.net:80 \
               --recv-keys 58118E89F3A912897C070ADBF76221572C52609D

![](http://i.imgur.com/wcZG2iI.png)

### Ubuntu系统添加Docker源 ###

	echo deb https://apt.dockerproject.org/repo ubuntu-xenial main | sudo tee /etc/apt/sources.list.d/docker.list

![](http://i.imgur.com/rHYYggD.png)

### 再次更新apt包索引 ###

	sudo apt-get update

### 验证是否从正确的仓库拉取安装包 ###

	apt-cache policy docker-engine

如果有类似于下面的输出，则说明从正确的仓库获取包：

![](http://i.imgur.com/ov2wNuE.png)

### 安装Docker ###

	sudo apt-get install docker-engine

![](http://i.imgur.com/55ehRQq.png)

这个命令结束之后，Docker即安装成功。

### 配置 ###
Docker默认是只有root才能执行Docker命令，因此需要添加用户权限

创建docker用户组：

	sudo groupadd docker

添加当前用户到Docker用户组：

	sudo usermod -aG docker $USER

### 以root用户登录系统 ###

### 启动Docker Daemon ###

	service docker start

### 测试安装是否成功 ###

	docker version

![](http://i.imgur.com/x2hQoL9.png)

### 运行第一个程序 ###

	docker run hello-world

