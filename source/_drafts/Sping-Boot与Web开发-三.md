---
title: Sping Boot与Web开发(三)
categories:
  - 教程笔记 - Spring Boot
tags:
  - 教程
  - Spring Boot
cover: /images/cover/springboot.jpg
abbrlink: 8212fb1f
---

# 简介

使用 SpringBoot 的步骤：

1. 创建 SpringBoot 应用，选中需要的模块；

2. SpringBoot 已经默认将这些场景配置好了，只需要在配置文件中指定少量配置就可以运行起来

3. 自己编写业务代码；

**自动配置原理**:

```
xxxxAutoConfiguration：给容器中自动配置组件；
xxxxProperties:配置类来封装配置文件的内容；
```

# webjars & 静态资源映射规则

## 从 Hello World 开始

按 `Ctrl+Shift+P` 输入 `Spring` 选择 `Spring Initializr: Generate a Maven Project`；

```yaml
Language: Java，
Group ID: com.example,
Artifact id: web-restfulcrud,
Spring Boot version: 2.2.5,
dependencies: [Spring Web,]
```

在 `DemoApplication.java` 同级文件夹下创建 `controller.HelloController.java`(新建文件后输入 `class` 并选择第二个可自动补全，`import` 文件在编辑文本时也会自动补全):

![](/images/Sping-Boot与Web开发-三/autofill.gif)

```java
package com.example.webrestfulcrud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}

}
```

运行主程序，访问 `localhost:8080/hello` 可以看到返回的 `Hello World！`。

## SpringBoot 对静态资源的映射规则

1. 所有 `/webjars/**` ，都在 `classpath:/META-INF/resources/webjars/` 寻找资源(webjars：以jar包的方式引入静态资源 http://www.webjars.org/，如引入 `jquery-webjar` 可在 `pom.xml` 中添加以下内容)

    ```xml
    <!--引入jquery-webjar 在访问的时候只需要写webjars下面资源的名称即可-->
    <dependency>
      <groupId>org.webjars</groupId>
      <artifactId>jquery</artifactId>
      <version>3.3.1</version>
    </dependency>
    ```

    重启项目，引入成功后，访问 `localhost:8080/webjars/jquery/3.3.1/jquery.js` 可以看到已经引入 `jquery`。

2. `/**` 访问当前项目的任何资源，都在（静态资源的文件夹）寻找映射

    ```
    "classpath:/META-INF/resources/", 
    "classpath:/resources/",
    "classpath:/static/", 
    "classpath:/public/" 
    "/"：当前项目的根路径
    ```

    当访问`localhost:8080/filename` 时，就会在静态资源文件夹里寻找 `filename`。

3. 欢迎页； 静态资源文件夹下的所有 `index.html` 页面；被 `/**`映射；

    在 `resource` 目录下创建 `public` 文件夹并新建 `index.html`（输入！回车自动补全）：

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        <h1>首页</h1>
    </body>
    </html>
    ```

    如，访问 `localhost:8080/` 就会寻找 `index.html` 页面

4. 所有的 `**/favicon.ico` 都是在静态资源文件下找；

# thymeleaf 模板引擎

**模板引擎**

![](/images/Sping-Boot与Web开发-三/2020-03-17-18-12-33.png)

JSP、Velocity、Freemarker、Thymeleaf

**SpringBoot 推荐的 Thymeleaf**，语法更简单，功能更强大；

## 引入 thymeleaf

在 `pom.xml` 文件中输入以下内容引入 thymeleaf
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

若要切换 thymeleaf 版本，如 3.0.9，添加以下内容：

```xml
<properties>
  <thymeleaf.version>3.0.9.RELEASE</thymeleaf.version>
  <!-- 布局功能的支持程序  thymeleaf3主程序  layout2以上版本 -->
  <!-- thymeleaf2   layout1-->
  <thymeleaf-layout-dialect.version>2.2.2</thymeleaf-layout-dialect.version>
</properties>
```

## Thymeleaf 使用

只要把 `HTML` 页面放在 `classpath:/templates/`，thymeleaf 就能自动渲染；

- 在 `templates` 文件夹下创建 `success.html`，输 `！` 并回车生成代码，然后输入：

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
  </head>
  <body>
      <h1>成功</h1>
  </body>
  </html>
  ```

- 然后在 `HelloController.java` 中添加该页面的控制器：

  ```java
  @RequestMapping("/success")
  public String success() {
      return "success";   // templates 中的文件
  }
  ```

  访问 `localhost:8080/success` 即可看到渲染的网页。

**使用步骤**：

1. 导入thymeleaf的名称空间

    在 `success.html` 文件中修改 `<html>` 标签：

    ```xml
    <html lang="en" xmlns:th="http://www.thymeleaf.org">
    ```

2. 使用thymeleaf语法；

    在 `success.html` 文件中修改 `<body>` 的内容：

    ```html
    <body>
        <h1>成功！</h1>
        <!--th:text 将div里面的文本内容设置为 -->
        <div th:text="${hello}">显示欢迎信息</div>
    </body>
    ```

访问 `localhost:8080/success` 可以看到网页已经将 `map<String, Object>` 中 `hello` 对应的 `你好` 取出来并显示。

## 语法规则

1. `th:text`改变当前元素里面的文本内容；​	`th：任意html属性`替换原生属性的值

![](/images/Sping-Boot与Web开发-三/2020-03-17-18-57-24.png)

2. 表达式

- 获取变量值 Variable Expressions: `${...}`

   1) 获取对象的属性、调用方法

   2) 使用内置的基本对象：

       ```yaml
       ctx : the context object.
       vars: the context variables.
       locale : the context locale.
       request : (only in Web Contexts) the HttpServletRequest object.
       response : (only in Web Contexts) the HttpServletResponse object.
       session : (only in Web Contexts) the HttpSession object.
       servletContext : (only in Web Contexts) the ServletContext object.
       ```

   3) 内置的工具对象：

       ```yaml
       execInfo : information about the template being processed.
       messages : methods for obtaining externalized messages inside variables expressions, in the same way as they would be obtained using #{…} syntax.
       uris : methods for escaping parts of URLs/URIs
       conversions : methods for executing the configured conversion service (if any).
       dates : methods for java.util.Date objects: formatting, component extraction, etc.
       calendars : analogous to #dates , but for java.util.Calendar objects.
       numbers : methods for formatting numeric objects.
       strings : methods for String objects: contains, startsWith, prepending/appending, etc.
       objects : methods for objects in general.
       bools : methods for boolean evaluation.
       arrays : methods for arrays.
       lists : methods for lists.
       sets : methods for sets.
       maps : methods for maps.
       aggregates : methods for creating aggregates on arrays or collections.
       ids : methods for dealing with id attributes that might be repeated (for example, as a result of an iteration).
       ```
- 选择表达式：和${}在功能上是一样 Selection Variable Expressions: `*{...}` 

  配合 `th:object`使用：

  ```html
  <div th:object="${session.user}">
    <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
    <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
    <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
  </div>
  ```

- 获取国际化内容 Message Expressions: `#{...}` 

- 定义URL Link URL Expressions: `@{...}` 

  ```java
  @{/order/process(execId=${execId},execType='FAST')}
  ```

- 片段引用表达式 `Fragment Expressions: ~{...}`
 
  ```html
  <div th:insert="~{commons :: main}">...</div>
  ```

3. 字面量 Literals:
 
  ```
  Text literals: 'one text' , 'Another one!' ,…
  Number literals: 0 , 34 , 3.0 , 12.3 ,…
  Boolean literals: true , false
  Null literal: null
  Literal tokens: one , sometext , main ,…
  ```
    
4. 文本操作 Text operations:

  ```
  String concatenation: +
  Literal substitutions: |The name is ${name}|
  ```

5. 数学运算 Arithmetic operations:
   
  ```
  Binary operators: + , - , * , / , %
  Minus sign (unary operator): -
  ```

6. 布尔运算 Boolean operations:
 
  ```
  Binary operators: and , or
  Boolean negation (unary operator): ! , not
  ```

7. 比较运算 Comparisons and equality:
  
  ```
  Comparators: > , < , >= , <= ( gt , lt , ge , le )
  Equality operators: == , != ( eq , ne )
  ```

8. 条件运算(三元运算符) Conditional operators:
  
  ```
  If-then: (if) ? (then)
  If-then-else: (if) ? (then) : (else)
  Default: (value) ?: (defaultvalue)
  ```

9. Special tokens:
    
  ```
  No-Operation: _ 
  ```

**实例**：

修改 `HelloController.java` 的 `success` 控制器：

```java
@RequestMapping("/success")
public String success(Map<String, Object> map) {
    map.put("hello", "<h1>你好</h1>");
    map.put("users", Arrays.asList("zhangsan", "lisi", "wangwu"));
    return "success";
}
```

修改 `success.html` 文件的 `<body>` 中的内容：

```html
<body>
    <h1>成功</h1>
    <!--th:text 将div里面的文本内容设置为-->
    <div id="div01" class="myDiv" th:id="${hello}" th:class="${hello}" th:text="${hello}">
        显示欢迎信息
    </div>

    <hr/>
    <div th:text="${hello}"></div>
    <div th:utext="${hello}"></div>
    <hr/>

    <!-- th:each 每次遍历都会生成当前这个标签: 3 个 h4 -->
    <h4 th:text="${user}" th:each="user:${users}"></h4>
    <hr/>

    <h4>
        <span th:each="user:${users}"> [[${user}]]</span>
    </h4>
</body>
```

运行结果如下：

![](/images/Sping-Boot与Web开发-三/2020-03-17-19-46-52.png)


















# 更多

更多Spring Boot教程笔记见[教程笔记 - Spring Boot](/categories/教程笔记-Spring-Boot/)

# 参考资料

[尚硅谷SpringBoot顶尖教程(springboot之idea版spring boot)](https://www.bilibili.com/video/av20965295)