title: hexo用subtree同步主题
date: 2016-01-29 22:54:32
categories:
- Hexo
tags:
- Hexo
- subtree
---

昨天把hexo搭好了以后，改了下主题模板的设置，提交有点问题。太晚了，也就没在意

今天去公司代码更新下来，发现少了点什么。所用的主题文件夹下面是空的。

在github上面看了下，文件夹是灰色的。比如像这个样子

https://www.zhihu.com/question/32213693/answer/55425001


搜了下，是因为直接clone第三方主题，所以其实下面是另一个完整的项目repo。

要同步主题，网上有几种方法。

1.  把.git删掉，这样主题文件夹就变成普通的文件夹了。add 之后，commit 到自己项目就行了。
但这样的问题是，没法保持主题的更新，等于是复制了一份到自己的目录。
2.  用git的submodule，在项目的子文件夹里包含子模块。
3.  用git的subtree，也是子文件夹里包含分支项目，但管理上比submodule简单一些，也可同步更新。

网上搜索了一些submodule 和subtree 的资料，虽然还不是很懂，还是考虑用简单的subtree来管理。


<!--more-->
 
具体实施，基本参考这个页面：
https://aoxuis.me/bo-ke/2013-08-06-git-subtree

由于第一次subtree的时候，提示

prefix 'themes/yilia' already exists.

若只是删除了目录再subtree会提示

Working tree has modifications.  Cannot add.

所以，本地删掉了yilia目录（先备份config）并提交，保证本地项目干净。

`git rm themes/yilia`

`git commit -m "delete yilia"`

`git push origin hexo`

`git status`

`git remote add -f yilia git@github.com:tidus5/hexo-theme-yilia.git`

`git subtree add --prefix=themes/yilia yilia master --squash`

`git fetch yilia master`


然后，最后一条命令执行时报了个错误

`git subtree pull --prefix=themes/yilia/ yilia --squash`

`You must provide <repository> <ref>`

试了下，在命令中加上具体分支名就行了

`git subtree pull --prefix=themes/yilia yilia master  --squash`


在使用上，可以就看做是自己项目的一部分（我是这么理解的）。但subtree的部分，也可以单独推送，和获取更新

`$ git commit -a -m 'update some'`

`$ git subtree push --prefix=themes/yilia/ yilia master`

`$ git push origin master                                    # 顺便主项目也 push 了`

或者

`git subtree push -P themes/yilia/ yilia master`

命令参考：

`'git subtree' add   -P <prefix> <commit>`

`'git subtree' add   -P <prefix> <repository> <ref>`

`'git subtree' pull  -P <prefix> <repository> <ref>`

`'git subtree' push  -P <prefix> <repository> <ref>`

`'git subtree' merge -P <prefix> <commit>`

`'git subtree' split -P <prefix> [OPTIONS] [<commit>]`


相关参考资料：


http://rogerdudler.github.io/git-guide/index.zh.html

http://youthyblog.com/2014/06/28/%E4%BD%BF%E7%94%A8github%E7%AE%A1%E7%90%86hexo%E6%9C%AC%E5%9C%B0%E6%96%87%E4%BB%B6/

http://zipperary.com/2013/05/28/hexo-guide-2/

==============================


http://www.williamsang.com/archives/2284.html

http://blog.jysoftware.com/2013/07/git%E7%AE%80%E4%BB%8B%EF%BC%9Asubmodule-subtree/

http://wenku.baidu.com/link?url=ola85Z5tIXJpxCjLTk-dcO81ayXLs68_y6dsmXIa0niF8vWlnAtnEEiZTGlzCNk1G_g36UYNHUBpu9oszONFNB54LNzo3rX7W_ULJg-P-eG

http://blog.zlxstar.me/blog/2014/07/18/git-submodule-vs-git-subtree/

https://www.atlassian.com/git/articles/alternatives-to-git-submodule-git-subtree/

http://blog.devtang.com/blog/2013/05/08/git-submodule-issues/

https://qdan.me/list/VOBvIeMuLl7IQHRp

https://segmentfault.com/q/1010000000799558

https://hpc.uni.lu/blog/2014/understanding-git-subtree/

http://aoxuis.me/post/2013-08-06-git-subtree

http://havee.me/linux/2012-07/the-git-advanced-subtree.html

http://www.cnblogs.com/kidsitcn/p/4541890.html

==========================


http://aceking.gitcafe.io/posts/git-submodule.html


http://efe.baidu.com/blog/git-submodule-vs-git-subtree/



---
我的个人博客地址：

[http://tidus5.github.io](http://tidus5.github.io)

我的CSDN博客目录：

[http://blog.csdn.net/tidus5](http://blog.csdn.net/tidus5)
