---
layout: "post"
title: "Hexo Blog"
date: "2018-09-02 18:05"
comments: true
tags:
  - Hexo
categories:
  - Hexo
---


## 前言
　　备忘通过Hexo实现从md文件生成静态页面，然后发布到GitHub Pages的过程。

## 主要流程
### 1、安装必须的软件
1. 安装Git
   - [Git官方版本的安装](http://git-scm.com/download/win)
2. 安装Node.js
   - [Node.js官方版本的安装](https://nodejs.org/en/)
3. 安装Hexo
   - 常规安装命令<br/>
    $ npm install -g hexo-cli

   - 常规命令有可能被“墙”，安装hexo为了避免出现类似情况，我使用[淘宝NPM镜像](https://npm.taobao.org),输入以下命令等待安装完成。<br/>
   $ npm install -g cnpm --registry=https://registry.npm.taobao.org

   - 使用淘宝NPM安装Hexo。<br/>
   $ cnpm install -g hexo-cli   （与原先的npm完全一样，只是命令改为cnpm,一样等待hexo安装完成），出现的WARN可以不用理会，继续输入以下命令：
   $ cnpm install hexo --save

<!--more-->




### 2、验证软件正确安装
- git --version     　　检验git是否安装成功，显示出git的具体版本
- node -v   　　　　检验node是否安装成功，显示出node的具体版本
- npm -v     　　　　检验npm是否安装成功，显示出npm的具体版本
- hexo -v　　　　检验hexo是否安装成功，显示出hexo的具体版本


### 3、 使用Hexo建站




- 安装完后，创建文件夹（例如D：\Hexo），在文件夹内点击鼠标右键选择Git bash，输入以下指令：

　　 `$ hexo init`　　　该命令会在目标文件夹内建立网站所需要的所有文件。

- 接下来是安装依赖包：

　　 `$ npm install`　或者　`$ cnpm install`　　　搭建起本地的Hexo博客

- 本地运行博客只要输入该命令

　　 `$ hexo generate　和　$ hexo server`　或者　`$ hexo s -g`　　启动本地博客，打开浏览器，输入localhost:4000,就可以在本地看到你的个人博客了


### 4、 一般的搭建方法

　　在上面，我们已经配置好了所需的所有东西，也成功地搭建了一个本地Hexo博客。现在，需要使用GitHub Pages搭建一个别人能够访问的Hexo博客。
#### 4.1 使用默认theme

　我们继续使用上面的文件夹D:\Hexo（也可以新建一个文件夹重新生成），然后编辑该文件夹下的_config.yml。
　默认生成的_config.yml：
``` Java
  # Deployment
  ## Docs: http://hexo.io/docs/deployment.html
  deploy:
    type:
```
　修改后的_config.yml：
``` Java
  deploy:
    type: git
    repo: 对应仓库的SSH地址（可以在GitHub对应的仓库中复制）
    branch: 分支（User Pages为master，Project Pages为gh-pages）
```

　为了能够使Hexo部署到GitHub上，需要安装一个插件：
``` Java
  $ npm install hexo-deployer-git --save  或者 $ cnpm install hexo-deployer-git --save
```

　执行下列指令即可完成部署：
``` Java
  $ hexo generate
  $ hexo deploy
  或者
  $ hexo d -g
```
　之后，可以通过在浏览器键入：username.github.io进行浏览。

#### 4.2 简介 blog/_config.yml文件
``` Java
  #博客名称
  title: 我的博客
  #副标题
  subtitle: 一天进步一点
  #简介
  description: 记录生活点滴
  #博客作者
  author: John Doe
  #博客语言
  language: zh-CN
  #时区
  timezone:

  #博客地址,与申请的GitHub一致
  url: http://elfwalk.github.io
  root: /
  #博客链接格式
  permalink: :year/:month/:day/:title/
  permalink_defaults:

  source_dir: source
  public_dir: public
  tag_dir: tags
  archive_dir: archives
  category_dir: categories
  code_dir: downloads/code
  i18n_dir: :lang
  skip_render:

  new_post_name: :title.md # File name of new posts
  default_layout: post
  titlecase: false # Transform title into titlecase
  external_link: true # Open external links in new tab
  filename_case: 0
  render_drafts: false
  post_asset_folder: false
  relative_link: false
  future: true
  highlight:
    enable: true
    line_number: true
    auto_detect: true
    tab_replace:

  default_category: uncategorized
  category_map:
  tag_map:

  #日期格式
  date_format: YYYY-MM-DD
  time_format: HH:mm:ss

  #分页，每页文章数量
  per_page: 10
  pagination_dir: page

  #博客主题
  theme: landscape

  #发布设置
  deploy:
    type: git
    #elfwalk改为你的github用户名
    repository: https://github.com/elfwalk/elfwalk.github.io.git
    branch: master
```


### 5、优化部署与管理(☆☆☆)
　　Hexo部署到GitHub上的文件，是.md（你的博文）转化之后的.html（静态网页）。因此，当你重装电脑或者想在不同电脑上修改博客时，就不可能了（除非你自己写html）。

　　其实，Hexo生成的网站文件中有.gitignore文件，因此它的本意也是想我们将Hexo生成的网站文件存放到GitHub上进行管理的（而不是用U盘或者云备份啦）。这样，不仅解决了上述的问题，还可以通过git的版本控制追踪你的博文的修改过程，是极赞的。

　　但是，如果每一个GitHub Pages都需要创建一个额外的仓库来存放Hexo网站文件，我感觉很麻烦（10个项目需要20个仓库(）。

　　所以，利用了分支！！！简单地说，每个想建立GitHub Pages的仓库，起码有两个分支，一个用来存放Hexo网站的文件，一个用来发布网站。

#### 5.1 搭建流程

    1. 创建仓库，username.github.io；
    2. 创建两个分支：master 与 hexo；
    3. 设置hexo为默认分支（因为我们只需要手动管理这个分支上的Hexo网站文件）；
    4. 使用git clone git@github.com:username/username.github.io.git拷贝仓库；
    5. 在本地username.github.io文件夹下通过Git bash依次执行npm install hexo、hexo init、npm install 和 npm install hexo-deployer-git（此时当前分支应显示为hexo）;
    6. 修改_config.yml中的deploy参数，分支应为master；
    7. 依次执行git add .;    git commit -m "…";    git push origin hexo提交网站相关的文件；
    8. 执行hexo generate -d生成网站并部署到GitHub上。

　　这样一来，在GitHub上的username.github.io仓库就有两个分支，一个hexo分支用来存放网站的原始文件，一个master分支用来存放生成的静态网页。
#### 5.3 管理流程
##### 5.3.1 日常修改

　　1. 在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理：
    2. 依次执行git add .git;  commit -m "…";   git push origin hexo指令将改动推送到GitHub（此时当前分支应为hexo）；
    3. 然后才执行hexo generate -d发布网站到master分支上。

#####  5.3.２本地资料丢失

当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：

    1. 使用git clone git@github.com:username/username.github.io.git拷贝仓库（默认分支为hexo）；
    2. 在本地新拷贝的username.github.io文件夹下通过Git bash依次执行下列指令：npm install hexo、npm install、npm install hexo-deployer-git（记得，不需要hexo init这条指令）
