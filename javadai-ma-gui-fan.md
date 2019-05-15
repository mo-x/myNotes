#### 命名风格

* 【强制】代码中的命名均不能以下划线或美元符号开始，也不能以下划线或美元符号结束。

```
反例: name / __name / $Object / name/ name​
```

* 【强制】代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。 说明:正确的英文拼写和语法可以让阅读者易于理解，避免歧义。注意，即使纯拼音命名方式 也要避免采用。正例:alibaba / taobao / youku / hangzhou 等国际通用的名称，可视同英文。 反例:DaZhePromotion \[打折\] / getPingfenByName\(\) \[评分\] / int 某变量 = 3

* 【强制】类名使用 UpperCamelCase 风格，必须遵从驼峰形式，但以下情形例外:DO / BO / DTO / VO / AO正例:MarcoPolo / UserDO / XmlService / TcpUdpDeal / TaPromotion 反例:macroPolo / UserDo / XMLService / TCPUDPDeal / TAPromotion

* 【强制】方法名、参数名、成员变量、局部变量都统一使用 lowerCamelCase 风格，必须遵从 驼峰形式。
  `正例: localValue / getHttpMessage() / inputUserId`

* 【强制】常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。 

```
    正例:MAX_STOCK_COUNT反例:MAX_COUNT
```

* 【强制】抽象类命名使用 Abstract 或 Base 开头;异常类命名使用 Exception 结尾;测试类 命名以它要测试的类的名称开始，以 Test 结尾。

* 【强制】中括号是数组类型的一部分，数组定义如下:String\[\] args; 反例:使用String args\[\]的方式来定义。

* 【强制】POJO 类中布尔类型的变量，都不要加 is，否则部分框架解析会引起序列化错误。 反例:定义为基本数据类型Boolean isDeleted;的属性，它的方法也是isDeleted\(\)，RPC

* 【强制】long或者Long初始赋值时，使用大写的L，不能是小写的l，小写容易跟数字1混 淆，造成误解。 说明:Long a= 2l;写的是数字的21，还是Long型的2?

* 【推荐】不要使用一个常量类维护所有常量，按常量功能进行归类，分开维护。说明:大而全的常量类，非得使用查找功能才能定位到修改的常量，不利于理解和维护。正例:缓存相关常量放在类CacheConsts下;系统配置相关常量放在类ConfigConsts下。



  


