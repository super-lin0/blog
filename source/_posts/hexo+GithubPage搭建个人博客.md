---
title: Hexo+GitHub Pages搭建个人博客
date: 2019-07-17 20:01:16
tags: [hexo, GitHub Pages]
---

<center>
  本文介绍了如何根据 Hexo+GitHub Pages 来搭建个人的博客
<center>
</br>
</center>
  标签：#hexo, #GitHub Pages
</center>

<!-- more -->

### 1、效果

首先来看最终的效果：

![](https://raw.githubusercontent.com/super-lin0/pic/master/20190717203359.png)

### 2、Hexo

然后我们来看看什么是`Hexo`![](https://raw.githubusercontent.com/super-lin0/pic/master/20190717203241.png)

- Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

### 3、Hexo 安装及使用

```
yarn global add hexo-cli
hexo -v // 查看hexo-cli版本以及是否安装成功
hexo init <folder> && cd <folder>	// 创建项目以及初始化
yarn	// 安装依赖
hexo g	// 发布到自己本地查看效果
hexo deploy	// 部署到远程
hexo s	// 本地控制台启动
```

- 在安装 Hexo 前需要安装 Nodejs、Git，在此不再赘述。请自行百度

### 4、结合 GitHub Pages

打开刚才新建的文件夹内，找到\_config.yml 修改以下内容：

```
deploy:
  type: git
  repo: https://github.com/super-lin0/super-lin0.github.io.git	// 部署GitHub Pages的工程路径
  branch: master
```

- Notes

  需要注意的是在自己的 Git 中所建的仓库名称和你的用户名需要相同，工程名以`.github.io`结尾

---

### 参考文献：

1、<https://hexo.io/zh-cn/docs/>

2、<https://pages.github.com/>

3、<http://theme-next.iissnan.com/>
