---
title: 配置了maven的国内镜像后，项目未通过镜像下载
categories:
  - 疑难杂症
tags:
  - MobaXterm
  - ssh密钥
cover: /images/cover/maven.jpg
abbrlink: cea3c7c6
date: 2020-03-17 16:28:01
---


本文转载自[配置了maven的国内镜像后，问题：(https://repo.maven.apache.org/maven2): Not authorized , ReasonPhrase:Authorizatio](https://blog.csdn.net/pkx1993/article/details/86691426)

# 问题：

最近学习springboot,从网上导入工程后，都出现maven无法下载相关资源的问题。

# 解决思路如下：

1. 怀疑是网络限制的原因，故在网上找了相关的解决方案，使用了阿里的云仓库，在maven的settings.xml文件的Mirrors中添加了如下内容：

```xml
<mirrors>
    
    <!-- 阿里云仓库 -->
    <mirror>
        <id>alimaven</id>
        <mirrorOf>central</mirrorOf>
        <name>aliyun maven</name>
        <url>https://maven.aliyun.com/nexus/content/repositories/central/</url>
        </mirror>
    
    <!-- 中央仓库1 -->
    <mirror>
        <id>repo1</id>
            <mirrorOf>central</mirrorOf>
        <name>Human Readable Name for this Mirror.</name>
        <url>http://repo1.maven.org/maven2/</url>
        </mirror>
    
    <!-- 中央仓库2 -->
    <mirror>
        <id>repo2</id>
        <mirrorOf>central</mirrorOf>
        <name>Human Readable Name for this Mirror.</name>
        <url>http://repo2.maven.org/maven2/</url>
    </mirror>
</mirrors>
```
2. 添加完成后，发现部分资源 可以下载了，但仍然有许多资源无法下载，后来参考了https://blog.sonatype.com/using-nexus-3-as-your-repository-part-1-maven-artifacts，发现pom.xml都是继承自super pom，
super pom中的有如下内容：

```xml
<repositories>
    <repository>
        <id>central</id>
        <name>Central Repository</name>
        <url>http://repo.maven.apache.org/maven2</url>
        <layout>default</layout>
        <snapshots>
        <enabled>false</enabled>
        </snapshots>
    </repository>
</repositories>

<pluginRepositories>
    <pluginRepository>
        <id>central</id>
        <name>Central Repository</name>
        <url>http://repo.maven.apache.org/maven2</url>
        <layout>default</layout>
        <snapshots>
        <enabled>false</enabled>
        </snapshots>
        <releases>
        <updatePolicy>never</updatePolicy>
        </releases>
    </pluginRepository>
</pluginRepositories>
```
故当maven下载资源时，会优先使用http://repo.maven.apache.org/maven2去下载。

最终解决方案是在自定义的 `pom.xml` 中加入如下内容：

```xml 
<repositories>
    <repository>
        <id>central</id>
        <url>http://host:port/content/groups/public</url>
    </repository>
</repositories>
 
<pluginRepositories>
    <pluginRepository>
        <id>central</id>
        <url>http://host:port/content/groups/public</url>
    </pluginRepository>
</pluginRepositories>
```

原作者未提及，作一补充：

若要修改为阿里源，则在 `pom.xml` 中添加：

```xml
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