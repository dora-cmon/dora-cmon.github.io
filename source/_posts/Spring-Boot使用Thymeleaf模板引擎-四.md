---
title: Spring Boot使用Thymeleaf模板引擎(四)
categories:
  - 教程笔记 - Spring Boot
tags:
  - 教程
  - Spring Boot
cover: /images/cover/springboot.jpg
abbrlink: 66045c6e
date: 2020-03-25 21:08:51
---


**模板引擎**

![](/images/Spring-Boot使用Thymeleaf模板引擎-四/2020-03-17-18-12-33.png)

JSP、Velocity、Freemarker、Thymeleaf

**SpringBoot 推荐 Thymeleaf 模板引擎**，语法更简单，功能更强大；

# 引入 thymeleaf

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

# Thymeleaf 使用

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

## 使用步骤：

### 导入thymeleaf的名称空间

在 `success.html` 文件中修改 `<html>` 标签：

```xml
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

### 使用thymeleaf语法

在 `success.html` 文件中修改 `<body>` 的内容：

```html
<body>
    <h1>成功！</h1>
    <!--th:text 将div里面的文本内容设置为 -->
    <div th:text="${var}}">显示欢迎信息</div>
</body>
```

模板引擎会将变量 `var` 的值取出并显示。

## 语法规则

1. `th:text`改变当前元素里面的文本内容；​	`th：任意html属性`替换原生属性的值

    ![](/images/Spring-Boot使用Thymeleaf模板引擎-四/2020-03-17-18-57-24.png)

2. 表达式

   - 获取变量值 Variable Expressions: `${...}`

      (1) 获取对象的属性、调用方法

      (2) 使用内置的基本对象：

          ```yaml
          ctx : the context object.
          vars: the context variables.
          locale : the context locale.
          request : (only in Web Contexts) the HttpServletRequest object.
          response : (only in Web Contexts) the HttpServletResponse object.
          session : (only in Web Contexts) the HttpSession object.
          servletContext : (only in Web Contexts) the ServletContext object.
          ```

      (3) 内置的工具对象：

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

# 实例演示：

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

![](/images/Spring-Boot使用Thymeleaf模板引擎-四/2020-03-17-19-46-52.png)

# 更多

更多Spring Boot教程笔记见[教程笔记 - Spring Boot](/categories/教程笔记-Spring-Boot/)

# 参考资料

[尚硅谷SpringBoot顶尖教程(springboot之idea版spring boot)](https://www.bilibili.com/video/av20965295)