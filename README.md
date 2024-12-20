

### docker常用命令

```
### 启动所有服务    docker-compose up -d 
### 启动一个服务    docker-compose up -d centos7.9
### 重启一个服务    docker-compose restart webapp
### 查看已启动容器的ip docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' centos7.9
### 重启一个服务    docker restart webapp
### 进入这个容器    docker exec -it centos7.9 /bin/bash
### 查看启动日志    docker logs centos7.9
### 关闭容器        docker stop centos7.9
### 再次原配置启动  docker start centos7.9
```

### 下载镜像
对于被墙的docker-hub等镜像仓库，可以用下面方式，把镜像拷贝到aliyun仓库。然后从aliyun再做docker pull。
https://github.com/sling2007/docker_image_pusher

### Minio集群

```
下载minio的客户端 http://dl.minio.org.cn/client/mc/release/windows-amd64/mc.exe 
下载源minio的策略配置 https://min.io/docs/minio/linux/examples/ReplicationAdminPolicy.json 到 ReplicationAdminPolicy.json
下载目的minio的策略配置 https://min.io/docs/minio/linux/examples/ReplicationRemoteUserPolicy.json 到 ReplicationRemoteUserPolicy.json

1、源minio
mc alias set minio-main http://10.101.59.170:9000 admin Marine.123
mc admin policy create minio-main ReplicationAdminPolicy ./ReplicationAdminPolicy.json
mc admin user add minio-main ReplicationAdmin LongRandomSecretKey
mc admin policy attach minio-main ReplicationAdminPolicy --user ReplicationAdmin

2、目的minio ，  注意不要用127.0.0.1，否则配置同步的时候，无法成功
mc alias set minio-slave http://10.101.59.170:9100 admin Marine.123
mc admin policy create minio-slave ReplicationRemoteUserPolicy ./ReplicationRemoteUserPolicy.json
mc admin user add minio-slave ReplicationRemoteUser LongRandomSecretKey
mc admin policy attach minio-slave ReplicationRemoteUserPolicy --user ReplicationRemoteUser

3、配置同步
mc replicate add minio-main/copytest01 --remote-bucket minio-slave/copytest01 --replicate "delete,delete-marker,existing-objects"
如果配置成功， 则会显示 “Replication configuration rule applied to minio-main/copytest01 successfully.”

4、测试
源minio中增删改文件，然后看目的minio中是否同步。

```


### zlmediakit 集群

```


```


