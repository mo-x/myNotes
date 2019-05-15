* 代码中的命名均不能以下划线或美元符号开始，也不能以下划线或美元符号结束。  

```
    反例:name / __name / $Object / name/ name​
```

* 代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。 说明:正确的英文拼写和语法可以让阅读者易于理解，避免歧义。注意，即使纯拼音命名方式 也要避免采用。正例:alibaba / taobao / youku / hangzhou 等国际通用的名称，可视同英文。 反例:DaZhePromotion \[打折\] / getPingfenByName\(\) \[评分\] / int 某变量 = 3

* 类名使用 UpperCamelCase 风格，必须遵从驼峰形式，但以下情形例外:DO / BO / DTO / VO / AO正例:MarcoPolo / UserDO / XmlService / TcpUdpDeal / TaPromotion 反例:macroPolo / UserDo / XMLService / TCPUDPDeal / TAPromotion

* 方法名、参数名、成员变量、局部变量都统一使用 lowerCamelCase 风格，必须遵从 驼峰形式。

```
    正例: localValue / getHttpMessage() / inputUserId
```

* 常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。

  ```
  正例:MAXSTOCKCOUNT反例:MAX_COUNT
  ```

* 抽象类命名使用 Abstract 或 Base 开头;异常类命名使用 Exception 结尾;测试类 命名以它要测试的类的名称开始，以 Test 结尾。

* 包名统一使用小写，点分隔符之间有且仅有一个自然语义的英语单词。包名统一使用单数形式，但是类名如果有复数含义，类名可以使用复数形式。  
  `正例:应用工具类包名为com.alibaba.open.util、类名为MessageUtils(此规则参考spring的框架结构)`

* 接口类中的方法和属性不要加任何修饰符号\(public也不要加\)，保持代码的简洁 性，并加上有效的Javadoc注释。尽量不要在接口里定义变量，如果一定要定义变量，肯定是 与接口方法相关，并且是整个应用的基础常量。  
  正例:接口方法签名:void f\(\);

  `接口基础常量表示:String COMPANY= "alibaba";          
  反例:接口方法定义:public abstractvoid f();`  
  说明:JDK8中接口允许有默认实现，那么这个default方法，是对所有实现类都有价值的默 认实现。

#### OOP规约

* **避免通过一个类的对象引用访问此类的静态变量或静态方法，无谓增加编译器解析成 本，直接用类名来访问即可。**

* 所有的覆写方法，必须加@Override注解。  
  说明:getObject\(\)与get0bject\(\)的问题。一个是字母的O，一个是数字的0，加@Override可以准确判断是否覆盖成功。另外，如果在抽象类中对方法签名进行修改，其实现类会马上编 译报错。

* 不能使用过时的类或方法。

* 相同参数类型，相同业务含义，才可以使用Java的可变参数，避免使用Object。说明:可变参数必须放置在参数列表的最后。\(提倡同学们尽量不用可变参数编程\)正例:public User getUsers\(String type, Integer... ids\) {...}

* 方法的参数建议不超过5个

* 使用lombok插件保持代码的简洁

* Object的equals方法容易抛空指针异常，应使用常量或确定有值的对象来调用equals。

  `正例:"test".equals(object);        
   反例:object.equals("test");`

* **所有的相同类型的包装类对象之间值的比较，全部使用equals方法比较**。说明:对于Integer var= ?在-128至127范围内的赋值，Integer对象是在IntegerCache.cache产生，会复用已有对象，这个区间内的Integer值可以直接使用==进行判断，但是这个区间之外的所有数据，都会在堆上产生，并不会复用已有对象，这是一个大坑， 推荐使用equals方法进行判断

* 关于基本数据类型与包装数据类型的使用标准如下:  
  1\)【强制】所有的POJO类属性必须使用包装数据类型。  
  2\)【推荐】所有的局部变量使用基本数据类型。

* 构造方法里面禁止加入任何业务逻辑，如果有初始化逻辑，请放在init方法中。

* 实体类写toString方法。可以使用IDE的中工具:source&gt;generate toString时或者使用lombok @ToString注解，如果继承了另一个实体类，注意在前面加一下super.toString。说明:在方法执行抛出异常时，可以直接调用POJO的toString\(\)方法打印其属性值，便于排查问题。



