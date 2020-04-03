---
title: Spring Boot使用JDBC访问MySQL(五)
categories:
  - 教程笔记 - Spring Boot
tags:
  - 教程
  - Spring Boot
cover: /images/cover/springboot.jpg
abbrlink: 6591177e
date: 2020-03-30 16:38:59
---


# 安装 MySQL

## Ubuntu

```bash
sudo apt install mysql-server
sudo apt install mysql-client
sudo apt install libmysqlclient-dev
```


## 检查安装

```bash
sudo netstat -tap |grep mysql
```

出现以下行，则表示 mysql 服务已启动：

```
tcp     0   0   localhost:mysql     0.0.0.0:*   LISTEN  10165/mysqld
```

## 创建数据库表

- 登录数据库

    ```bash
    # 若 root 账户无密码
    mysql -uroot
    # 若 root 账户有密码
    mysql -uroot -p<your_passwd>
    ```

- 修改数据库账户密码（给 `root` 本地用户设置密码）

    ```sql
    mysql> set password for root@localhost=password("111111");
    ```

- 创建数据库
    
    ```sql
    create database SpringBootLearn;
    use SpringBootLearn;
    ```

- 创建表

    ```sql
    CREATE TABLE account (
        id int(11) NOT NULL AUTO_INCREMENT,
        name varchar(20) NOT NULL,
        money double DEFAULT NULL,
        PRIMARY KEY (id)
    )ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;

    INSERT INTO account VALUES ('1', 'Zhao', '100');
    INSERT INTO account VALUES ('2', 'Qian', '1000');
    INSERT INTO account VALUES ('3', 'Sun', '10000');
    ```

- 查看表
  
    ```sql
    SELECT * FROM account;
    ```

    ```
    +----+------+-------+
    | id | name | money |
    +----+------+-------+
    |  1 | Zhao |   100 |
    |  2 | Qian |  1000 |
    |  3 | Sun  | 10000 |
    +----+------+-------+
    ```

# 代码编写

## 创建工程

按`Ctrl + Shift + P`，输入 `Spring`，选择 `Spring Initializer`，然后选择 `java`；

`Group Id` 默认；
`Artifact Id` 输入`spring-boot-jdbc`；
`Spring Boot version` 选择 `2.2.x`。

添加依赖库：
- Spring Boot DevTools
- Spring Web
- Lombok
- JDBC API
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

## application.properties

```yml
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/SpringBootLearn
spring.datasource.username=root
spring.datasource.password=111111
```

## 实体类(Account.java)

注意如果没有引入 `Lombok` 插件，则需手动编写 `setter, getter`。

```java
@Data
public class Account {

    private int id;
    private String name;
    private double money;
}
```

## dao 层(IAccountDAO.java, AccountDaoImple.java)

```java
// IAccountDAO.java
public interface IAccountDAO {
    int add(Account account);

    int update(Account account);

    int delete(int id);

    Account findAccountById(int id);

    List<Account> findAccountList();
}
```

```java
//AccountDaoImple.java
@Repository
public class AccountDaoImpl implements IAccountDAO {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public int add(final Account account) {
        return jdbcTemplate.update("INSERT into account(name, money) values(?, ?)", 
                                    account.getName(), account.getMoney());
    }

    @Override
    public int update(final Account account) {
        return jdbcTemplate.update("UPDATE account SET name=?, money=? WHERE id=?",
                                    account.getName(), account.getMoney(), account.getId());
    }

    @Override
    public int delete(final int id) {
        return jdbcTemplate.update("DELETE from TABLE account where id=?", id);
    }

    @Override
    public Account findAccountById(final int id) {
        final List<Account> list = jdbcTemplate.query("SELECT * from account WHERE id=?",
                                new Object[]{id}, new BeanPropertyRowMapper(Account.class));
        if(list != null && list.size() > 0){
            final Account account = list.get(0);
            return account;
        }else{
            return null;
        }
    }

    @Override
    public List<Account> findAccountList() {
        final List<Account> list = jdbcTemplate.query("SELECT * from account", 
                                new Object[]{}, new BeanPropertyRowMapper(Account.class));
        if(list!=null && list.size()>0){
            return list;
        }else{
            return null;
        }
    }
}
```

## service 层(IAccountService.java, AccountServiceImpl.java)

```java
//IAccountService.java
public interface IAccountService {

    int add(Account account);

    int update(Account account);

    int delete(int id);

    Account findAccountById(int id);

    List<Account> findAccountList();
}
```

```java
//AccountServiceImple.java
@Service
public class AccountServiceImpl implements IAccountService{

    @Autowired
    IAccountDAO accountDAO;
    @Override
    public int add(Account account) {
        return accountDAO.add(account);
    }

    @Override
    public int update(Account account) {
        return accountDAO.update(account);
    }

    @Override
    public int delete(int id) {
        return accountDAO.delete(id);
    }

    @Override
    public Account findAccountById(int id) {
        return accountDAO.findAccountById(id);
    }

    @Override
    public List<Account> findAccountList() {
        return accountDAO.findAccountList();
    }
}
```

## 控制器(AccountController.java)

```java
@RestController
@RequestMapping("/account")
public class AccountController {

    @Autowired
    IAccountService accountService;

    @GetMapping(value="/list")
    public List<Account> getAccounts() {
        return accountService.findAccountList();
    }

    @GetMapping("/{id}")
    public Account getAccountById(@PathVariable("id") int id) {
        return accountService.findAccountById(id);
    }

    @PutMapping("/{id}")
    public  String updateAccount(@PathVariable("id")int id , 
                                 @RequestParam(value = "name",required = true)String name,
                                 @RequestParam(value = "money" ,required = true)double money){
        Account account=new Account();
        account.setMoney(money);
        account.setName(name);
        account.setId(id);
        int t=accountService.update(account);
        if(t==1){
            return account.toString();
        }else {
            return "fail";
        }
    }

    @PostMapping("")
    public  String postAccount(@RequestParam(value = "name")String name,
                               @RequestParam(value = "money" )double money){
        Account account=new Account();
        account.setMoney(money);
        account.setName(name);
        int t= accountService.add(account);
        if(t==1){
            return account.toString();
        }else {
            return "fail";
        }
    }
}
```

# 测试代码

使用 [Postman](https://www.postman.com/downloads/)进行 `get, post, put` 等测试：

![测试结果](/images/Spring-Boot使用JDBC访问MySQL-五/2020-03-30-16-38-05.png)

# 更多

更多Spring Boot教程笔记见[教程笔记 - Spring Boot](/categories/教程笔记-Spring-Boot/)

# 参考资料

[Spring Boot教程第3篇：JDBCTemplate](https://www.fangzhipeng.com/springboot/2017/05/03/sb3-Jdbc.html)

[Ubuntu上mysql的安装及使用(通用版)](https://www.jb51.net/article/157282.htm)

[Spring boot 项目目录结构](https://blog.csdn.net/u012675150/article/details/79351990)