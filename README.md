### dockerfiles
```
1.1 有组件Mysql(mysql-server:5.7)
        Redis(redis:6.0.11)
        PHP(7.3)
        NGINX(nginx:stable-alpine)
        Node
1.2 创建 bridge网络
1.3 在项目代码中可以使用容器的 container_name 或者 docker-compose.yml 的 services name（such as db,web,redis,mifunfrontweb）。
 ```
 
 - MySQL
 ```
 1)images : mysql/mysql-server:5.7
 2)container_name : xbbmysql
 3)environment 环境变量
 4)command中是为初始化的参数，可以选择以 my.cnf的形式
 5)volumes 此处挂载的本地目录为 ./mysql , 容器内目录为 /var/lib/mysql
 6) expose port 3306,33060
 ```
 
 - Redis
 ```
 1)image: redis:6.0.11
 2)container_name: xbbredis
 3)command ：bash -c "chown redis /var/lib/redis && redis-server /etc/redis.conf" 启动命令
   其中/etc/redis.conf为挂载的配置文件
 4)sysctls 为修改的container中的系统参数
 5)volumes 中挂载了配置文件和数据目录
 ```
 
 - frontweb(xcdnmifunfrontweb,newmifunfrontweb)
 <br> yarndev:v02  build from [Dockerfile](./yarn/Dockerfile) 
 ```
 1)image: yarndev:v02 
 2)container_name: 
 3) volumes: 由于Dockerfile中 WORKDIR 目录为/home/web，所以需要把实际的代码目录挂载到 /home/web
 ```
 
 
 
 
 
