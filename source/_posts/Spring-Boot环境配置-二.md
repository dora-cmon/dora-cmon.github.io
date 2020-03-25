---
title: Spring Boot环境配置(二)
categories:
  - 教程笔记 - Spring Boot
tags:
  - 教程
  - Spring Boot
cover: /images/cover/springboot.jpg
date: 2020-03-25 21:08:16
---



# 环境准备

- jdk 8
- maven 3
- SpringBoot 2
- VS Code

# JDK 8

[官方网站](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)下载 JDK 并安装。

## Windows （将路径修改为你的 jdk 安装路径）：

右键计算机 -> 属性 -> 高级系统设置 -> 高级 -> 环境变量，设置`Path` 和 `JAVA_HOME` 环境变量：

![path](/images/Spring-Boot环境配置-二/2020-02-26-16-45-47.png)


![java_home](/images/Spring-Boot环境配置-二/2020-02-26-16-47-54.png)

## Ubuntu:

通常自带 JAVA，执行命令 `java -version`，若版本为 `1.8.x` 则已有 jdk 8：

```
openjdk version "1.8.0_242"
```

若没有安装 JAVA 或版本不对，则：

```bash
sudo apt-get update
sudo apt-get install openjdk-8-jdk
```

## 验证安装

输入命令 `java -version` 检测是否成功：

```bash
openjdk version "1.8.0_242"
OpenJDK Runtime Environment (build 1.8.0_242-8u242-b08-0ubuntu3~18.04-b08)
OpenJDK 64-Bit Server VM (build 25.242-b08, mixed mode)
```

# Maven

## Windows：

[官方网站](https://maven.apache.org/download.cgi)下载二进制 `zip` 文件并解压([清华镜像站](https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/))。

配置环境变量：

右键计算机 -> 属性 -> 高级系统设置 -> 高级 -> 环境变量，设置`Path` 和 `MAVEN_HOME` 环境变量：

![Path](/images/Spring-Boot环境配置-二/2020-02-26-16-59-00.png)

![MAVEN_HOME](/images/Spring-Boot环境配置-二/2020-02-26-17-01-26.png)

## Ubuntu:

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

## 验证安装

然后在命令行输入`mvn -v`检测是否成功：

```bash
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: C:\Program Files\apache-maven-3.6.3\bin\..
Java version: 13.0.2, vendor: Oracle Corporation, runtime: C:\Program Files\Java\jdk-13.0.2
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
```

## 仓库换源

配置阿里云仓库：

```xml
<mirror>
    <id>nexus-aliyun</id>
    <mirrorOf>*</mirrorOf>
    <name>nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```

![](/images/Spring-Boot环境配置-二/2020-03-25-16-49-35.png)

# VS Code

安装插件：

- Java Extension Pack
- Spring Boot Extension Pack

在配置文件中添加(修改成自己的地址）：

```json
// Java Config
"java.home": "/usr/lib/jvm/java-1.8.0-openjdk-amd64/",   //java jdk地址
"java.configuration.maven.userSettings": "/usr/local/apache-maven-3.6.3/confsettings.xml",    //mvaen配置文件路径
// Maven Config
"maven.executable.path": "/usr/local/apache-maven-3.6.3/bin/mvn",  //maven命令执行路径
"maven.terminal.useJavaHome": true,
"maven.terminal.customEnv": [
    {
        "environmentVariable": "JAVA_HOME",
        "value": "/usr/lib/jvm/java-1.8.0-openjdk-amd64/"
    }
],
```

完成后重新启动 VS Code。

# 更多

更多Spring Boot教程笔记见[教程笔记 - Spring Boot](/categories/教程笔记-Spring-Boot/)

# 参考资料

[尚硅谷SpringBoot顶尖教程(springboot之idea版spring boot)](https://www.bilibili.com/video/av20965295)

[JDK介绍与安装](https://blog.csdn.net/shuaigexiaobo/article/details/85280084)

[Maven windows 安装](https://blog.csdn.net/qq_36160730/article/details/91579235)

[vs code 安装与配置 springboot](https://www.jianshu.com/p/ef859019603d)

[Ubuntu安装maven](https://blog.csdn.net/qq_29695701/article/details/90705181)