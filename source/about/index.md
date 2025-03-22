---
title: 关于本hexo主题
date: 2020-07-10 11:42:18
---

主题仓库：https://github.com/hdxw/hexo-theme-prowiki

在线示例：<a href="https://demo.wujiaxing.com/prowiki" target="_blank">https://demo.wujiaxing.com/prowiki</a>

示例源码：https://github.com/hdxw/hexo-theme-prowiki-demo

自建插件：https://github.com/hdxw/hexo-font-spider

## 安装与配置

傻瓜教程，操作环境搭建参考hexo官方文档：https://hexo.io/zh-cn/docs/

### 新建hexo项目

初始化项目

```bash
hexo init hexo-theme-prowiki-demo
```

复制prowiki到`[hexo-site]/themes/`中，并修改`[hexo-site]/_config.yml`的配置如下

```yml
highlight:
  hljs: true # 修改为true，开启代码高亮

theme: prowiki # 修改主题为prowiki，值为文件夹名
```

移动主题配置文件`[hexo-site]/themes/prowiki/_config.yml`到根目录并重命名为`[hexo-site]/_config.prowiki.yml`，因为主题内置的scripts插件需要读取`hexo.config.theme_config`值来动态生成和渲染部分页面

最后安装依赖，安装完成就能启动简单的hexo项目了

```bash
cd hexo-theme-prowiki-demo
npm install
hexo s
```

Hexo文档：[使用代替主题配置文件](https://hexo.io/zh-cn/docs/configuration.html#%E4%BD%BF%E7%94%A8%E4%BB%A3%E6%9B%BF%E4%B8%BB%E9%A2%98%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)

### hexo版本更新

ProWiki-v2.0.0使用hexo版本为7.3.0，可以通过如下命令更新

```bash
# nodejs，根据电脑系统环境，选择最新LTS版本并下载安装
https://nodejs.org/zh-cn/download/

# npm
npm install -g npm

# hexo-cli
npm i hexo-cli -g

# 切换到项目目录，更新package.json
npm install -g npm-check
npm-check
npm install -g npm-upgrade
npm-upgrade

# 更新全局插件
npm update -g
# 更新系统插件
npm update --save
```

注意修改`[hexo-site]/package.json`中的hexo版本号

### 自定义导航

首先需要编辑导航相关配置

默认配置只显示“首页”导航，如果想自定义导航栏可以自行编辑

```yml [hexo-site]/_config.prowiki.yml
menus: 
  首页: /
  归档: archives
  分类: categories
  标签: tags
  友链: friends
  关于: about
```

其中`archives`、`categories`、`tags`的导航文字描述可自由编辑，访问路径读取的是根目录配置文件中的配置项

```yml [hexo-site]/_config.yml
# Directory
tag_dir: tags
archive_dir: archives
category_dir: categories
```

#### 开启归档、分类、标签导航

以上导航的数据分页插件均为官方插件，通过`hexo init`生成的默认模板，`package.json`中的依赖信息已包含相关插件，不需额外添加，如需手动添加插件可通过如下命令

```bash
npm install hexo-generator-archive --save # 归档
npm install hexo-generator-category --save # 分类
npm install hexo-generator-tag --save # 标签
```

以上相关分页插件的配置信息，需手动添加到根目录配置文件中，具体参数及解释如下

```yml [hexo-site]/_config.yml
# 插件hexo-generator-archive的分页配置
archive_generator:
  enabled: true # 是否启用插件
  per_page: 10 # 每页文章数，设置为0时不分页
  yearly: true # 生成按年份的归档
  monthly: true # 生成按月份的归档
  daily: true # 生成按天的归档
  order_by: -date # 默认按日期降序排序，不要修改

# 插件hexo-generator-category的分页配置
category_generator:
  per_page: 10 # 每页文章数，设置为0时不分页
  order_by: -date

# 插件hexo-generator-tag的分页配置
tag_generator:
  per_page: 10 # 每页文章数，设置为0时不分页
  order_by: -date
  enable_index_page: true # 是否生成:tag_dir/index.html，layout顺序依次是`tag-index`, `tag`, `archive`, `index`
```

页面的布局、样式由“主题”的模板提供，以下是主题配置文件相关配置项

```yml [hexo-site]/_config.prowiki.yml
tag:
  bubble_same_color: false # 标签首页气泡图，气泡背景色是否相同
```

#### 开启友链导航

编辑主题配置文件

```yml
friends:
  enable: true # 启用url: /friends，默认关闭
  path: friends # 访问路径，没有/，可修改
  groups: # 使用_config.prowiki.yml时会取并集，优先级_config.prowiki.yml大于themes/prowiki/_config.yml
    - group: 搜索引擎
      items:
        - name: 百度
          url: https://www.baidu.com
    - group: hexo主题
      items:
        - name: ProWiki
          avatar: img/abaaba.png
          url: https://github.com/hdxw/hexo-theme-prowiki
    - items:
      - name: 无分组链接
        url: .
```

如果想在下面添加说明信息，可以通过`hexo new page friends`命令新建页面，然后编辑`source/friends/index.md`文件

#### 开启关于导航

“关于”导航没有独立的页面渲染模板或解析，实际是hexo命令生成的page页面，page和post是一样的，区别是page有单独的根目录url。

新建页面(page)命令如下，其中“about”可改变

```bash
hexo new page "about"
```

然后编辑`source/about/index.md`内容即可。

#### 开启搜索功能

编辑主题配置文件

```yml
actions:
  search_enable: true # 改为true开启搜索

# 搜索配置
localsearch:
  unescape: true # 字符串是否转义（解码）
  top_n_per_article: 1 # 每篇文章匹配几次
  auto_trigger: true # 输入内容改变时，是否自动显示搜索结果。false时需要按回车或者放大镜图标进行搜索
```

按照说明安装`hexo-generator-searchdb`插件并添加根目录配置信息，具体信息见下方“建议插件”

### 建议插件

#### hexo-abbrlink

功能作用：文章自动生成hash链接，可以规范和缩短链接长度，防止文章url中出现中文编码等情况

```bash
npm install hexo-abbrlink --save
```

根目录配置文件编辑如下信息

```yml [hexo-site]/_config.yml
# 永久链接路径格式
permalink: posts/:abbrlink/ 
# 或
permalink: posts/:abbrlink.html

abbrlink:
  alg: crc32 # 算法：crc16、crc32
  rep: hex # dec(十进制) hex(十六进制)
  drafts: false # 是否为草稿生成
  force: false # 是否覆盖文章已有abbrlink重新生成
```

已有的文章可以`hexo s`启动服务后，打开并编辑文章再<kbd>Ctrl+S</kbd>保存自动生成一下

#### hexo-blog-encrypt

功能作用：文章内容加密

```bash
npm install hexo-blog-encrypt --save
```

根目录配置文件添加如下信息

```yml [hexo-site]/_config.yml
# Security
encrypt: # hexo-blog-encrypt
  abstract: 有东西被加密了, 请输入密码查看.
  message: 您好, 这里需要密码.
  tags:
  - {name: hello, password: hello}
  theme: default # 加密组件主题样式，支持的类型有：[default, blink, flip, shrink, surge, up, wave, xray]
  wrong_pass_message: 抱歉, 这个密码看着不太对, 请再试试.
  wrong_hash_message: 抱歉, 这个文章不能被校验, 不过您还是能看看解密后的内容.
```

可以在配置信息里按照文章标签批量设置密码，同时匹配多个标签密码时，会取按字母顺序排序第一的标签密码。

也可以在文章内容文件头添加`password`配置值，在此的配置项优先级最高，当设置`password: ""`时文章不加密。

#### hexo-generator-searchdb

功能作用：自动生成搜索数据文件

```bash
npm install hexo-generator-searchdb --save
```

根目录配置文件添加如下信息

```yml [hexo-site]/_config.yml
search:
  path: search.xml # 支持.xml和.json两种格式
  field: post # post：仅包含所有_posts目录下文章；page：仅包含hexo new page xxx生成的页面；all：包含所有以上两项
  content: true # 会否包含文章内容，如果为false则只包含文章标题的元数据
  format: html # 文章内容的格式。html/striptags(去掉所有html标签)/raw(原始markdown内容)
```

#### hexo-font-spider

功能作用：命令行插件，安装后可全局执行，用于自动检测并缩减自定义字体文件的大小

```bash
npm install hexo-font-spider --save
```

安装后可使用命令行命令`hexo fc`，使用方法

```bash
$ hexo generate
# 在之后执行如下命令
$ hexo fc
```

### 编辑网站配置

#### 配置文件

主题中的配置文件很通俗易懂，还有详细的注释，这里说一下根目录配置文件中的一些配置项，详细解析参考[官方配置文档](https://hexo.io/zh-cn/docs/configuration.html)

```yml
# 网站基础配置信息（一般在head里）
title: Hexo   #这个是站点名称会显示在顶部导航栏、网站底部
subtitle: ''
description: '' # 站点描述信息
keywords: # 站点内容关键词
author: 阿巴阿巴 # 站点作者，仅在网页meta中使用
language: zh-CN # themes/主题文件夹/languages中的文件名
timezone: 'Asia/Shanghai'

url: https://demo.wujiaxing.com/prowiki # 部署的网站的域名，会影响文章底部的版权信息
root: /prowiki/ # 部署时不是在域名的根目录，填写对应的子目录名
permalink: :year/:month/:day/:abbrlink.html # 文章永久链接格式

# 全局目录配置，建议保持默认值
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories

new_post_name: :title.md # 新建生成posts的文件名
default_layout: post # 文章内容默认模板文件名

highlight:
  line_number: true # 是否显示代码行号
  hljs: true # 是否开启代码高亮，建议开启

# 主页的分页信息
index_generator:
  path: ''
  per_page: 15
  order_by: -date

# archive_generator/category_generator/tag_generator在上面的“开启归档、分类、标签导航”中，这里不再重复

default_category: 未分类 # 文章无分类时自动添加的分类名称，需要hexo s后Ctrl+S一下
category_map: # 分类名的映射，可以把分类名映射为其他字符，主要用于去掉url路径的中文
  未分类: wfl
tag_map: # 类似上面，是标签名的映射
  测试标签1: test1
  测试标签2: test2

# 分页配置，一般是generator插件在调用
per_page: 10 # 值为0表示不分页
pagination_dir: page # 分页页面目录

# include/exclude配置处理或忽略'source/'目录的文件/文件夹，ignore会应用到所有文件夹
include:
exclude:
ignore:

theme: prowiki # 主题文件夹名

# 支持hexo deploy自动上传代码，需要安装插件npm install hexo-deployer-git --save
deploy:
  type: git
  repository: ''
  branch: master
```

#### 静态文件

在网站编译时，`[hexo-site]/source/`目录内，除*.md文件外的文件、文件夹，都会自动复制一份到public目录。

所以一些静态文件可以直接放在source目录下。

比如网站logo文件`favicon.ico`、爬虫规则文件`robots.txt`、文章需要引用的图片文件等。

#### 默认文章模板

`[hexo-site]/scaffolds/`目录下是hexo的默认模板，里面的文件就是用命令生成的文章、页面、草稿的初始内容。

生成命令如下

```bash
# 生成文章
hexo new 'post-title'
hexo new post 'post-title'
# 生成页面
hexo new page 'page-path'
# 生成草稿
hexo new draft 'draft-title'
```

可以通过编辑模板文件内容，简化每次新建时需要复制粘贴的工作

```markdown
---
title: {{ title }}
date: {{ date }}
tags:
categories:
author:
password: ""
---

```

### BUG及报错

1. ~~自动生成的文章“Hello World”中代码样式有问题是因为~~ hexo更新后已没问题

```plaintext
```和bash中间不能有空格
```

2. ~~代码复制按钮占用代码宽度，代码块出现横向滚动条时比较明显。~~ 已用position:absolute修正

3. ~~高亮代码，横向滚动条撑高元素高度~~ 布局方式不影响代码对齐，中间边框线使用撑高元素的边框

4. ~~点浏览器的刷新，自定义字体加载会闪烁~~ 在@font-face中设置font-display: block

## 更新日志

### feature-v2.1(更新计划)

> - 文章支持打赏/评论/分享、访客统计
> - 自定义配色方案、切换主题样式
> - 支持RSS，并添加header`<link rel="alternate" href="path/of/rss" type="application/atom+xml">`

### release-v2.0

很久没更新了，hexo、插件都升级了好几个版本，本主题也更新一下，顺便进行一些优化

> 1. 框架环境版本更新
>
> | 组件                  | 旧版本  | 新版本   |
> | --------------------- | ------- | -------- |
> | nodejs                | -       | v22.14.0 |
> | hexo                  | v4.2.1  | v7.3.0   |
> | fontawesome[图标字体] | v5.13.1 | v6.7.2   |
> | clipboard.js          | v2.0.6  | v2.0.11  |
>
> 2. 全部代码重构优化，局部内容样式微调
>
>    - 代码结构优化，移除重复css，采用更合理的页面布局方式，重写部分有问题代码
>    - logo版本号角标移动到footer主题名上，logo文字改用css3动画动态渐变色
>    - 更新“关于”文档内容
>    - 适配hexo-blog-encrypt不同theme样式
>    - 主色不变，部分颜色搭配微调，一些html标签样式微调
>    - css样式初步兼容手机端
>    - 所有页面支持“返回顶部”按钮
>    - 分类首页样式更新，明确层级归属
>    - 标签首页由tagcloud改为highcharts气泡图
>    - 使用helper扩展自定义函数拼接网页的title
>
> 3. 支持插件hexo-generator-searchdb及相关站内搜索功能
> 4. 创建插件hexo-font-spider，解决自定义字体文件过大加载缓慢的问题
>    - https://github.com/hdxw/hexo-font-spider
>    - https://github.com/aui/font-spider
> 5. 支持在单个文章头部添加版权声明的开关
>    ```markdown
---
title: no copyright
copyright: false // 默认开启，无需添加
---
post content
```
> 

### release-v1.2

因为没什么改动的需求和想法，长时间未更新

> 1.优化作者头像垂直对齐
> 2.支持下拉菜单
> 3.更改demo域名：https://demo.wujiaxing.com/prowiki/
> 4.修改favicon.ico路径，/img/favicon.ico => /favicon.ico
> 5.更新普惠体字体文件puhuiti.ttf => puhuiti3.woff2

### release-v1.1

修bug，加功能

> 1.修复加密文章中table/img样式问题，img点击不放大问题
> 2.fix配置信息。默认作者为主题中配置的第一个，网页header添加根目录配置中的站点信息
> 3.顶部菜单支持新标签页跳转
> 4.文章右侧目录显示当前所处位置，并随着页面自动变化、移动
> 5.代码块，添加一键复制功能

### release-v1.0

第一个正式发布版本

> 1.适配hexo-theme-unit-test所有样式
> 2.调整发现的其余css样式问题
> 3.支持图片查看大图

### develop-v0.9

> 1.完成基础页面内容展示，基础样式初设
> 2.支持创建：分类、标签、友链页面
> 3.预设兼容hexo-blog-encrypt插件
> 4.为了方便，简化掉了首页中文章开头可有可无的摘要

## 写在最后

下面是一些没用的话

### 创建主题目的

想写一些类似wiki、说明书、操作手册之类有较强类别区分的说明性文章，模板要支持多功能分类，还要卸去浮华，不要一切花里胡哨。
官方主题库里没有找到合适的，一时手贱就随便写了这个。
作为一个非专业的业余前端开发，自己做的再丑也要以为妙极。

### 取个名字吧

原本想写一些web漏洞、软件破解、硬件利用的文章所以以黑色为主色调，示例博客名字就叫
HTW：hack the world

wiki的名字被占用了，所以添字母后主题就叫 ProWiki ，主题全名：hexo-theme-prowiki

### 需求分析

1. 强化分类和标签的使用
2. 文章内标题有锚点
3. 风格简约，文字清晰，代码舒适

### 开发笔记

![](images/about.png)

1. hexo实际上只有三个默认主页面：index.ejs(首页列表)、post.ejs(文章内容)、archive.ejs(归档)，新建page默认解析到post.ejs(default_layout配置项)

2. 创建根目录自定义页面方法
   - 方法一：先`hexo new page "page-path"`，然后在themes/[theme-name]/layout根目录下添加page-path.ejs，最后在post.ejs中添加path判断，并解析page-path/index.html到page-path.ejs
   - 方法二：创建themes/[theme-name]/scripts/any-name.js脚本，在其中注册解析路径，详细内容见[hexo api文档](https://hexo.io/zh-cn/api/generator)。然后创建themes/[theme-name]/layout/friend.ejs，即可编辑自定义页面
     ```js
     hexo.extend.generator.register("friend", function (locals) {
      return {
        path: this.config.theme_config.friends.path+"/index.html",
        data: locals,
        layout: ["friend"],
      };
     });
     ```

3. 页面参数

```json
{
  "site": { # 全站信息
    "posts": [], # 所有文章
    "pages": [], # 所有分页
    "categories": [], # 所有分类
    "tags": [], # 所有标签
  },
  "page": { # 页面信息
    # -------------------分割线，以下是通用变量
    "title": "",
    "date": "",
    "updated": "",
    "comments": true,
    "layout": "",
    "content": "",
    "excerpt": "",
    "more": "",
    "source": "",
    "full_source": "",
    "path": "",
    "permalink": "",
    "prev": "",
    "next": "",
    "raw": "",
    "photos": [],
    "link": "",
    # -------------------分割线，以下是post.ejs专属变量
    "published": true,
    "categories": [],
    "tags": [],
    # -------------------分割线，以下是index.ejs专属变量
    "per_page": 15,      # 每页显示的文章数量
    "total": 2,          # 总页数
    "current": 1,        # 当前页数
    "current_url": "",
    "posts": [],         # 本页文章
    "prev": 0,           # 上一页的页数。如果此页是第一页的话则为 0
    "prev_link": "",
    "next": 2,           # 下一页的页数。如果此页是最后一页的话则为 0
    "next_link": "",
    "path": "",          # 当前页面的路径（不含根目录）。我们通常在主题中使用 url_for(page.path)
    # -------------------分割线，以下是archive.ejs专属变量
    "archive": true,
    "year": 2020,
    "month": 3,
    # -------------------分割线，以下是category.ejs专属变量
    "category": "",
    # -------------------分割线，以下是tag.ejs专属变量
    "tag": "",
  },
  "config": { # hexo项目配置文件，_config.yml
    "title": "",
    "author": "",
    "description": "",
    ...
  },
  "theme": {}, # 主题配置文件，themes/prowiki/_config.yml
  "path": "",
  "url": "",
  "env": "",
}
```

4. ejs模板

```html
<%- content %> // 解析成html
<%= content %> // 解析成字符串
<%- partial('_widget/post_title', {post:page}) %> // 子页面传参
```

5. css单位

```css
body{
  width: 100px; /* pixel像素的缩写，基于屏幕分辨率，如铺满1920 x 1080屏幕就是1920px x 1080px */
  width: 100%; /* 百分比，一般来说是相对于父元素。
                  position: absolute;的元素是相对于已定位的父元素；
                  position: fixed;的元素是相对于viewport */
  width: 100vw; /* 相对视口(viewport：浏览器可视窗口)的宽度而定的，1vw长度等于视口宽度的1/100 */
  
  height: 100vh; /* 同vw类似 */
  height: 100vm; /* 相对视口的宽度和高度较小的那个，1vm是其等分成100份的大小。兼容性差 */

  font-size: 16px;
  font-size: 1em; /* 相对长度，基于父元素的font-size，若未设置则相对于浏览器的默认字体尺寸 */
  font-size: 1rem; /* css3新增相对单位，基于html根元素的字体大小，如未设置则以浏览器默认字体大小计算 */
}
```

## 参考链接
- [Hexo官方文档](https://hexo.io/zh-cn/docs/index.html)
- [Hexo主题开发](https://www.cnblogs.com/yyhh/p/11058985.html)
- [CSS3实现多种背景效果](https://www.cnblogs.com/laixiangran/p/8999711.html)
- [解决锚点链接跳转后位置上下偏移的偏移的办法](https://blog.csdn.net/luoyu6/article/details/82947713)
- [原生js 平滑滚动到页面的某个位置](https://www.cnblogs.com/z-one/p/9603263.html)
- [html锚点 偏移问题](https://blog.csdn.net/wanglj7525/article/details/54632186)
- [JS：目录跟随定位效果的实现及锚点中使用name和id定位的区别](https://blog.csdn.net/cangyingaoyou/article/details/7460146)
- [Css单位px，rem，em，vw，vh的区别](https://www.cnblogs.com/theblogs/p/10516098.html)
- [hexo主题测试](https://github.com/hexojs/hexo-theme-unit-test)
- [Highcharts API文档](https://api.highcharts.com/highcharts/)
