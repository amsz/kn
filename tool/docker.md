# Docker

## 安装

[Docker for Mac](https://docs.docker.com/docker-for-mac/) 或 `brew cask install docker`

```shell
# 官方
curl -sSL https://get.docker.com/ | sh
# 或 DaoCloud
curl -sSL https://get.daocloud.io/docker | sh
```

#### 镜像加速

- [阿里云加速器](https://cr.console.aliyun.com/#/accelerator)
- [DaoCloud 加速器](https://www.daocloud.io/mirror#accelerator-doc)

都需要注册，以 DaoCloud 为例，进入 [Dashboard](https://dashboard.daocloud.io)，点击个人信息左侧的 🚀 获得 mirror 地址，添加到 Registry mirrors 中。

#### docker run

* `-i, --interactive` 交互式操作
* `-t, --tty`  终端
* `--rm` 退出后删除容器

#### docker exec

 进入镜像 `docker exec -it <id | name> bash` 

## 镜像

#### Jenkins

```shell
docker pull jenkins
docker run -d -p 49001:8080 -v $PWD/jenkins:/var/jenkins_home:z -t jenkins
```

Openresty

https://moonbingbing.gitbooks.io/openresty-best-practices

```shell
docker pull openresty/openresty:xenial
docker run -d -p 80:80 --net=host openresty
```



## 参考

https://yeasy.gitbooks.io/docker_practice