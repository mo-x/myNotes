`1. jvm启动时添加参数`

`    #出现 OOME 时生成堆 dump: `

`    -XX:+HeapDumpOnOutOfMemoryError`

`    #生成堆文件地址：`

`    -XX:HeapDumpPath=/home/liuke/jvmlogs/`

`2. 发现程序异常前通过执行指令，直接生成当前JVM的dmp文件，6214是指JVM的进程号`

`    jmap -dump:format=b,file=serviceDump.dat 6214`

说明：由于第一种方式是一种事后方式，需要等待当前JVM出现问题后才能生成dmp文件，实时性不高，第二种方式在执行时，JVM是暂停服务的，所以对线上的运行会产生影响。所以建议第一种方式。

