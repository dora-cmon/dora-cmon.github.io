---
title: Spring Boot入门(一)
categories:
  - 教程笔记 - Spring Boot
tags:
  - 教程
  - Spring Boot
cover: /images/cover/springboot.jpg
abbrlink: c7a119d9
date: 2020-02-27 13:21:37
---


# Spring Boot 简介

Spring Boot 是由 Pivotal 团队在2013年开始研发、2014年4月发布第一个版本的全新开源的轻量级框架。它基于 Spring4.0 设计，不仅继承了 Spring 框架原有的优秀特性，而且还通过简化配置来进一步**简化了 Spring 应用的整个搭建和开发过程**。另外 Spring Boot 通过集成大量的框架使得依赖包的版本冲突，以及引用的不稳定性等问题得到了很好的解决。

**优点**：

- 快速创建独立运行的 Spring 项目以及与主流框架集成
- 使用嵌入式的 Servlet 容器，应用无需打成 WAR 包
- starters 自动依赖于版本控制
- 大量的自动配置，简化开发，也可修改默认值
- 无需配置XML，无代码生成，开箱即用
- 准生产环境的的运行时应用监控
- 与云计算的天然集成

**缺点**：

入门容易，精通难

# 微服务简介

微服务是一个新兴的**软件架构**，就是**把一个大型的单个应用程序和服务拆分为数十个的支持微服务**。一个微服务的策略可以让工作变得更为简便，它可扩展单个组件而不是整个的应用程序堆栈，从而满足服务等级协议。

每个功能元素最终都是一个可独立替换和独立升级的软件单元。

# 环境准备

- jdk 13.0.2
- maven 3.6.3
- SpringBoot 1.5.9
- VS Code

## JDK

[官方网站](https://www.oracle.com/java/technologies/javase-downloads.html)下载 JDK 并安装([华为镜像站](https://repo.huaweicloud.com/java/jdk/))。

**Windows**：

  右键计算机 -> 属性 -> 高级系统设置 -> 高级 -> 环境变量，设置`Path` 和 `JAVA_HOME` 环境变量：

  ![path](/images/Spring-Boot入门-一/2020-02-26-16-45-47.png)


  ![java_home](/images/Spring-Boot入门-一/2020-02-26-16-47-54.png)

**Ubuntu**:

  ```bash
  sudo add-apt-repository ppa:linuxuprising/java
  sudo apt-get update
  sudo apt-get install oracle-java13-installer
  ```

然后在命令行输入`java -version`检测是否成功：

```bash
java 13.0.2 2020-01-14
Java(TM) SE Runtime Environment (build 13.0.2+8)
Java HotSpot(TM) 64-Bit Server VM (build 13.0.2+8, mixed mode, sharing)
```
## Maven

**Windows**：

  [官方网站](https://maven.apache.org/download.cgi)下载二进制 `zip` 文件并解压([清华镜像站](https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/))。

  配置环境变量：

  右键计算机 -> 属性 -> 高级系统设置 -> 高级 -> 环境变量，设置`Path` 和 `MAVEN_HOME` 环境变量：

  ![Path](/images/Spring-Boot入门-一/2020-02-26-16-59-00.png)

  ![MAVEN_HOME](/images/Spring-Boot入门-一/2020-02-26-17-01-26.png)

**Ubuntu**:

  [官方网站](https://maven.apache.org/download.cgi)下载二进制 `tar.gz` 文件([清华镜像站](https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/))并解压至 `/usr/local/maven`。

  ```bash
  sudo tar zxvf apache-maven-3.6.3-bin.tar.gz -C /usr/local
  ```

  配置环境变量：

  ```bash
  echo "MAVEN_HOME=/usr/local/apache-maven-3.6.3" >> ~/.bashrc
  echo "PATH=\${PATH}:\${MAVEN_HOME}/bin" >> ~/.bashrc
  echo "export PATH" >> ~/.bashrc
  source ~/.bashrc
  ```


然后在命令行输入`mvn -v`检测是否成功：

```bash
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: C:\Program Files\apache-maven-3.6.3\bin\..
Java version: 13.0.2, vendor: Oracle Corporation, runtime: C:\Program Files\Java\jdk-13.0.2
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
```

创建 `maven-repository` 仓库文件夹目录并添加仓库地址：
```bash
# Windows 

# 新建 C:\Program Files\apache-maven-3.6.3\maven-repository

# 在 C:\Program Files\apache-maven-3.6.3\conf\settings.xml 中添加仓库地址

<localRepository>C:\Program Files\apache-maven-3.6.3\maven-repository</localRepository>

# Ubuntu
mkdir /usr/local/apache-maven-3.6.3/maven-repository
sudo gedit /usr/local/apache-maven-3.6.3/conf/settings.xml

# 添加仓库地址
<localRepository>/usr/local/apache-maven-3.6.3/maven-repository</localRepository>
```

![仓库地址](/images/Spring-Boot入门-一/2020-02-26-17-14-04.png)

然后配置阿里云仓库

```xml
<mirrors>
    <id>nexus-aliyun</id>
    <mirrorOf>*</mirrorOf>
    <name>nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirrors>
```

![ali](/images/Spring-Boot入门-一/2020-02-26-17-31-04.png)

# VS Code

安装插件：

- Java Extension Pack
- Spring Boot Extension Pack

在配置文件中添加(修改成自己的地址）：

```json
// Windows
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

// Ubuntu
"java.home": "/usr/lib/jvm/jdk-13/",   //java jdk地址
"java.configuration.maven.userSettings": "/usr/local/apache-maven-3.6.3/confsettings.xml",    //mvaen配置文件路径
"maven.executable.path": "/usr/local/apache-maven-3.6.3/bin/mvn",  //maven命令执行路径
"maven.terminal.useJavaHome": true,
"maven.terminal.customEnv": [
    {
        "environmentVariable": "JAVA_HOME",
        "value": "/usr/lib/jvm/jdk-13/"
    }
],
```

完成后重新启动 VS Code。

# Hello World 项目

功能：浏览器发送 hello 请求，服务器接受请求并处理，响应 Hello World 字符串。

## 创建 Maven 工程（jar）

按`Ctrl+Shift+P`，输入`Spring`，选择 `Spring Initializer: Generate a Maven Project`：

![generate](/images/Spring-Boot入门-一/2020-02-26-20-05-16.png)

然后选择 `java`：

![java](/images/Spring-Boot入门-一/2020-02-26-20-06-12.png)

`Group Id` 默认，`Artifact Id` 输入`spring-boot-helloworld`默认，`Spring Boot version` 选择 `2.2.4`：

![2.2.4](/images/Spring-Boot入门-一/2020-02-26-20-08-17.png)

搜索添加需要的依赖库，鼠标单击可勾选，这里添加以下几个：
- Spring Boot DevTools（代码修改热更新，无需重启）
- Spring Web（集成tomcat、SpringMVC）
- Lombok（智能生成setter、getter、toString等接口，无需手动生成，代码更简介）

![依赖库](/images/Spring-Boot入门-一/2020-02-26-20-09-58.png)

然后弹出目录选择框，创建`helloworld`目录并在该目录下生成项目。生成结束后会在右下角弹出提示，点击`open`。

打开 `src/main/java/com/example/demo/DemoApplication.java`文件，并点击右下角加载按钮，等待下载依赖：

![加载](/images/Spring-Boot入门-一/2020-02-26-20-26-49.png)

**注意**：此时可能下载地址并不是阿里云镜像，依旧极其缓慢，原因参考{% post_link 配置了maven的国内镜像后，项目未通过镜像下载 %}。按照该文章修改 `pom.xml` 后速度起飞。

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

完成后如下图：

![下载完成](/images/Spring-Boot入门-一/2020-02-26-20-17-29.png)

## 编写主程序

VS Code 已经自动写好主程序，`DemoApplication.java`就是程序入口。

## 编写相关的Controller, Service

在同级目录下创建 `controller` 目录，在其下创建 `helloworldController.java`文件(新建文件后输入 `class` 并选择第二个可自动补全，`import` 文件在编辑文本时也会自动补全):

![](/images/Sping-Boot与Web开发-三/autofill.gif)）：

![创建文件](/images/Spring-Boot入门-一/2020-02-26-20-34-26.png)

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

![launch](/images/Spring-Boot入门-一/2020-02-26-20-47-43.png)

自动生成的文件如下，不用修改。

![生成文件](/images/Spring-Boot入门-一/2020-02-26-20-48-47.png)

回到主程序，然后按下`F5`即可运行程序：

![运行程序](/images/Spring-Boot入门-一/2020-02-26-20-50-53.png)

浏览器进入`127.0.0.1:8080/hello`，看到返回 Hello World：

![hello](/images/Spring-Boot入门-一/2020-02-26-20-52-56.png)

## 简化部署

点击左侧 `MAVEN PROJECTS` 右键 `spring-boot-helloworld`，选择 `package` 进行打包：

![package](/images/Spring-Boot入门-一/2020-02-26-21-00-57.png)

打包后在 `target` 文件夹中：

![包](/images/Spring-Boot入门-一/2020-02-26-21-12-50.png)

进入 `target` 目录的命令行，输入 `java -jar spring-boot-helloworld-0.0.1-SNAPSHOT.jar`，即可直接运行该包：

![直接运行](/images/Spring-Boot入门-一/2020-02-26-21-14-58.png)

# Hello World 探究

## POM 文件

### 父项目

`pom.xml` 中的 `parent` 标签：

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.4.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

管理 Spring Boot 应用里的所有依赖版本，是 Spring Boot 的版本仲裁中心。以后导入依赖默认不需要显示版本，没有在 dependencies 里面管理的依赖需要声明版本号。

### 导入的依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**spring-boot-starter**：场景启动器，导入 web 模块正常运行所依赖的组件；

Spring Boot 将所有的功能场景都抽取出来，做成一个个的 starters（启动器），只需要在项目里引入这些 starter 相关场景的所有依赖都会导入进来。

## 主程序类，主入口类

```java
/**
 *  @SpringBootApplication 来标注一个主程序类，说明这是一个 Spring Boot 应用
 */

@SpringBootApplication
public class DemoApplication {

	public static void main(String[] args) {
        // Spring 应用启动
		SpringApplication.run(DemoApplication.class, args);
	}

}
```

`@SpringBootApplication`：Spring Boot Application 修饰某个类说明该类是 Spring Boot 的主配置类，Spring Boot 运行该类的 main 方法来启动 Spring Boot 应用。

进入`SpringBootApplication.class`类：

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
```

- `SpringBootConfiguration`：Spring Boot 的配置类
  
  修饰某个类，表示这是一个 Spring Boot 的配置；`@Configuration`：用该注解修饰配置类；配置类 —— 配置文件；配置类业是容器中的一个`@Component`组件。

- `@EnableAutoConfiguration`：开启自动配置功能
  
  Spring Boot 自动配置；`@EnableAutoConfiguration` 告诉 Spring Boot 开启自动配置功能；这样自动配置才能生效。

  ```java
  @AutoConfigurationPackage
  @Import(AutoConfigurationImportSelector.class)
  public @interface EnableAutoConfiguration {
  ```
  - `@AutoConfigurationPackage`：自动配置包

    将主配置类（`@SpringBootApplication`修饰的类）所在包及其所有子包里的所有组件扫描到Spring容器；
    
  - `@Import(AutoConfigurationImportSelector.class)`：Spring 的底层注解 `@Import`，给容器中导入一个组件；导入的组件由 `AutoConfigurationPackages.Registrar.class` 来指定。

    `EnableAutoConfigurationImportSelector`:导入哪些组件的选择器；将所有需要导入的组件以全类名的方式返回；这些组件会被添加到容器中，会给容器中导入非常多的自动配置类（xxxAutoConfiguration）；即给容器中导入该场景需要的所有组件，并配置好这些组件。

## `resources` 文件夹目录结构

- **static**：保存所有的静态资源，js css images
- **templates**：保存所有的模板页面；（Spring Boot 默认 jar 包使用嵌入式的 Tomcat，默认不支持 JSP 页面）；可以使用模板引擎（freemarker，thymeleaf）
- **application.properties**：Spring Boot 应用的配置文件


# 更多

更多Spring Boot教程笔记见[教程笔记 - Spring Boot](/categories/教程笔记-Spring-Boot/)

# 参考资料

[尚硅谷SpringBoot顶尖教程(springboot之idea版spring boot)](https://www.bilibili.com/video/av20965295)

[JDK介绍与安装](https://blog.csdn.net/shuaigexiaobo/article/details/85280084)

[Maven windows 安装](https://blog.csdn.net/qq_36160730/article/details/91579235)

[vs code 安装与配置 springboot](https://www.jianshu.com/p/ef859019603d)

[Ubuntu安装maven](https://blog.csdn.net/qq_29695701/article/details/90705181)

[Ubuntu 安装java 13](https://www.jianshu.com/p/9eab518e9814)