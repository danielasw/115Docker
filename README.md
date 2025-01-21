## 威联通小白安装115docker
0编程基础，全靠开发者的issues和ai给我debug，历时两天终于跑起来了，留个记录，也给其他没有基础的兄弟们一点帮助

**前置知识：putty软件。SSH必备，拉取镜像，运行docker必须使用。**

开发者默认拉取
```shell
docker pull xiuxiu10201/115
```

无法拉取镜像的问题，可以采取自建镜像库的方式曲线救国

[1.自建镜像库](https://github.com/tech-shrimp/docker_image_pusher)
[2.威联通拉取方式](https://blog.csdn.net/weixin_42763614/article/details/135949990)


拉取完镜像后部署docker

在作者的原指令基础上把cookies需要的部分全部加上了，文字部分替换成自己的，即可实现一键运行

```shell
docker run --name=115 \
--user 0:0 \
--env PASSWORD=123456 \
--env DISPLAY_WIDTH=1920 \
--env DISPLAY_HEIGHT=1080 \
--env CID=你的 \
--env SEID=你的 \
--env UID=你的 \
--env KID=你的 \
-v /share/你的数据目录:/etc/115 \
-v /share/你的下载目录:/opt/Downloads \
--rm --network=host -d --tmpfs /tmp \
--label io.containers.autoupdate=registry \
--env TZ=Asia/Shanghai \
registry.cn-hangzhou.aliyuncs.com/da你的仓库名/115
```


注意最后这一句
```shell
registry.cn-hangzhou.aliyuncs.com/da你的仓库名/115
```
这一句就是自建镜像库的仓库名，如果前面可以成功拉取开发者的镜像，则可以替换成下面这句
```shell
docker.io/xiuxiu10201/115:latest
```


运行成功后进入http://192.168.31.156:1150/vnc.html

密码123456，需要自定义则在代码第三行修改

## 常见问题：

**1.cookie**

CID/SEID/UID/KID在浏览器登陆115后F12进入开发者工具提取

**2.主机地址**

-v /share 这两行一定要找清楚，威联通的文件目录略有些复杂，nas的目录写错了会导致下载的文件看不到

如果不知道具体的目录，则需要通过putty使用df -h查找卷，然后find检验具体的文件夹地址，具体方法自行检索/AI


纯小白的研究之旅，可能有些误打误撞，总之是跑出来了暂时没问题，仅供参考

感谢开发者~
 
 
 
 
  
 
 
 

##  

**以下是作者dream10201原文**

# 115浏览器Docker版
用于无头环境Linux上传和下载文件。
![previews](https://github.com/dream10201/115Docker/blob/master/image.png)
## 拉取镜像
```bash
docker pull docker.io/xiuxiu10201/115:latest
// or
docker pull ghcr.io/dream10201/115docker:latest
```
## 运行命令
```shell
docker run --name=115 \
--user 0:0
--env PASSWORD=123456 \
--env DISPLAY_WIDTH=1920 \
--env DISPLAY_HEIGHT=1080 \
--rm --network=host -d \
--env TZ=Asia/Shanghai \
docker.io/xiuxiu10201/115:latest
```

## Web
[http://localhost:1150/vnc.html](http://localhost:1150/vnc.html)

## 环境变量

| 名称 | 描述 | 必须|
|:---------:|:---------:|:---------:|
|PASSWORD|VNC密码|Y|
|CID|Cookie|N|
|SEID|Cookie|N|
|UID|Cookie|N|
|KID|Cookie|N|
|DISPLAY_WIDTH|窗口宽度|N|
|DISPLAY_HEIGHT|窗口高度|N|

## 挂载目录

| 路径 | 描述 | 必须|
|:---------:|:---------:|:---------:|
|/etc/115|115浏览器数据目录|N|
|/opt/Downloads|下载目录|N|

## 端口占用
| 端口 | 描述 | 必须|
|:---------:|:---------:|:---------:|
|1150|WEB端口|Y|
|1152|VNC端口|N|

