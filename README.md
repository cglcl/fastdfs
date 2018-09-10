#  fastdfs
使用docker-compose 创建fastdfs单机版服务(tarcker,storage,nginx)
## 使用
1. 安装docker和docker-compose  
2. 安装git    
3. clone项目    
 ```
 git clone https://qbanxiaoli@github.com/qbanxiaoli/fastdfs.git 
 ```    
4. 进入fastdfs目录  
```
 cd fastdfs
```   
5. 修改docker-compose.yml
```
version: '3.0'
services:
    fastdfs:
        build: .
        image: registry.cn-hangzhou.aliyuncs.com/qbanxiaoli/fastdfs
        # 该容器是否需要开机启动+自动重启。若需要，则取消注释。
        restart: always
        container_name: fastdfs
        environment:
            # nginx服务端口,默认80端口，可修改
            - WEB_PORT=80
            # tracker_server服务端口，默认22122端口，可修改
            - FDFS_PORT 22122
            # docker所在主机的IP地址，默认使用eth0网卡的地址
            - IP=123.207.85.155
        volumes:
            # 将本地目录映射到docker容器内的fastdfs数据存储目录，将fastdfs文件存储到主机上，以免每次重建docker容器，之前存储的文件就丢失了。
            - ${HOME}/var/local/fdfs:/var/local/fdfs
        # 使docker具有root权限以读写主机上的目录
        privileged: true
        # 网络模式为host，即直接使用主机的网络接口
        network_mode: "host"

```  
docker所在主机IP必须修改.
 
6. 执行docker-compose命令  
```
docker-compose up -d
```
7. 测试fastdfs是否搭建成功
```
docker exec -it fastdfs /bin/bash 
```
```
echo "Hello FastDFS!">index.html
```
```
fdfs_test /etc/fdfs/client.conf upload index.html
```      