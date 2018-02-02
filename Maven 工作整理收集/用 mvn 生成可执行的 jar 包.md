用 mvn 生成可执行的 jar 包
==========================
>mvn 的 pom 文件加入以下内容：
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>1.3.3.RELEASE</version>
            <executions>
                <execution>
                    <goals>
                        <goal>repackage</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
    <finalName>demo</finalName>
</build>
```
>打出来的 jar 包可执行文件，是可以启动执行的。

>以下命令是执行打好包的 jar 文件：
```text
java -jar demo.jar
```

>实际上用 Springboot 所生成出来的项目是不需要部署到 Tomcat 里面去的，而是反过来 Tomcat 内嵌在打出来的可执行的 jar 包里面。
