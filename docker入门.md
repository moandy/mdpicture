# docker入门之用ubuntu16.04下载gcc编译helloworld

## docker简介

Docker 使用客户端-服务器 (C/S) 架构模式，使用远程API来管理和创建Docker容器，通过 Docker 镜像来创建，是一种轻量化的方式，与虚拟机相比，它没有硬件虚拟化层，其在内存访问，文件系统，网络速度上明显快的多

![](http://www.runoob.com/wp-content/uploads/2016/04/576507-docker1.png)

|       | 性能测试工具   | 主机   | Docker |
| ----- | -------- | ---- | ------ |
| CPU   | sysbench | 1    | 0.9945 |
| 写内存   | sysbench | 1    | 0.9826 |
| 读内存   | sysbench | 1    | 1.0025 |
| 磁盘I/O | sysbench | 1    | 0.9811 |
| 网络    | sysbench | 1    | 0.9626 |

## docker安装

- ### ubuntu16.04

```shell
$ sudo apt-get update
$ sudo apt-get install docker
$ sudo apt-get install docker.io
```

### CentOS 7

`$sudo yum install docker`



## docker基本命令

#### 使用search命令搜索镜像

`$ sudo docker search [镜像]`

#### 使用pull命令下载镜像

`$ sudo docker pull [镜像:版本]`

#### 使用images命令列出镜像目录

`$ sudo docker images`

#### 使用run命令创建容器

`$ sudo docker run -dit [镜像:版本] [命令]`

**![]()-d:** 表示后台运行

**-i:**允许你对容器内的标准输入 (STDIN) 进行交互。

**-t:**在新容器内指定一个伪终端或终端。

#### 使用ps命令查看容器列表

`$ sudo docker ps -a`

#### 使用stop命令终止容器

`$ sudo docker stop [容器名称]`



## Docker 编译c++文件

##### 编译环境:ubuntu:16.04

`$ sudo docker pull ubuntu:16.04`

由于笔者已经pull了ubuntu:16.04了，所以就不再展示pull的过程。

`$ sudo docker images`

![](H:\MyGitHub\mdpicture\1.png)

##### 文件目录

.
├── Dockerfile
└── helloworld.cpp

##### 撰写Dockerfile文件

由于笔者也是初次接触Dockerfile文件，只会一些基础的Dockerfile语法。

```dockerfile
FROM ubuntu:16.04      			#基于哪个镜像进行
COPY . .			  		    #复制文件 [复制文件在本地的路径] [文件在容器的路径]
WORKDIR .                       #用于设置RUN、CMD、ENTRYPOINT命令的目录
RUN apt-get update              #执行命令
RUN apt-get install g++ -y
expose 5000						#用于设置与主机相连的端口号
```

##### 创建镜像

`$ sudo docker image build -t mohk/helloworld .   #.表示当前目录` 

`$ sudo docker images`

![](H:\MyGitHub\mdpicture\2.png)

##### 运行容器

`$ sudo docker run -it mohk/helloworld `

`$ g++ helloworld.cpp -o helloworld -w -g `

`$ ./helloworld`

![](H:\MyGitHub\mdpicture\3.png)



到此，我们的hellworld基于ubuntu:16.04环境下已经执行完成。

其实，我们还可以将自己要执行的命令写进Dockerfile里面，这样可以省去自己很多时间，有点类似makefile，有兴趣的话可以自己去尝试一下。

还可以自己去[docker官网](https://hub.docker.com/)注册一个账号，把自己的容器上传上去到自己的主页(类似github的仓库)。

![](H:\MyGitHub\mdpicture\4.png)