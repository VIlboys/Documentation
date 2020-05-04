#### 前言：

<font size="4">在现在的大部分的项目中使用的都是Maven依赖jar包的方式，主要也是因为将精力都放在业务上面，使用工具来帮助我们构建和管理项目的依赖，简单的说Maven 简化和标准化项目建设过程。处理编译，分配，文档，团队协作和其他任务的无缝连接。 Maven 增加可重用性并负责建立相关的任务。</font>

<font size="4">Maven的仓库地址：[地址连接](https://mvnrepository.com/?__cf_chl_captcha_tk__=7919fffd7cb05be2a8ec47e6a624dd12436bfd10-1585754651-0-AayAFo9r-QEE7By3AbwKAXAC0SYnqmlMb9O57uApQ1_7piqKb-Y2CSZGnMkakaKI1hBTmQGkbjunMxNpwFF9BzFhoxNGamJlkfMQuwY2QGCc9ADV0ZBha4aWFkQG7s8EJ9pbWydPMJMZsXeu5KLmv8-NW6lcEnVk0OPnEJMB4ahpwvpfU3J96gi252HkIz09iCMXJyPzmM3JBw_sdJgZTs1HTwWdEXseJt62W_s9mq0K7fiHUnpztKs6pN2QbTzkHNcS-8FtTHEMKqkMLvvnz4Qc5VzOcaVyxWQZm-rUYOhX_DtvKZ_2KaPDveFLp3N1ddiqx0gjlXBJsgUgNPKxoxIP7ql4_UBZzc4m3HMBZXwGKjZEfbggrIQNM27VF49OTg)   这里有很多jar包，在极少数情况下有可能会出现有些jar包你是无法下载依赖的</font>

例如：

下面这个jar包如果你是直接从Maven的仓库下载的话科学的来说是会下载失败的

```xml
<!-- https://mvnrepository.com/artifact/com.google.code/kaptcha -->
<dependency>
    <groupId>com.google.code</groupId>
    <artifactId>kaptcha</artifactId>
    <version>2.3.0</version>
</dependency>
```

#### 解决思路：

- 手机管理依赖，将依赖按传统的方式放入libs 目录中 缺点是需要手动管理依赖版本。
- 将依赖安装到本地仓库中，按照 Maven 下载依赖的优先级，会优先查找本地仓库中的依赖。缺点是协同开发时，其他开发人员会因为本地缺少依赖导致项目启动报错。

虽然这个解决的思路可以解决依赖缺少的问题，但操作起来不是那么的优雅，所以我们还是借助Maven提供的插件来解决这个问题

1，找到这个仓库的地址我们下载它的的jar包，下载这个留着有用

![01.png](https://i.loli.net/2020/04/02/43F5O2ACcDE67jr.png)

在管理Maven项目管理依赖的`pom.xml`文件中

管理这个依赖得版本号：

```xml
<pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-install-plugin</artifactId>
                    <version>2.5.2</version>
                </plugin>
            </plugins>
        </pluginManagement>
```



如果是Maven工程的话新建一个单独的项目：`my-shop-external` 新建一个`pom.xml`文件，这个项目管理外部的依赖

我们使用一个插件：

```xml
<build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-install-plugin</artifactId>
                <version>2.5.2</version>
                <executions>
                    <execution>
                        <id>install-external-kaptcha</id>
                        <!-- 触发时机：执行 mvn clean 命令时自动触发插件 -->
                        <phase>clean</phase>
                        <configuration>
                            <!-- 存放依赖文件的位置 -->
                            <file>${project.basedir}/libs/kaptcha-2.3.jar</file>
                            <repositoryLayout>default</repositoryLayout>
                            <!-- 自定义 groupId -->
                            <groupId>com.google.code.kaptcha</groupId>
                            <!-- 自定义 artifactId -->
                            <artifactId>kaptcha</artifactId>
                            <!-- 自定义版本号 -->
                            <version>2.3</version>
                            <!-- 打包方式 -->
                            <packaging>jar</packaging>
                            <!-- 是否自动生成 POM -->
                            <generatePom>true</generatePom>
                        </configuration>
                        <goals>
                            <goal>install-file</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

将刚刚下载好的jar包放入这个项目的`libs`目录下 这样我们的插件就会自动识别到。

然后使用命令：

```maven
mvc clean
```

即可把这个依赖安装到本地仓库里面了