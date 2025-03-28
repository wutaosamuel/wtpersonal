# 折腾Hugo静态网站


## 背景

  很早之前就听说过Github推出了一个叫Git Page的产品 ...

## 准备

因为在很多开发，部署，测试的情况需要下载很多依赖，
所以我不会在我的windows主力机上做开发。
因此ubuntu虚拟机和windows10的linux子系统是我主要开发平台。
如果要使用LoveIt主题的Sass/SCSS，必须安装extended标签的hugo。
现在GO源码安装能同时自动安装相应依赖，所以现在从源码安装也很方便。
**但是请做好GO代理**。

- [Git](https://git-scm.com/)
- [Go(1.11版本以上)](https://golang.org/dl/)

## 安装 Hugo

首先，下载Hugo源码。
可以从[Hugo github主页](https://github.com/gohugoio/hugo)上git clone，
也可以像我我一样用go get，同时他也会下载依赖(请做好代理):

``` bash
git clone https://github.com/gohugoio/hugo
```

再进入Go源码文件夹下hugo的文件夹:

```bash
cd hugo
```

这里会使用Sass/SCSS，所以在用Go安装的时候会用extended的tag。

安装(请做好代理):

```bash
CGO_ENABLED=1 go install --tags extended
```

最后查看hugo是否成功安装(如果是全局代理的, 现在可以取消了):

```bash
hugo version
```

## 使用Hugo

### Hugo & Git 初始化

首先，创建或使用一个空文件夹作为你的私有或公共的git仓库。
利用**hugo new**会创建一个新的文件夹并且会在文件夹下生成一个新的网站结构:

```bash
hugo new side my_website
cd my_website
```

并初始化为git仓库，可以使用**git add .**和**git commit -m "comment"**提交第一次记录:

```bash
git init
```

因为网站的主题是[LoveIt](https://hugoloveit.com/),
所以需要克隆LoveIt的主题到**themes**目录:

```bash
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

### Hugo & LoveIt 配置

网页的配置文件是**config.toml**, 可以参考LoveIt的配置文件以及我的配置:

- [LoveIt官方配置](https://github.com/dillonzq/LoveIt/blob/master/exampleSite/config.toml)
- 这个网站的配置(还没想好怎么贴)

### 文章创作

利用hugo生成不同语言的文章**first_post.language.md**

```bash
hugo new posts/first_post.zh-cn.md
```

之后就可以直接编辑**content/posts/first_post.zh-cn.md**文件

### 预览网站

本地启动网站:

```bash
hugo server --buildDrafts --bind=0.0.0.0
```

-D/--buildDrafts 用来渲染文件;

--bind=0.0.0.0 允许所有ip能登录你的本地网站 yourIP:1313

### 生成最终网站

修改完网站的内容之后，
需要生成能够部署在github page上的文件(需要修改文章中draft: false):

```bash
hugo
```

会生成public的文件夹。

将public文件夹下内容git commit(需要git初始化)记录之后就可以上传到对应的github page的仓库上。

