---
title: VS Code配置Spring Boot
categories:
  - 环境搭建 - VS Code
tags:
  - VS Code
  - Spring Boot
  - 配置
cover: /images/cover/springboot.jpg
abbrlink: 49760de
date: 2020-02-27 13:02:57
---


# 环境准备

- jdk 13.0.2
- maven 3.6.3
- SpringBoot 1.5.9
- VS Code

## JDK

[官方网站](https://www.oracle.com/java/technologies/javase-downloads.html)下载JDK并安装。

右键计算机 - &gt  属性 - &gt  高级系统设置 - &gt  高级 - &gt  环境变量，设置`Path` 和 `JAVA_HOME` 环境变量：

![path](/images/1-Spring-Boot入门/2020-02-26-16-45-47.png)


![java_home](/images/1-Spring-Boot入门/2020-02-26-16-47-54.png)

然后在命令行输入`java --version`检测是否成功：

```bash
java 13.0.2 2020-01-14
Java(TM) SE Runtime Environment (build 13.0.2+8)
Java HotSpot(TM) 64-Bit Server VM (build 13.0.2+8, mixed mode, sharing)
```
## Maven

[官方网站](https://maven.apache.org/download.cgi)下载maven并解压。

右键计算机 - &gt  属性 - &gt  高级系统设置 - &gt  高级 - &gt  环境变量，设置`Path` 和 `MAVEN_HOME` 环境变量：

![Path](/images/1-Spring-Boot入门/2020-02-26-16-59-00.png)

![MAVEN_HOME](/images/1-Spring-Boot入门/2020-02-26-17-01-26.png)

然后在命令行输入`mvn -v`检测是否成功：

```bash
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: C:\Program Files\apache-maven-3.6.3\bin\..
Java version: 13.0.2, vendor: Oracle Corporation, runtime: C:\Program Files\Java\jdk-13.0.2
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
```

创建仓库文件夹`C:\Program Files\apache-maven-3.6.3\maven-repository`并在`C:\Program Files\apache-maven-3.6.3\conf\settings.xml`中添加仓库地址：

```html
 &lt localRepository &gt C:\Program Files\apache-maven-3.6.3\maven-repository &lt /localRepository &gt 
```

![仓库地址](/images/1-Spring-Boot入门/2020-02-26-17-14-04.png)

然后配置阿里云仓库

```html
 &lt mirrors &gt 
     &lt id &gt nexus-aliyun &lt /id &gt 
     &lt mirrorOf &gt * &lt /mirrorOf &gt 
     &lt name &gt nexus aliyun &lt /name &gt 
     &lt url &gt http://maven.aliyun.com/nexus/content/groups/public &lt /url &gt 
 &lt /mirrors &gt 
```

![ali](/images/1-Spring-Boot入门/2020-02-26-17-31-04.png)

# VS Code

安装插件：

- Java Extension Pack
- Maven for Java
- Spring Boot Extension Pack

在配置文件中添加(修改成自己的地址）：

```json
"java.home": "C:\\Program Files\\Java\\jdk-13.0.2",   //java jdk地址
"java.configuration.maven.userSettings": "C:\\Program Files\\apache-maven-3.6.3\\conf\\settings.xml",    //mvaen配置文件路径
"maven.executable.path": "C:\\Program Files\\apache-maven-3.6.3\\bin\\mvn.cmd",  //maven命令执行路径
"maven.terminal.useJavaHome": true,
"maven.terminal.customEnv": [
    {
        "environmentVariable": "JAVA_HOME",
        "value": "C:\\Program Files\\Java\\jdk-13.0.2"
    }
],
```

完成后重新启动 VS Code。

# Hello World 项目

功能：浏览器发送 hello 请求，服务器接受请求并处理，响应 Hello World 字符串。

## 创建 Maven 工程（jar）

按`Ctrl+Shift+P`，输入`Spring`，选择 `Spring Initializer: Generate a Maven Project`：

![generate](/images/1-Spring-Boot入门/2020-02-26-20-05-16.png)

然后选择 `java`：

![java](/images/1-Spring-Boot入门/2020-02-26-20-06-12.png)

`Group Id` 默认，`Artifact Id` 输入`spring-boot-helloworld`默认，`Spring Boot version` 选择 `2.2.4`：

![2.2.4](/images/1-Spring-Boot入门/2020-02-26-20-08-17.png)

搜索添加需要的依赖库，鼠标单击可勾选，这里添加以下几个：
- Spring Boot DevTools（代码修改热更新，无需重启）
- Spring Web（集成tomcat、SpringMVC）
- Lombok（智能生成setter、getter、toString等接口，无需手动生成，代码更简介）

![依赖库](/images/1-Spring-Boot入门/2020-02-26-20-09-58.png)

然后弹出目录选择框，创建`helloworld`目录并在该目录下生成项目。生成结束后会在右下角弹出提示，点击`open`。

打开 `src/main/java/com/example/demo/DemoApplication.java`文件，并点击右下角加载按钮，等待下载依赖：

![加载](/images/1-Spring-Boot入门/2020-02-26-20-26-49.png)

下载可能很慢，完成后如下图：

![下载完成](/images/1-Spring-Boot入门/2020-02-26-20-17-29.png)

## 编写主程序

VS Code 已经自动写好主程序，`DemoApplication.java`就是程序入口。

## 编写相关的Controller, Service

在同级目录下创建 `controller` 目录，在其下创建 `helloworldController.java`文件（注意将`Group Id` 和`Artifact Id`换成自己的）：

![创建文件](/images/1-Spring-Boot入门/2020-02-26-20-34-26.png)

```java
package com.doracmon.springboothelloworld.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class helloworldController {

    @ResponseBody
    @RequestMapping("/hello")
    public String hello() {
        return "Hello world!";
    }
}
```

## 运行程序

回到 `DemoApplication.java`，点击左侧Debug按钮，然后点击 `create a launch.json file`：

![launch](/images/1-Spring-Boot入门/2020-02-26-20-47-43.png)

自动生成的文件如下，不用修改。

![生成文件](/images/1-Spring-Boot入门/2020-02-26-20-48-47.png)

回到主程序，然后按下`F5`即可运行程序：

![运行程序](/images/1-Spring-Boot入门/2020-02-26-20-50-53.png)

浏览器进入`127.0.0.1:8080/hello`，看到返回 Hello World：

![hello](/images/1-Spring-Boot入门/2020-02-26-20-52-56.png)

## 简化部署

点击左侧 `MAVEN PROJECTS` 右键 `spring-boot-helloworld`，选择 `package` 进行打包：

![package](/images/1-Spring-Boot入门/2020-02-26-21-00-57.png)

打包后在 `target` 文件夹中：

![包](/images/1-Spring-Boot入门/2020-02-26-21-12-50.png)

进入 `target` 目录的命令行，输入 `java -jar spring-boot-helloworld-0.0.1-SNAPSHOT.jar`，即可直接运行该包：

![直接运行](/images/1-Spring-Boot入门/2020-02-26-21-14-58.png)

# 更多

更多VS Code相关配置见[环境搭建 - VS Code](/categories/环境搭建-VS-Code/)

# 参考资料

[尚硅谷SpringBoot顶尖教程(springboot之idea版spring boot)](https://www.bilibili.com/video/av20965295)

[JDK介绍与安装](https://blog.csdn.net/shuaigexiaobo/article/details/85280084)

[Maven windows 安装](https://blog.csdn.net/qq_36160730/article/details/91579235)

[vs code 安装与配置 springboot](https://www.jianshu.com/p/ef859019603d)