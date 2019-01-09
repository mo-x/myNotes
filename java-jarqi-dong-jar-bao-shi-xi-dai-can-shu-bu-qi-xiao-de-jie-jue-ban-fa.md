在测试springCloud例子的时候，将项目打成jar包，并通过

java -jar xxxxx.jar --spring.profiles.active=xxx

不能实现命令行控制

通过各种资料的查询，发现要将命令改成这样子：

java -Dspring.profiles.active=xxx -jar xxx.jar 

