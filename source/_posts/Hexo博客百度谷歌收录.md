---
title: Hexo博客百度谷歌收录
categories:
  - 环境搭建 - Hexo
tags:
  - Hexo
  - 码云
  - 博客
cover: /images/cover/Hexo.jpg
abbrlink: 3b6da06d
date: 2020-03-05 13:56:57
---


# 百度收录

## 添加网站

登录百度搜索资源平台，然后进入[站点管理](https://ziyuan.baidu.com/site)页面，进行身份验证，成功后点击添加网站按钮添加博客网址。

- 第一步：输入网站地址，如 https://dora_cmon.gitee.io

- 第二步：选择站点属性，最多可选三项，如影视动漫、信息技术等

- 第三步：验证网站的所有权

## 验证网站的所有权

验证网站有三种方式：文件验证、HTML标签验证、CNAME验证。任选其一即可，Butterfly 主题能够很方便的进行 HTML 标签验证。

### 文件验证

![](/images/Hexo博客百度谷歌收录/2020-03-05-12-55-24.png)

下载百度验证文件，放到 `themes/next/source` 文件夹下，（因为站点source下面的html文件都会被按照主题样式重新渲染，最后html文件的内容会被改变，百度验证就不能识别。）虽然可以更改html文件让主题不去重新渲染此html文件，但直接放到主题下的 `source` 文件夹而不去更改文件内容更方便。

**为保持验证通过的状态,成功验证后请不要删除HTML文件**

### HTML标签验证

将以下代码添加到您的网站首页HTML代码的 `<head>` 标签与 `</head>` 标签之间，完成操作后请点击“验证”按钮。

'''
<meta name="baidu-site-verification" content="c4z6ZbMZ8I" />
'''

**注意**：content这个值每个网站都是不一样的，要替换成你自己网站的值

```html
<html>
  <head>
    <meta name="baidu-site-verification" content="c4z6ZbMZ8I" />
    <title>My title</title>
  </head>
  <body>
    page contents
  </body>
</html>
```

**为保持验证通过的状态,成功验证后请不要删除该标签**

在 Butterfly 主题中，直接配置 `butterfly.yml` 文件中的 `baidu_site_verification` 字段为 `c4z6ZbMZ8I` 即可，非常方便（注意将随机值换成自己的）。

### CNAME验证

CNME验证的方法适用于博客已绑定域名的情况下，将 c4z6ZbMZ8I.dora_cmon.gitee.io 使用 CNAME 解析到 ziyuan.baidu.com，完成操作后请点击“完成验证”按钮。

**为保持验证通过的状态,成功验证后请不要删除该DNS记录**

## 链接提交

完成网站所有权验证后就可以向百度提交链接了，找到 `数据引入->链接提交` 进入提交页，链接提交分成自动提交和手动提交两种方式。

### 自动提交

自动提交分为三种：

- **主动推送**：通过主动调用百度提供的接口提交链接
  
- **自动推送**：在页面被访问时，页面URL将立即被推送给百度
  
- **sitemap**：sitemap文件里包含站点的所有页面地址，提供给搜索引擎的爬虫爬取

**主动推送**

安装插件 `sudo cnpm install hexo-baidu-url-submit --save` 

然后在根目录的配置文件 `_config.yml` 中新增字段：

```yaml
#设置百度主动推送
baidu_url_submit:
  count: 100  #比如200，代表提交最新的200个链接
  host: dora_cmon.gitee.io # 在百度站长平台中注册的域名，这个改为你自己的域名
  token: your_token # 请注意这是您的秘钥， 所以请不要把博客源代码发布在公众仓库里!
  path: baidu_urls.txt # 文本文档的地址， 新链接会保存在此文本文档里，这个默认
```

token 从站长平台获取：

![](/images/Hexo博客百度谷歌收录/2020-03-19-16-11-42.png)

再配置 `deploy` 项：

```yaml
deploy:
- type: git
  repo: git@gitee.com:dora_cmon/dora_cmon.git
  branch: master
- type: baidu_url_submitter
```
在执行 `hexo d` 时，新的连接会自动向百度推送。

![](../images/Hexo博客百度谷歌收录/2020-03-19-16-12-55.png)

**自动推送**

在 Butterfly 主题中，直接配置 `butterfly.yml` 文件中的 `baidu_push` 字段为 `true` 即可。

在其他主题中，除以上设置外，还需要进入站点根目录 `\themes\next\layout\_scripts` 目录，修改 `baidu_push.swig` 文件为以下内容，若无该文件直接创建一个：

```javascript
{% if theme.baidu_push %}
<script>
(function(){
    var bp = document.createElement('script');
    var curProtocol = window.location.protocol.split(':')[0];
    if (curProtocol === 'https') {
        bp.src = 'https://zz.bdstatic.com/linksubmit/push.js';
    }
    else {
        bp.src = 'http://push.zhanzhang.baidu.com/push.js';
    }
    var s = document.getElementsByTagName("script")[0];
    s.parentNode.insertBefore(bp, s);
})();
</script>
{% endif %}
```

重新编译生成部署即可，这样每次访问一个页面都会主动把这个页面的地址提交给百度。

访问博客任一页面，然后按F12，切换到Elements，如果看到如下代码说明以上js代码插入成功：

![](/images/Hexo博客百度谷歌收录/2020-03-05-13-18-52.png)

**sitemap**

安装sitemap生成器插件：

```bash
sudo cnpm install hexo-generator-sitemap --save
sudo cnpm install hexo-generator-baidu-sitemap --save
```

然后修改站点配置文件 `_config.yml`，将 `url` 改成博客的地址：

```yaml
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://dora_cmon.gitee.io
root: /
```

然后在最后添加以下字段：

```yaml
# 自动生成sitemap
sitemap:
  path: sitemap.xml
baidusitemap:
  path: baidusitemap.xml
```
重新生成部署，会在public目录下生成sitemap.xml、baidusitemap.xml两个文件。

此时访问域名下的这两个文件（以我为例）：

- https://dora_cmon.gitee.io/baidusitemap.xml
- https://dora_cmon.gitee.io/sitemap.xml

![](/images/Hexo博客百度谷歌收录/2020-03-05-13-28-21.png)

最后在 `自动提交->sitemap` 页面填入sitemap的地址并提交：

![](/images/Hexo博客百度谷歌收录/2020-03-05-13-29-35.png)


# 谷歌收录

收录逻辑与百度大同小异，以下做简要记录。

进入[Google站点提交入口](https://www.google.com/webmasters/tools/home?hl=zh-CN)，提交网址：

![](/images/Hexo博客百度谷歌收录/2020-03-05-13-36-12.png)

验证网站所有权：

![](/images/Hexo博客百度谷歌收录/2020-03-05-13-37-21.png)

若为 Butterfly 主题，修改 `butterfly.yml` 配置文件中的 `google_site_verification` 字段即可。

![](/images/Hexo博客百度谷歌收录/2020-03-05-13-41-27.png)

**链接提交**：

- sitemap
  
  在 `站点地图` 页面输入之前产生的站点地图链接 `sitemap.xml`，提交即可：

![](/images/Hexo博客百度谷歌收录/2020-03-05-13-43-54.png)

# 更多

参考[环境搭建-Hexo](/categories/环境搭建-Hexo/)

# 参考资料

[Hexo系列：（四）Hexo博客提交百度和Google收录](https://www.jianshu.com/p/7d3d87b52ad7)

[Hexo博客百度收录](https://blog.csdn.net/mqdxiaoxiao/article/details/93378785)

[hexo-theme-butterfly安裝文檔](https://jerryc.me/posts/21cfbf15/#%E7%B6%B2%E7%AB%99%E9%A9%97%E8%AD%89)