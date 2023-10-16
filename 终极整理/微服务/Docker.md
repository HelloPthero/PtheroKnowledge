

容器和vm虚拟机的区别：内核共享，操作系统阉割

容器技术是利用Linux的LXC(做资源隔离，资源管控)包装提供好用的接口方式



```
1.项目右键，添加docker支持,创建dockerfile文件
2.将项目通过xfpt复制到服务器
3.运行代码

构建镜像
docker build -t 镜像名称 -f
xxx/xxx/xxx/Dockerfile    ---文件地址
xxx/xxx/xxx                 ---上下文

镜像运行实例
docker run -itd -p 5177:80 --name 实例名称 镜像名称             --默认监听自己的80端口 主机的端口5177
```

```
docker ps -a 
```

192.168.17.2

17.128  --  254



安装docker

```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

运行

```
sudo systemctl start docker

// 拉取镜像
sudo docker pull hello-world
// 执行hello-world
sudo docker run hello-world
```

Linux命令

```
问题：permission denied
su root 切换用户 
sudo groupadd docker               #添加用户组
sudo gpasswd -a username docker    #将当前用户添加至用户组
newgrp docker                      #更新用户组

```

常用命令

```
搜索仓库镜像：docker search 镜像名
拉取镜像：docker pull 镜像名
查看正在运行的容器：docker ps
查看所有容器：docker ps -a
删除容器：docker rm container_id
查看镜像：docker images
删除镜像：docker rmi image_id
启动（停止的）容器：docker start 容器ID
停止容器：docker stop  容器ID
重启容器：docker restart 容器ID
启动（新）容器：docker run -it ubuntu /bin/bash
进入容器：docker attach 容器ID或docker exec -it 容器ID /bin/bash，推荐使用后者。
```

