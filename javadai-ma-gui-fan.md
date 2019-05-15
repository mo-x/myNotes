* 代码中的命名均不能以下划线或美元符号开始，也不能以下划线或美元符号结束。  

```
    反例:name / __name / $Object / name/ name​
```

* 代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。 说明:正确的英文拼写和语法可以让阅读者易于理解，避免歧义。注意，即使纯拼音命名方式 也要避免采用。正例:alibaba / taobao / youku / hangzhou 等国际通用的名称，可视同英文。 反例:DaZhePromotion \[打折\] / getPingfenByName\(\) \[评分\] / int 某变量 = 3

*  类名使用 UpperCamelCase 风格，必须遵从驼峰形式，但以下情形例外:DO / BO / DTO / VO / AO正例:MarcoPolo / UserDO / XmlService / TcpUdpDeal / TaPromotion 反例:macroPolo / UserDo / XMLService / TCPUDPDeal / TAPromotion

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

* 


