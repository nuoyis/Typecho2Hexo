---
title: Hexo外带主题的配置方案
date: 2024-02-18 10:32:26
categories: default
tags: []
---
Hexo作为一个能静态化的轻量级博客程序，拥有着无需服务器，仅需一个github pages+全球CDN就秒杀绝大多数的动态语言博客。当然动态语言博客可以多人使用，hexo适合个人使用且无后台，也就无后台被攻击这种说法了。
首先感谢:https://www.zdynb.cn/2019/hexo-bi-bei-cha-jian.html的文章，如果你要配置额外内容请在此篇文章内查找

windows安装方法  
首先，Hexo的框架你得安装吧，nodejs作为底层得安装吧  
第一步, 下载个nodejs就行了。[快捷链接](https://nodejs.org/en)  
第二部, cmd命令框执行 `npm install hexo-cli -g`
第三步，新建一个文件夹，然后重新在目录内打开cmd，执行hexo init(执行前保持目录内干净)
第四步,修改_config.yml内容，我的站点配置如下。（部分内容我打了星的都是敏感区域，还有些需要上方链接点击进去配置插件。
```
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: 诺依阁的日记簿
subtitle: '一个记录技术的日记簿'
description: '一个记录技术的日记簿'
keywords: typecho,php,blog,诺依阁,站长,系统安装,环境搭建,服务器,站长,诺依阁的日常记录,生活,技术,科技,日记,诺依阁的日记簿,博客,云服务器,白嫖,自媒体,树洞空间
author: nuoyis
language: zh-CN
timezone: 'Asia/Shanghai'
email: wkkjonlykang@vip.qq.com

# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: https://blog.nuoyis.net
permalink: :abbrlink.html
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks

#baidu
baidu_url_submit:
  count: 80             # 提交最新链接数量
  host: blog.nuoyis.net    # 在百度站长平台中注册的域名
  token: xxxxxxxxxxxxxx # 请注意这是您的秘钥， 所以请不要把博客源代码发布在公众仓库里!
  path: baidu_urls.txt  # 文本文档的地址， 新链接会保存在此文本文档里

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link:
  enable: true # Open external links in new tab
  field: site # Apply to the whole site
  exclude: ''
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
syntax_highlighter: highlight.js
highlight:
  line_number: true
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
prismjs:
  preprocess: true
  line_number: true
  tab_replace: ''

# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 10
  order_by: -date

# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Metadata elements
## https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta
meta_generator: true

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss
## updated_option supports 'mtime', 'date', 'empty'
updated_option: 'mtime'

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

# Include / Exclude file(s)
## include:/exclude: options only apply to the 'source/' folder
include:
exclude:
ignore:

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: butterfly

# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
#github这里配置了账户密钥
deploy:
  - type: git
    repository:
      github: https://************************@github.com/nuoyis/blog.git,master

search:
  path: search.xml
  field: post
  content: true
  template: ./search.xml

feed:
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content:
  content_limit: 140
  content_limit_delim: ' '
  order_by: -date
  
githubEmojis:
  enable: true
  className: github-emoji
  inject: true
  styles:
  customEmojis:

# hexo-admin authentification
admin:
  username: ******
  password_hash: ******************************************************
  secret: nuoyis
```
如果你npm无法下载完全，那么就需要用到如下命令  
```
npm config set registry https://registry.npmmirror.com/
```
写文章的话,你需要先在这个init目录下执行hexo new "你要写的文章内容",  
然后就在source\_posts里面可以看到文章并编写。  
主题的话，你需要按照主题文档的要求去配置。
下面就以butterfly的做示范。
首先主题clone一份（在git的那个窗口，没下载的用clone下面一条命令)
```
#clone二选一
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly

git clone -b master https://gitee.com/immyw/hexo-theme-butterfly.git themes/butterfly
```

zip方式需要自行解压到themes目录，可能需要创建文件夹butterfly

```
https://gitee.com/immyw/hexo-theme-butterfly/repository/archive/master.zip
```

从butterfly文件夹内复制_config.yml，并将其重命名为_config.butterfly.yml。
然后进入https://butterfly.js.org/posts/21cfbf15/ 这个链接进行详细的配置。
至此，Hexo大致就完成了。然后就是做好运维，不要像我一样三天两头改博客地址
如果你不想每次都得手动上传，请查看文章:https://blog.nuoyis.net/posts/1594.html
如果你不想一篇篇把文章从typecho移到hexo，请看这篇文章:https://blog.nuoyis.net/posts/b4cf.html