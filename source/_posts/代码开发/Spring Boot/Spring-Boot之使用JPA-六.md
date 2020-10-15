---
title: Spring Boot之使用JPA(六)
categories:
  - 代码开发 - Spring Boot
tags:
  - 教程
  - Spring Boot
cover: /images/cover/springboot.jpg
abbrlink: eafa9b97
date: 2020-03-30 19:32:56
---


JPA是Java Persistence API的简称，中文名Java持久层API，是JDK 5.0注解或XML描述对象－关系表的映射关系，并将运行期的实体对象持久化到数据库中。

JPA 的目标之一是制定一个可以由很多供应商实现的API，并且开发人员可以编码来实现该API，而不是使用私有供应商特有的API。


# 数据库安装、创建

参考 {% post_link Spring-Boot使用JDBC访问MySQL-五 %}

# 代码编写

## 创建工程

按`Ctrl + Shift + P`，输入 `Spring`，选择 `Spring Initializer`，然后选择 `java`；

`Group Id` 默认；
`Artifact Id` 输入`spring-boot-jpa`；
`Spring Boot version` 选择 `2.2.x`。

添加依赖库：
- Spring Boot DevTools
- Spring Web
- Lombok
- Spring Data JPA
- MySQL Driver

## 目录结构

```
src.main
    ├── java.com.example.springbootjdbc
    │   ├── controller
    │   │   └── AccountController.java
    │   ├── DemoApplication.java
    │   ├── domain
    │   │   └── Account.java
    │   ├── repository
    │   │   ├── AccountDaoImpl.java
    │   │   └── IAccountDAO.java
    │   └── service
    │       ├── IAccountService.java
    │       └── impl
    │           └── AccountServiceImpl.java
    └── resources
        └── application.properties

```

## application.yml

删除 `application.properties` 使用 YML 配置文件：

```yml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/SpringBootLearn?useUnicode=true&characterEncoding=utf8&characterSetResults=utf8
    username: root
    password: 111111

  jpa:
    show-sql: true
    properties:
      hibernate:
      format_sql: true
```

## 实体类(Account.java)

通过 `@Entity` 注解，表明该类是一个映射的实体类, `@Id` 表示主键:

```java
@Data
@Entity
public class Account {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String name;
    private double money;
}
```

## dao 层(AccountDao.java)

编写一个继承自 JpaRepository 的接口就能完成数据访问，不用编写额外代码。其中包含了几本的单表查询的方法，非常方便。

其中，
- Account 是对象名，而不是具体的表名
- Interger 是主键的类型，一般为 `Integer` 或者 `Long`。

```java
public interface AccountDao  extends JpaRepository<Account,Integer> {
}
```

## 控制器(AccountController.java)

在本例中没有编写 service 层(直接在控制器中实现)，在实际开发中，不可省略。

```java
@RestController
@RequestMapping("/account")
public class AccountController {

    @Autowired
    AccountDao accountDao;

    @GetMapping(value = "/list")
    public List<Account> getAccount() {
        return accountDao.findAll();
    }

    @GetMapping(value = "/{id}")
    public Optional<Account> getAccountById(@PathVariable("id") int id) {
        return accountDao.findById(id);
    }

    @PutMapping(value = "/{id}")
    public String updateAccount(@PathVariable("id") int id, @RequestParam(value = "name", required = true) String name,
                                @RequestParam(value = "money", required = true) double money) {
        Account account = new Account();
        account.setMoney(money);
        account.setName(name);
        account.setId(id);
        Account account1 = accountDao.saveAndFlush(account);

        return account1.toString();

    }

    @PostMapping(value = "")
    public String postAccount(@RequestParam(value = "name") String name,
                              @RequestParam(value = "money") double money) {
        Account account = new Account();
        account.setMoney(money);
        account.setName(name);
        Account account1 = accountDao.save(account);
        return account1.toString();
    }
}
```

# 测试代码

使用 [Postman](https://www.postman.com/downloads/)进行 `get, post, put` 等测试。


# 更多

更多Spring Boot教程笔记见[代码开发 - Spring Boot](/categories/代码开发-Spring-Boot/)

# 参考资料

[JPA_百度百科](https://baike.baidu.com/item/JPA/5660672?fr=aladdin)

[Spring Boot教程第4篇：JPA](https://www.fangzhipeng.com/springboot/2017/05/04/sb4-jpaJ.html)

[Spring boot 项目目录结构](https://blog.csdn.net/u012675150/article/details/79351990)