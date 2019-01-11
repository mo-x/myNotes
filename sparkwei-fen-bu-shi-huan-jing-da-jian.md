* 首先下载spark包 解压缩
  * 包目录介绍
    * drwxrwxr-x 2 1001 1001 4096 Nov 24 2017 bin	-&gt;运行脚本目录
      drwxrwxr-x 2 1001 1001 4096 Nov 24 2017 conf	-&gt;配置文件
      drwxrwxr-x 5 1001 1001 4096 Nov 24 2017 data	-&gt;数据，自带例子的数据
      drwxrwxr-x 4 1001 1001 4096 Nov 24 2017 examples	-&gt;Spark自带的一些例子的源码
      drwxrwxr-x 2 1001 1001 12288 Nov 24 2017 jars	-&gt;jar包，第三方的的jar文件
      -rw-rw-r-- 1 1001 1001 17881 Nov 24 2017 LICENSE	
      drwxrwxr-x 2 1001 1001 4096 Nov 24 2017 licenses	
      -rw-rw-r-- 1 1001 1001 24645 Nov 24 2017 NOTICE	
      drwxrwxr-x 8 1001 1001 4096 Nov 24 2017 python	-&gt;对python语言支持的适配
      drwxrwxr-x 3 1001 1001 4096 Nov 24 2017 R	-&gt;对R语言支持的适配
      -rw-rw-r-- 1 1001 1001 3809 Nov 24 2017 README.md
      -rw-rw-r-- 1 1001 1001 128 Nov 24 2017 RELEASE
      drwxrwxr-x 2 1001 1001 4096 Nov 24 2017 sbin	-&gt;集群启停，因为spark有自带的集群环境
      drwxrwxr-x 2 1001 1001 4096 Nov 24 2017 yarn	-&gt;对yarn支持的一些安装包
			
  * 修改配置
    * 进入conf目录 目录结构如下

    ![](/assets/Snipaste_2019-01-11_15-52-40.png)

    * copy spark-env.sh.template文件 修改后缀为.sh 添加如下内容
      ```
      export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_162.jdk/Contents/Home
      export SPARK_MASTER_IP=127.0.0.1
      export SPARK_WORKER_MEMORY=512m
      export master=spark://127.0.0.1:7070
      ```
    * 修改slaves文件内容

    ```
        127.0.0.1
    ```

* ./sbin/start-all.sh 启动  
* console 输入jps 查看java进程 ![](/assets/Snipaste_2019-01-11_15-58-41.png)



