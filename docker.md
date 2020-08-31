# docker

## 安装docker
```

sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

```
```
sudo yum install -y yum-utils \
>   device-mapper-persistent-data \
>   lvm2

```
```
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

sudo yum install docker-ce docker-ce-cli containerd.io


```

## docker 安装mysql-server

```
sudo docker pull mysql:5.6.48

```

## 本机安装mysql-client/mysql-connector-c++
```
[sxy@localhost cpp]$ sudo yum install mysql-connector-c++-devel.x86_64

```


##
