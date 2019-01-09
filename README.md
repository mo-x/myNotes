### 记一次Spring boot项目乱码排查

#### 问题:服务端校验用户输入的密码不能为中文 但是客户端输入中文是可以在测试环境通过正则表达式的

1. First 首先本地环境是没有问题的 确认为测试环境有问题

2. 添加监控代码 打印日志获取参数的编码

   \`\`\`String decodePassword = new String\(mebPasswordModifyReq.getNewPassword\(\).getBytes\("utf-8"\), "utf-8"\);

   String decodePassword1 = new String\(mebPasswordModifyReq.getNewPassword\(\).getBytes\(\), "utf-8"\);

   logger.error\("decodePassword1=" + decodePassword1\);

   String decodePassword2 = new String\(mebPasswordModifyReq.getNewPassword\(\).getBytes\("ISO-8859-1"\), "utf-8"\);

   \`\`\`

   发现都是乱码 说明该字符串无法转换为utf-8,担心是logger 日志底层有做处理\(然而并没有\) 改为控制台打印

   \`System.out.println\("console 输出新密码为:" + mebPasswordModifyReq.getNewPassword\(\)\);\`

   发现还是乱码 暴躁+1

3. 抓包请求发现 \*\*content-type application/json;charSet=utf8\*\*

4. 编写方法获取字符串的代码

   \`\`\`

   String encode\[\] = new String\[\]{"UTF-8","ISO-8859-1","GB2312","GBK","GB18030","Big5",

   ```
            "Unicode",

            "ASCII"

    };

    for \(int i = 0; i &lt; encode.length; i++\) {

        try {

            if \(str.equals\(new String\(str.getBytes\(encode\[i\]\), encode\[i\]\)\)\) {

                return encode\[i\];

            }

        } catch \(Exception ex\) {

        }

    }

    return "";
   ```

   \`\`\`

   监控日志打印出来为utf-8 确认为项目环境问题

5. 查看pom文件 添加以下代码

   \`\`\`

   &lt;project.build.sourceEncoding&gt;UTF-8&lt;/project.build.sourceEncoding&gt;

   &lt;project.reporting.outputEncoding&gt;UTF-8&lt;/project.reporting.outputEncoding&gt;

   &lt;java.version&gt;1.8&lt;/java.version&gt;

   &lt;spring-cloud.version&gt;Edgware.RELEASE&lt;/spring-cloud.version&gt;

   &lt;maven.compiler.encoding&gt;UTF-8&lt;/maven.compiler.encoding&gt;

   \`\`\`

   再次部署项目 继续乱码 暴躁+2

6. 添加代码 获取当前系统的字符集编码

   \` logger.error\("file.encoding is " + System.getProperty\("file.encoding"\)\);\`

   惊喜出现了 监控日志发现

   进入容器 输入\`echo $LANG\` 发现为空

   go on

   敲入\`jinfo -flags process\_id\` 输出为

   \`\`\`

   -XX:CICompilerCount=4 -XX:InitialHeapSize=262144000 -XX:MaxHeapSize=4164943872 -XX:MaxNewSize=1388314624 -XX:MinHeapDeltaBytes=524288 -XX:NewSize=87031808 -XX:OldSize=175112192 -XX:+UseCompressedClassPointers -XX:+UseCompressedOops -XX:+UseParallelGC

   Command line:  -Djava.security.egd=file:/dev/./urandom  -Djava.util.concurrent.ForkJoinPool.common.parallelism=2000

   \`\`\`

   排查发现未指定 \*\*file.encoding\*\*

7. 敲入 \`jinfo -sysprops process\_id\` 查看java 系统各项参数

   发现\`file.encoding = ANSI\_X3.4-1968\`

   至此排查完毕 确认为乱码的问题是由于未指定java系统正确的编码引起的

解决办法：\*\*修改dockerfile文件脚本 添加-Dfile.encoding=utf-8 指定起字符集监控日志发现 中文恢复正常显示\*\*

\*\*总结：乱码问题的定位较复杂 需排查系统环境 项目编码 请求编码 启动脚本 jvm系统参数等 乱码的本质就是编码与解码采用的编码方式不一样\*\*

延伸阅读：

```
java系统参数sun.jnu.encoding为文件名的字符编码集
```



