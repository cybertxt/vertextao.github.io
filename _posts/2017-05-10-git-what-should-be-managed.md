---

layout: post
title: Git 指南 -- 什么应该被纳入管理？
date: 2017-05-10 13:50:00 +0800

---
# Git 指南 -- 什么应该被纳入管理？

如果还不了解Git是什么，可以先阅读这篇博文：[http://www.cnblogs.com/schaepher/p/5561193.html](http://www.cnblogs.com/schaepher/p/5561193.html)

## 是作品，而不是产品

### 什么是作品？

精心设计，手工打造的。举例：

* 源代码文件
* 部分配置文件
* 文档（包括个人写作，博客等）

### 什么是产品？

可以批量生产的。举例：

* 编译、链接产生的临时文件、目标文件、可执行文件
* 发行的软件包
* 运行时生成的日志文件、临时文件

## 如何防止不必要的文件被纳入管理？

### **.gitignore**

在被git管理的目录内，我们可以放置```.gitignore```文件来让git过滤那些不需要关注的文件。

详细说明及规则请参考：[https://git-scm.com/docs/gitignore](https://git-scm.com/docs/gitignore)

### 资源

Github 已经为各类工程整理了常规情况下需要过滤的文件，可在这里查看：[https://github.com/github/gitignore](https://github.com/github/gitignore)

把与自己工程类型对应的文件内容复制到自己工程的```.gitignore```中保存即可。


## 如果已经提交不必要的文件怎么办？

如果是少量的小文件，通常不必做什么，正常删除即可。但是如果不小心提交了不必要大文件，或者非常多的小文件，导致整个git仓库的大小变得难以接受，这时候就需要做清理工作。

**注意：无论如何，请先做好备份，避免误操作导致自己的作品丢失！**

### 简单粗暴的方法

如果你有一份远程仓库

* 把**需要保留**的文件复制出来一份
* 删除整个本地仓库
* 从远程仓库重新 ```git clone``` 一份下来
* 把第一步复制出来的文件覆盖进去，再提交

*如果你在本地已经有多次提交并且没有做```git push```，会使得这部分提交记录丢失，但是留住了最新版本+已经push到远程的版本！*

### 优雅的方法（有点复杂）

如果你很清楚要移除那些文件

* 删除并重写提交记录
    ```
    git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch xxx'
    ```
其中```xxx```填写你要删除的文件名，当然也支持通配符，如：```*.tar.gz``` 匹配所有的后缀名为 tar.gz 压缩包。

* 删除引用
    ```
    rm -rf .git/refs/original/
    rm -rf .git/logs/
    ```
* 垃圾回收并清理
    ```
    git gc
    git prune --expire now
    ```

如果你不知道是哪个文件占用那么大空间（比如这个文件已经在当前版本被删除了，但是历史版本里存在），请参考文档：[移除对象](https://git-scm.com/book/zh/v1/Git-%E5%86%85%E9%83%A8%E5%8E%9F%E7%90%86-%E7%BB%B4%E6%8A%A4%E5%8F%8A%E6%95%B0%E6%8D%AE%E6%81%A2%E5%A4%8D#移除对象)
