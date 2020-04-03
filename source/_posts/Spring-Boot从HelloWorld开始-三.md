---
title: Spring Boot从HelloWorld开始(三)
categories:
  - 教程笔记 - Spring Boot
tags:
  - 教程
  - Spring Boot
cover: /images/cover/springboot.jpg
abbrlink: 9fa67ccf
date: 2020-03-25 21:08:31
---


功能：浏览器发送 hello 请求，服务器接受请求并处理，响应 Hello World 字符串。

# 创建 Maven 工程（jar）

按`Ctrl + Shift + P`，输入 `Spring`，选择 `Spring Initializer: Generate a Maven Project`：

![generate](/images/Spring-Boot从HelloWorld开始-三/2020-02-26-20-05-16.png)

然后选择 `java`：

![java](/images/Spring-Boot从HelloWorld开始-三/2020-02-26-20-06-12.png)

`Group Id` 默认；
`Artifact Id` 输入`spring-boot-helloworld`；
`Spring Boot version` 选择 `2.2.x`。

![2.2.4](/images/Spring-Boot从HelloWorld开始-三/2020-02-26-20-08-17.png)

搜索添加需要的依赖库，鼠标单击可勾选，这里添加以下几个：
- Spring Boot DevTools（代码修改热更新，无需重启）
- Spring Web（集成tomcat、SpringMVC）
- Lombok（智能生成setter、getter、toString等接口，无需手动生成，代码更简洁）

![依赖库](/images/Spring-Boot从HelloWorld开始-三/2020-02-26-20-09-58.png)

然后弹出目录选择框，选择生成项目的目录位置。生成结束后会在桌面右下角弹出提示，点击`open`。

点击右下角加载按钮，等待下载 Maven 依赖：

![加载](/images/Spring-Boot从HelloWorld开始-三/2020-02-26-20-26-49.png)

**注意**：此时可能下载地址并不是阿里云镜像，速度极其缓慢（原因参考{% post_link 配置了maven的国内镜像后，项目未通过镜像下载 %}）**在 `pom.xml` 中添加以下内容**：

```xml
<!-- pom.xml 添加内容如下 -->
<repositories>
    <repository>
        <id>central</id>
            <url>http://maven.aliyun.com/nexus/content/repositories/central</url>
    </repository>
</repositories>
    
<pluginRepositories>
    <pluginRepository>
            <id>central</id>
            <url>http://maven.aliyun.com/nexus/content/repositories/central</url>
    </pluginRepository>
</pluginRepositories>
```

下载完成后如下图：

![下载完成](/images/Spring-Boot从HelloWorld开始-三/2020-02-26-20-17-29.png)

# 代码编写

## 目录结构

```
com
└── example
    └── springboothelloworld
        ├── controller
        │   └── helloController.java
        └── DemoApplication.java
```

## 编写主程序

VS Code 已经自动写好主程序，`DemoApplication.java` 就是程序入口。

## 编写 Controller

在同级目录下创建 `controller` 目录，在其下创建 `helloController.java`文件(新建文件后输入 `class` 并选择第二个选项可自动补全，`import` 文件在编辑文本时也会自动补全):

![](/images/Sping-Boot与Web开发-三/autofill.gif)）：

```java
package com.example.springboothelloworld.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

/**
 * helloController
 */
@Controller
public class helloController {

    @ResponseBody
    @RequestMapping("/hello")
    public String hello() {
        return "Hello World!";
    }
    
}
```

# 运行程序

回到 `DemoApplication.java`，按下`F5`即可运行程序：

![运行程序](/images/Spring-Boot从HelloWorld开始-三/2020-02-26-20-50-53.png)

浏览器进入`127.0.0.1:8080/hello`，看到返回 Hello World：

![hello](/images/Spring-Boot从HelloWorld开始-三/2020-02-26-20-52-56.png)

# 简化部署

点击左侧 `MAVEN PROJECTS` 右键 `spring-boot-helloworld`，选择 `package` 进行打包：

![package](/images/Spring-Boot从HelloWorld开始-三/2020-02-26-21-00-57.png)

打包后文件保存在 `target` 文件夹中：

![包](/images/Spring-Boot从HelloWorld开始-三/2020-02-26-21-12-50.png)

进入 `target` 目录，输入命令 `java -jar spring-boot-helloworld-0.0.1-SNAPSHOT.jar`，即可直接运行该包：

![直接运行](/images/Spring-Boot从HelloWorld开始-三/2020-02-26-21-14-58.png)

# 更多

更多Spring Boot教程笔记见[教程笔记 - Spring Boot](/categories/教程笔记-Spring-Boot/)

# 参考资料

[尚硅谷SpringBoot顶尖教程(springboot之idea版spring boot)](https://www.bilibili.com/video/av20965295)
