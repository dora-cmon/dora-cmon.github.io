---
title: Spring Boot使用Vue实现前后端分离(七)
categories:
  - 教程笔记 - Spring Boot
tags:
  - 教程
  - Spring Boot
cover: /images/cover/springboot.jpg
abbrlink: 71d4016a
---

前端通过 Ajax 请求来访问后端的数据库接口，将 Model 展示到 View 中。前后端开发者只需要提前约定好接口文档(URL，参数，数据类型...)，然后分别独立开发即可。

前端应用：负责数据展示和用户交互
后端应用：负责提供数据处理接口

# Vue 环境搭建

1. 安装 Node.js
   
    ```bash
    sudo apt update
    sudo apt install nodejs
    ```

    检查是否安装正确 `nodejs -v`：

    ```bash
    v8.10.0
    ```

2. 安装 cnpm

    ```bash
    sudo apt install npm
    sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
    ```
    cnpm 是国内镜像，安装完成后用 `cnpm` 代替默认的 `npm` 命令。在一个项目中不要将 `npm` 与 `cnpm` 混用，否则容易出现问题。

    检查是否安装成功 `cnpm -v`:

    ```bash
    /bin/sh: 1: npm: not found
    cnpm@6.1.1 (/usr/local/lib/node_modules/cnpm/lib/parse_argv.js)
    npm@6.14.2 (/usr/local/lib/node_modules/cnpm/node_modules/npm/lib/npm.js)
    node@8.10.0 (/usr/bin/node)
    npminstall@3.27.0 (/usr/local/lib/node_modules/cnpm/node_modules/npminstall/lib/index.js)
    prefix=null 
    linux x64 5.3.0-42-generic 
    registry=https://r.npm.taobao.org
    ```

3. 安装 vue 脚手架工具 vue-cli

    ```bash
    sudo cnpm install -g @vue/cli
    sudo cnpm install -g @vue/cli-init
    ```

    检查是否安装成功 `vue -V`:

    ```bash
    @vue/cli 4.2.3
    ```

    安装 `webpack`:

    ```bash
    sudo cnpm install -g webpack
    ```

# Spring Boot 后端构建

在 {% post_link Spring-Boot之使用JPA-六 %} 中代码的基础上进行构建。

##　修改部署端口

在 `application.yml` 中修改默认端口为 `8181`：

```yml
server: 
  port: 8181
```

## 配置跨域支持

新建配置类 `config/CrosConfig.java`：

```java
@Configuration
public class CrosConfig implements WebMvcConfigurer {

  @Override
  public void addCorsMappings(CorsRegistry registry) {
    registry.addMapping("/**")
            .allowedOrigins("*")
            .allowedMethods("GET", "HEAD", "POST", "PUT", "DELETE", "OPTIONS")
            .allowCredentials(true)
            .maxAge(3600)
            .allowedHeaders("*");
  }
}
```

运行 Spring Boot 项目，注意端口是否正确！

# Vue 前端构建

## 创建项目

在合适的目录下创建 Vue 项目：

```bash
sudo vue init webpack vue-demo
# 修改文件所有者为当前用户
sudo chown <user>:<group> -R vue-demo
```

出现提示直接回车默认，当出现以下选项时，

- 使用 ESLint 代码规范(no):

  ```bash
  ? Use ESLint to lint your code? (Y/n) n
  ```

- 执行 `npm install`(No，之后再使用 `cnpm`进行安装)：

  ```bash
  ? Should we run `npm install` for you after the project has been created? (recom
  mended) 
    Yes, use NPM 
    Yes, use Yarn 
  ❯ No, I will handle that myself
  ```

完成后进入项目目录，执行以下命令：

```bash
cd vue-demo
cnpm install
cnpm run dev
```

完成后，在浏览器打开 `localhost:8080` 即可看到主页。

![vue主页](/images/Spring-Boot使用Vue实现前后端分离-七/2020-03-31-18-05-22.png)

## 预备工作

在项目目录中执行以下命令安装依赖：

```bash
# 用于网络请求
vue add axios
# element 风格的 UI 框架
vue add element-ui
# 用于管理状态
cnpm install vuex --save
# 用于实现页面跳转
cnpm install vue-router --save
```

使用 VS Code 打开项目目录，安装插件 `Vetur`；

## 简单页面展示(一)

### 目录结构

```
vue-demo
└── src
    ├── App.vue
    ├── components
    │   ├── Account.vue
    │   └── HelloWorld.vue
    ├── main.js
    └── router
        └── index.js
```

### Account 页面(Account.vue)

```xml
<template>
  <div>
    <table>
      <tr>
        <td>序号</td>
        <td>姓名</td>
        <td>工资</td>
      </tr>

      <!-- 因为开启代码检查，所以必须写 :key 字段，本例中该字段无用 -->
      <tr v-for="item in accounts" :key="item.id">
        <td>{{item.id}}</td>
        <td>{{item.name}}</td>
        <td>{{item.money}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: 'Account',
  data () {
    return {
      msg: 'Hello vue',
      accounts: [] // 声明变量，前端获取该数据作展示
    }
  },
  created () {
    // 通过 this.accounts 访问变量
    const _this = this
    // 访问后端接口，并将返回值赋予 this.accounts
    axios.get('http://localhost:8181/account/list').then(function (resp) {
      _this.accounts = resp.data
    })
  }
}
</script>

<style>

</style>
```

### 添加路由(router/index.js)

部分默认字段省略

```js
// import ...
import Account from '@/components/Account'

Vue.use(Router)

export default new Router({
  mode: 'history', // 去除 url 中的 # 符号
  routes: [
    // ...
    {
      path: '/account',
      // name 字段可不写，省略
      component: Account
    }
  ]
})

```

### 访问效果

执行 `npm run dev`，然后在浏览器打开 `localhost:8080` 即可：

![简单页面](/images/Spring-Boot使用Vue实现前后端分离-七/2020-04-01-17-38-28.png)

## 搭建页面框架(二)

### 目录结构

```
vue-demo
└── src
    ├── App.vue
    ├── main.js
    ├── router
    │   └── index.js
    └── views
        ├── Index.vue
        ├── PageFour.vue
        ├── PageOne.vue
        ├── PageThree.vue
        └── PageTwo.vue
```

### 主容器(App.vue)

`<router-view/>` 是页面动态加载的位置

```xml
<template>
  <div id="app">
    <!-- 页面加载位置 -->
    <router-view/>
  </div>
</template>

<script>
export default {
}
</script>

<style>

</style>

```

### 创建分页面(PageOne.vue, PageTwo.vue, PageThree.vue, PageFour.vue)

以 `页面1` 为例，仅展示标题作测试，其他页面类推。

```html
<template>
  <h1>这是页面1</h1>
</template>

<script>
export default {

}
</script>

<style>

</style>
```

### 添加路由(router/index.js)

```js
// import ...
import PageOne from '@/views/PageOne'
import PageTwo from '@/views/PageTwo'
import PageThree from '@/views/PageThree'
import PageFour from '@/views/PageFour'
import Index from '@/views/Index'

Vue.use(Router)

export default new Router({
  mode: 'history', // 去除 url 中的 # 符号
  routes: [
    // 主菜单栏
    {
      path: '/',
      name: '导航1',
      component: Index,
      // sub-menu 对应的子页面
      children: [
        {
          path: '/pageOne',
          name: '页面1',
          component: PageOne
        },
        {
          path: '/pageTwo',
          name: '页面2',
          component: PageTwo
        }
      ]
    },
    {
      path: '/navigation',
      name: '导航2',
      component: Index,
      children: [
        {
          path: '/pageThree',
          name: '页面3',
          component: PageThree
        },
        {
          path: '/pageFour',
          name: '页面4',
          component: PageFour
        }
      ]
    }
  ]
})
```

### 添加主页面(views/Index.vue)

可以在 [element 官网 -> 组件 -> Container 布局容器](https://element.eleme.cn/#/zh-CN/component/container) 中选择合适的布局，然后修改代码。

本例以如下布局为基础进行修改：

![页面布局](/images/Spring-Boot使用Vue实现前后端分离-七/2020-04-01-17-20-26.png)

```xml
<template>
  <el-container style="height: 500px; border: 1px solid #eee">

      <!-- 侧边导航栏 -->
      <el-aside width="200px" style="background-color: rgb(238, 241, 246)">

        <!-- 菜单栏; default-openeds 配置默认打开的菜单-->
        <el-menu router :default-openeds="['0', '1']">
          <!-- 子菜单; 循环获取routers中的页面，并配置菜单项 -->
          <el-submenu v-for="(item, index) in $router.options.routes" :key="item"
           :index="index+''">
            <!-- <i>配置菜单图标</i>; item.name 获取路由配置中项目的 name -->
            <template slot="title"><i class="el-icon-setting"></i>{{item.name}}</template>
            <!-- 通过:index="item2.path" 将 index 指定为子页面路径，即可完成绑定 -->
            <!-- 访问路径与某项路径一致时，将其设为激活状态-->
            <el-menu-item v-for="item2 in item.children" :key="item2"
            :index="item2.path"
            :class="$route.path==item2.path?'is-active':''">{{item2.name}}</el-menu-item>
          </el-submenu>
        </el-menu>

      </el-aside>

      <el-container>

        <el-main>
          <!-- 页面动态加载位置 -->
          <router-view/>
        </el-main>

      </el-container>

    </el-container>
</template>

<script>
export default {

}
</script>

<style>

</style>
```

### 效果展示

执行 `npm run dev`，然后在浏览器打开 `localhost:8080` 即可：

![页面布局](/images/Spring-Boot使用Vue实现前后端分离-七/2020-04-01-17-45-40.png)

## 链接数据库并实现增删改查(三)


在 [element 官网 -> 组件 -> Table 表格](https://element.eleme.cn/#/zh-CN/component/table) 中选择合适的表格模板；

在 [element 官网 -> 组件 -> Pagination 分页](https://element.eleme.cn/#/zh-CN/component/pagination) 中选择合适的分页模板；

本例以如下表格和分页模板为基础进行修改：

![表格模板](/images/Spring-Boot使用Vue实现前后端分离-七/2020-04-01-17-51-10.png)

![分页模板](/images/Spring-Boot使用Vue实现前后端分离-七/2020-04-01-17-58-34.png)

## 修改后端，支持分页

修改 `AccountController.java` 中的 `getAccount`函数，在接口中加入 `page` 和 `size`：

```java
@GetMapping(value = "/list/{page}/{size}")
public Page<Account> getAccount(@PathVariable("page") Integer page,
                                @PathVariable("size") Integer size) {
    PageRequest request = PageRequest.of(page, size);
    return accountDao.findAll(request);
}
```

## 修改前端，分页展示数据

修改 `PageOne.vue`:

```xml

```






# 更多

更多Spring Boot教程笔记见[教程笔记 - Spring Boot](/categories/教程笔记-Spring-Boot/)

# 参考资料

[超详细的SpringBoot+Mybatis+Vue整合笔记](https://www.cnblogs.com/ggjun/p/10990215.html)

[Springboot Vue Login(从零开始实现Springboot+Vue登录)](https://blog.csdn.net/xiaojinlai123/article/details/90694372)

[Vue.js + Spring Boot 项目实战](https://learner.blog.csdn.net/column/info/36064)

[【2020版】4小时学会Spring Boot+Vue前后端分离开发](https://www.bilibili.com/video/BV137411B7vB)