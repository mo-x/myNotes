\#\#\#\# 运行交互式命令

 - docker run -i -t ubuntu:15.10 /bin/bash

 -t:在新容器内指定一个伪终端或终端。

 -i:允许你对容器内的标准输入 \(STDIN\) 进行交互。



\#\#\#\# 后台模式运行

- docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"



\#\#\#\# 容器操作命令

- 查看运行中的容器 

  - docker ps

- 查看容器的标准输出

  - docker logs {CONTAINER ID \| NAMES}

- 停止容器

  - docker stop {CONTAINER ID \| NAMES}

- 启动容器

  - docker start {CONTAINER ID \| NAMES}

- 删除容器 \(注意容器是停止状态\)

  - docket rm {CONTAINER ID \| NAMES}

- 查看容器列表

  - docker images

- 搜索容器 

  - docker search 

- 进入容器

  - docker exec -it {CONTAINER ID \| NAMES} /bin/bash





\#\#\#\# 镜像操作命令

  -  拉取镜像 docker pull {name}

  -  运行镜像 docker run 

  -  镜像命名 docker tag {id} {name}

  - 更新命令 

   - docker commit -m="has update" -a="runoob" e218edb10161 runoob/ubuntu:v2

sha256:70bf1840fd7c0d2d8ef0a42a817eb29f854c1af8f7c59fc03ac7bdee9545aff8

    - -m:提交的描述信息

    - -a:指定镜像作者

    - e218edb10161：容器ID



\#\#\#\# 运行web应用

  - example

  1. 获取镜像 docker pull training/webapp

  2. 运行 docker run -d -P training/webapp python app.py

      - -d:让容器在后台运行。

      - -P:将容器内部使用的网络端口映射到我们使用的主机上。

  3. 指定端口运行 docker run -d -p 5000:5000 training/webapp python app.py

  4. 查看指定的容器端口映射 docker port {CONTAINER ID \| NAMES}

  5. 查看WEB应用日志 docker logs -f {CONTAINER ID \| NAMES}

  6. 查看WEB应用程序容器的进程 docker top {CONTAINER ID \| NAMES}

  7. 查看WEB应用底层信息 docker inspect {CONTAINER ID \| NAMES}

  

\#\#\#\# 安装官方mysql镜像

  - docker pull mysql

  - docker run -p 3307:3306 --name my-mysql2 -e MYSQL\_ROOT\_PASSWORD=123456 -d mysql:5.7

