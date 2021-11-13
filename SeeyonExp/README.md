# SeeyonExp

### 0x00 开发日志

2021-11-14 增加 maven打包时，程序入口方法；防止将用户id 打包进jar包造成信息泄露

使用自定义方法修改 例如：`Built-By: Apache Maven`

```
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

```
httpClient.doDel(getUrl,expDel);
change to
httpClient.doDel(realUrl,expDel);
```

2021-11-14 修改source警告，默认1.5

`/src/main/java/VersionRecognition.java`

```
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
