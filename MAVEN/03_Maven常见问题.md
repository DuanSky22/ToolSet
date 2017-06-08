[TOC]

# Maven常见问题

## 常见的Maven仓库

+ [Sonatype Nexus](http://repository.sonatype.org)
+ [Jarvana](http://www.jarvana.com/jarvana)
+ [MVNbrowser](http://mvnbrowser.com)
+ [MVNrepository](http://mvnrepository.com)


## 常见插件仓库

+ [Apache](http://maven.apache.org/plugins/index.html)
+ [Codehaus](http://repository.codehaus.org/org/codehaus/mojo/)


## 打可执行jar包

​        Maven默认打包生成的jar包是不能够直接运行的，因为带有main方法的类信息不会添加到manifest中。因此为了生成可执行的jar文件，需要借助其它Maven插件。这里我们使用**maven-shade-plugin**插件，添加如下配置即可：

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <version>1.2.1</version>
    <executions>
        <execution>
            <phase>package</phase>
            <goals>
                <goal>shade</goal>
            </goals>
            <configuration>
                <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>com.duansky.learn.mvn.HelloWorld</mainClass>
                    </transformer>
                </transformers>
            </configuration>
        </execution>
    </executions>
</plugin>
```

注：plugin在`<project> <build> <plugins>`下面。

## Maven常用命令

### 基础

+ `-clean`：清除输出目录target/
+ `-compile`：编译项目主代码，将编译结果输出至target/classes目录下
+ `-test`：执行测试代码
+ `-package`：将项目编译、测试之后打包（默认的打包类型时jar）到target/目录下
+ `-install`：安装项目到本地仓库中

### 依赖

+ `mvn dependency:list` 以列表的形式查看所有已经解析的依赖
+ `mvn dependency:tree`以树的形式查看所有已经解析的依赖
+ `mvn dependency:analyze`分析项目中的依赖以帮助用户对依赖进行优化。分析结果有两种：（a）Used undeclared dependencies 项目中使用到的但是没有直接声明的依赖。（b）Unused declared dependencies 项目中未使用的但是有声明的依赖。

### 插件

+ `mvn install -D maven.test.skip=true` 跳过所有的测试，直接安装到本地。
+ `mvn help:describe -D plugin=compiler`：查看compiler插件的相关描述。

### 裁剪反应堆

+ `-am,--also-make`：同时构建所列模块的依赖模块。
+ `-amd, --also-make-dependencies`：同时构建依赖于所列模块的模块
+ `-pl, --projects <arg>`：构建指定的模块，模块之间用逗号间隔
+ `-rf --resume-from <arg>`：从指定的模块回复反应堆