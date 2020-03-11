---
title: 2.Spring Boot配置
categories:
  - 教程笔记 - Spring Boot
tags:
  - 教程
  - Spring Boot
abbrlink: 2847a771
cover: /images/cover/springboot.jpg
date: 2020-02-29 20:02:11
---


# 配置文件

Spring Boot 使用一个全局配置文件，配置文件位置在 `src/main/resources` 目录下或 `类路径/config`下，文件名固定：

- application.properties
- application.yml

配置文件可以修改 Spring Boot 的默认配置。

# YAML(YAML Ain't Markup Language)

以数据为中心，比 `json, xml` 等更适合做配置文件：

```ymal
server:
  port: 8081
```

## YAML 基本语法

`k:(空格)v` 表示一对键值对（空格必须有），以缩进来控制层级关系。

```yaml
server:
  port: 8081
  path: /hello 
```

- 字面量：普通值（数字，字符串，布尔）

  `k: v`：字符串默认不加引号；`""` 双引号，不转义特殊字符；`''` 单引号，转义特殊字符

- 对象、Map（属性和值/键值对）

  在对象下另起一行写属性和值，注意缩进：

  ```yaml
  friends:
    lastName: Li
    age: 23
  ```

  行内写法：

  ```yaml
  friends: {lastName: Li, age: 23}
  ```

- 数组（List, Set）:

  用 `-` 表示数组中的一个元素：

  ```yaml
  pets:
    - cat 
    - dog
    - pig
  ```

  行内写法：

  ```yaml
  pets: [cat, dog, pig]
  ```

## YAML 配置文件值获取(注入)

配置文件：

```yaml
person:
  lastName: hello
  age: 18
  boss: false
  birth: 2017/12/12
  maps: {k1: v1, k2: v2}
  lists: [lisi, zhaoliu]
  dog:
    name: 小狗
    age: 12
```

javaBean：

```java
/**
 * 将配置文件中配置的每一个属性的值，映射到这个组件中
 * @ConfigurationProperties: 告诉 Spring Boot 将本类中的所有属性和配置文件中相关的配置进行绑定；
 *       prefix = "person"：配置文件中那个对象下的所有属性进行意义映射
 *
 * 只有这个组件是容器中的组件，才能提供容器的@ConfigurationProperties功能；
 */
@Component
@Configuration Properties(prefix = "person")
public class Person {

  private String lastName;
  private Integer age;
  private Boolean boss;
  private Date birth;

  private Map &lt String, Object &gt  maps;
  private List &lt object &gt  lists;
  private Dog dog;
}
```

可以导入配置文件处理器，用来在编写配置文件时提供提示：

```xml
&lt !--导入配置文件处理器，配置文件进行绑定就会有提示-- &gt
&lt dependency &gt
   &lt groupId &gt org.springframework.boot &lt /groupId &gt 
   &lt artifactId &gt spring-boot-configuration-processor &lt /artifactId &gt 
   &lt optional &gt true &lt /optional &gt 
 &lt /dependency &gt 
```

# `@ConfigurationProperties` 与 `@Value`的区别

| |`@ConfigurationProperties`|`@Value`|
|----|----|----|
|功能|批量注入配置文件中的属性|逐个指定|
|松散语法|支持|不支持|
|SpEL|不支持|支持|
|JSR303数据校验|支持|不支持|
|复杂类型封装|支持|不支持|

- 如果只在某个业务逻辑中需要获取一下配置文件中的某项值，则用 `@Value`；

- 如果专门编写了一个 javaBean 来和配置文件进行映射，则使用 `@ConfigurationProperties`;

## `@ConfigurationProperties` 例子

```java
@Component
@ConfigurationProperties(prefix = "person")
@Validated
public class Person{
  //lastName 必须是邮箱格式
  @Email
  private String lastName;
  private Integer age;
  private Boolean boss;
}
```

## `@Value` 例子

```java
@Component
public class Person{
  
  @Value("${person.last-name}")
  private String lastName;

  @Value("#{11*2}")
  private Integer age;

  @Value("true")
  private Boolean boss;
}
```

# `@PropertySource, @ImportResource, @Bean`

- `@PropertySource`：加载指定的配置文件；

  而`@ConfigurationProperties()` 默认从全局配置文件中获取值，而 `@PropertySource` 可以从指定文件中获取值：

  ```java
  @PropertySource(value = {"classpath:person.properties"})
  @Component
  @ConfigurationProperties(prefix = "person")
  public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
  }
  ```

- `@ImportResource`：导入 Spring 的配置文件，让配置文件里的内容生效；

  Spring Boot 里面没有 Spring 的配置文件，自己编写的配置文件也不能自动识别；想让 Spring 配置文件加载生效，需要将 `@ImportResource` 标注在一个配置类上。

  ```java
  // 导入 Spring 的配置文件让其生效
  @ImportResource(locations = {"classpath:beans.xml"})
  ```

**Spring Boot 推荐给容器中添加组件的方式**：使用全注解的方式

1. 配置类 ———— Spring 配置文件
  
2. 使用 `@Bean` 给容器中添加组件
  
  ```java
  /**
    * @Configuration:指明当前类是一个配置类，替代 Spring 配置文件
    * 在配置文件中用 &lt bean &gt  &lt /bean/ &gt 标签添加组件
    */
  @Configuration
  public class MyAppConfig {
    // 将方法的返回值添加到容器中；容器中这个组件默认的id就是方法名
    @Bean
    public HelloService helloService(){
      Systemout.println("配置类@Bean给容器中添加组件...");
      return new HelloService();
    }
  }
  ```

# 配置文件占位符

```java
app.name = MyApp
app.description = ${app.name} is a Spring Boot application
```

## 随机数

```java
${random.value}, ${random.int}, ${random.long}
${random.int(10)}, ${random.int[1024, 65535]}
```

## 占位符默认值

占位符可以在配置文件中引用前面配置过的属性，如果找不到指定属性时，可以用 `${app.name:默认值}` 来指定其默认值。

# Profile 多环境支持

Profile 是 Spring 对不融环境提供不同配置功能的支持，可以通过激活、指定参数等方式快速切换环境

## 多 profile 文件形式：
   
格式：application-{profile.properties}:
  - application-dev.properties
  - application-prod.properties

默认使用 `application.properties` 文件

## yml支持多文档块模式：
   
```yml
server:
  port: 8081
spring:
  profiles:
    active: dev
---
server:
  port: 8083
spring:
  profiles: dev
---
server:
  port: 8084
spring:
  profiles: prod
```

## 激活方式:
   
|||
|-|-|
|命令行|java -jar file.jar --spring.profiles.active=dev|
|配置文件中指定|springprofiles.active=dev|
|jvm参数|-Dspring.profiles.active=dev|

# 配置文件加载位置

Spring Boot 启动会扫描一下位置的 `application.properties` 或者 `application.yml` 文件作为 Spring boot 的默认配置文件

```
- file:./config/
- file:./
- classpath:/config/
- classpath:/
```
以上顺序优先级从高到低，所有位置的文件都会被加载，高优先级配置内容覆盖低优先级配置内容，所有配置互补。（也可以通过配置 spring.config.location 来改变默认配置）

# 外部配置加载顺序

Spring Boot 也可以从以下位置加载配置；优先级从高到低，高优先级配置内容覆盖低优先级配置内容，所有配置互补。

**1. 命令行参数**
**`java -jar file.jar --server.port=8087 --server.context-path=/abc`**
**多个配置用空格分开：`--配置项=值`**
 &lt /br &gt 2. 来自 `java:comp/env` 的 NDI属性
3. Java 系统属性 `System.getProperties()`
4. 操作系统环境变量
5. `RandomValuePropertySource` 配置的 `random.*` 属性值

**由 jar 包外向 jar 包内进行寻找；**
**优先加载带`profile`；**
**6. `jar` 包外部的 `application-{profile}.properties` 或 `application.yml` （带 `spring.profile`）配置文件**
**7. `jar` 包内部的 `application-{profile}.properties` 或 `application.yml` （带 `spring.profile`）配置文件**
  
**然后加载不带`Profile`；**
**8. `jar` 包外部的 `application.properties` 或 `application.yml` （不带 `spring.profile`）配置文件**
**9.  `jar` 包内部的 `application.properties` 或 `application.yml` （不带 `spring.profile`）配置文件**
 &lt /br &gt 10. `@Configuration` 注解类上的 `@PropertySource`
11. 通过 SpringApplication

# 自动配置原理

Spring Boot 启动的时候加载主配置类，开启了自动配置功能 `@EnableAutoConfiguration`

以**HttpEncodingAutoConfiguration**(Http编码自动配置)为例解释自动配置原理:

```java
@Configuration   //表示这是一个配置类，以前编写的配置文件一样，也可以给容器中添加组件
@EnableConfigurationProperties(HttpEncodingProperties.class)  //启动指定类的ConfigurationProperties功能；将配置文件中对应的值和HttpEncodingProperties绑定起来；并把HttpEncodingProperties加入到ioc容器中
@ConditionalOnWebApplication //Spring底层@Conditional注解（Spring注解版），根据不同的条件，如果满足指定的条件，整个配置类里面的配置就会生效；    判断当前应用是否是web应用，如果是，当前配置类生效
@ConditionalOnClass(CharacterEncodingFilter.class)  //判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；
@ConditionalOnProperty(prefix = "spring.http.encoding", value = "enabled", matchIfMissing = true)  //判断配置文件中是否存在某个配置  spring.http.encoding.enabled；如果不存在，判断也是成立的

//即使我们配置文件中不配置pring.http.encoding.enabled=true，也是默认生效的；

public class HttpEncodingAutoConfiguration {

  //他已经和SpringBoot的配置文件映射了
  private final HttpEncodingProperties properties;

  //只有一个有参构造器的情况下，参数的值就会从容器中拿
  public HttpEncodingAutoConfiguration(HttpEncodingProperties properties) {
  this.properties = properties;
  }

  @Bean   //给容器中添加一个组件，这个组件的某些值需要从properties中获取
  @ConditionalOnMissingBean(CharacterEncodingFilter.class) //判断容器没有这个组件
  public CharacterEncodingFilter characterEncodingFilter() {
    CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
    filter.setEncoding(this.properties.getCharset().name());
    filter.setForceRequestEncoding(this.properties.shouldForce(Type.REQUEST));
    filter.setForceResponseEncoding(this.properties.shouldForce(Type.RESPONSE));
    return filter;
}
```

所有在配置文件中能配置的属性都在 `xxxxProperties` 类中封装；配置文件能配置什么可以参照某个功能对应的该属性类：

```java
@ConfigurationProperties(prefix = "spring.http.encoding")  //从配置文件中获取指定的值和bean的属性进行绑定
public class HttpEncodingProperties {

  public static final Charset DEFAULT_CHARSET = Charset.forName("UTF-8");
```

可以查看 `HttpEncodingAutoConfiguration`

## 使用精髓

1. SpringBoot启动会加载大量的自动配置类

2. 确定我们需要的功能有没有SpringBoot默认写好的自动配置类

3. 再确定这个自动配置类中到底配置了哪些组件；（只要我们要用的组件有，就不需要重新配置了）

4. 给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们可以在配置文件中指定这些属性的值

## 自动配置类必须在一定的条件下才能生效

**可以通过启用  debug=true属性；来让控制台打印自动配置报告**，这样我们就可以很方便的知道哪些自动配置类生效；

# `@Conditional` 派生注解（原生的 `@Conditional` 作用）

```java
=========================
AUTO-CONFIGURATION REPORT
=========================

Positive matches: //（自动配置类启用的）
-----------------
  DispatcherServletAutoConfiguration matched:
    - @ConditionalOnClass found required class 'org.springframework.web.servlet.DispatcherServlet'; @ConditionalOnMissingClass did not find unwanted class (OnClassCondition)
    - @ConditionalOnWebApplication (required) found StandardServletEnvironment (OnWebApplicationCondition)
        
Negative matches: //（没有启动，没有匹配成功的自动配置类）
-----------------
  ActiveMQAutoConfiguration:
    Did not match:
      - @ConditionalOnClass did not find required classes 'javax.jms.ConnectionFactory', 'org.apache.activemq.ActiveMQConnectionFactory' (OnClassCondition)

  AopAutoConfiguration:
    Did not match:
      - @ConditionalOnClass did not find required classes 'org.aspectj.lang.annotation.Aspect', 'org.aspectj.lang.reflect.Advice' (OnClassCondition)
        
```

必须是 `@Conditional` 指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；

|||
| - |  - | 
| @Conditional扩展注解             | 作用（判断是否满足当前指定条件）               |
| @ConditionalOnJava              | 系统的java版本是否符合要求                |
| @ConditionalOnBean              | 容器中存在指定Bean；                   |
| @ConditionalOnMissingBean       | 容器中不存在指定Bean；                  |
| @ConditionalOnExpression        | 满足SpEL表达式指定                    |
| @ConditionalOnClass             | 系统中有指定的类                       |
| @ConditionalOnMissingClass      | 系统中没有指定的类                      |
| @ConditionalOnSingleCandidate   | 容器中只有一个指定的Bean，或者这个Bean是首选Bean |
| @ConditionalOnProperty          | 系统中指定的属性是否有指定的值                |
| @ConditionalOnResource          | 类路径下是否存在指定资源文件                 |
| @ConditionalOnWebApplication    | 当前是web环境                       |
| @ConditionalOnNotWebApplication | 当前不是web环境                      |
| @ConditionalOnJndi              | JNDI存在指定项                      |

# 更多

更多Spring Boot教程笔记见[教程笔记 - Spring Boot](/categories/教程笔记-Spring-Boot/)

# 参考资料

[尚硅谷SpringBoot顶尖教程(springboot之idea版spring boot)](https://www.bilibili.com/video/av20965295)