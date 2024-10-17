---
title: Hexo常用命令
tags: [Hexo,字典]
copyright: false
date: 2023-02-09 16:29:50
categories: 技术连连看
description: 本不想写这些东西，奈何自己不会，只好记录一下。
cover: cover.jpg
typora-root-url: Hexo常用命令
---

### hexo init [folder]

新建一个网站。如果没有设置`folder`，Hexo默认再当前文件夹建立网站。

 ### hexo  g  或  hexo generate

该命令执行后在hexo站点根目录下生成public文件夹

| 选项              | 描述                                                         |
| ----------------- | ------------------------------------------------------------ |
| -d, --deploy      | 文件生成后立即部署网站                                       |
| -w, --watch       | 监视文件变动                                                 |
| -b, --bail        | 生成过程中如果发生任何未处理的异常则抛出异常                 |
| -f, --force       | 强制重新生成文件Hexo 引入了差分机制，如果 public 目录存在，那么 hexo g 只会重新生成改动的文件。使用该参数的效果接近 hexo clean && hexo generate |
| -c, --concurrency | 最大同时生成文件的数量，默认无限制                           |

启动 hexo 服务，默认地址为  `http://localhost:4000 `

| 选项         | 描述                           |
| ------------ | ------------------------------ |
| -p, --port   | 重设端口                       |
| -s, --static | 使用静态文件                   |
| -l, --log    | 启动日记记录，使用覆盖记录格式 |

### hexo clean

把（1）中生成的public文件夹删除，也就是删除生成的静态文件

### hexo new [layout] \<title\>

例如：`hexo new post "Hello World"`  (new 可简写为 n)

[layout] 可选值：post , draft  , page  ,不写时默认为post

| 参数         | 描述                                        |
| ------------ | ------------------------------------------- |
| -p ,--path   | 自定义新文章的路径                          |
| -r,--replace | 如果存在同名文章，将其替换                  |
| -s,--slug    | 文章的Slug，作为新文章的文件名和发布后的URL |

默认情况下，Hexo 会使用文章的标题来决定文章文件的路径。对于独立页面来说，Hexo 会创建一个以标题为名字的目录，并在目录中放置一个 `index.md` 文件。你可以使用 `--path` 参数来覆盖上述行为、自行决定文件的目录

例如：`hexo new page --path about/me "About me"`

以上命令会创建一个 `source/about/me.md` 文件，同时 Front Matter 中的 title 为 `"About me"`

### 删除文章

直接再本地`source/_post`文件夹中删除文章源文件后重新部署即可。

参考命令：`hexo clean && hexo g && hexo s  //clean  防止灵异事件发生`

### hexo  public  [layout]  \<filename\>

发布草稿

### hexo list \<type\>

` type  Available types: page, post, route, tag, category`

列出网站资料 

### hexo version

显示Hexo版本

### hexo  --safe

进入安全模式，在安全模式下，不会载入插件和脚本。当您在安装新插件遭遇问题时，可以尝试以安全模式重新执行。

### hexo  --debug

进入调试模式，在终端中显示调试信息并记录到debug.log。当您碰到问题时，可以尝试用调试模式重新执行一次，并提交调试信息到GitHub。

### hexo  --silent

进入简洁模式，隐藏终端信息。

### hexo  --draft

显示`source/_ `文件夹中的草稿文章。

---

**The  End**
