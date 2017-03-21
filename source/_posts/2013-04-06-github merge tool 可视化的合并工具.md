title: github merge tool 可视化的合并工具
date: 2013-04-06 23:27:00
categories:
- github
- merge
tags:
- github
---
在另外一台机子上改了代码，回来后，发现同步出了问题，也就是有冲突了。
用SVN的话，还直观一些。 可 GitHub for windows 没有提供可视化的解决冲突的界面，只会说同步失败，然后建立一个detached Head 的分支，让自己解决冲突去。

查了一晚上，找了一些相关的问题解决方法。
git status ，提示有一些文件 merge 失败，其实就是冲突了。
git mergetool 的时候，提示 `C:\program files\..`   下的 bcomp.exe 找不到。 这其实就是默认设置成 beyond compare的路径，然后这里没找到对应的文件。
我这好像装了某个版本的beyond compare ，这东西确实比较好用。
设置的话，经google 查找   git mergetool  beyondcompare  找到介绍。

 <!--more-->
对于windows 用户：

Windows users can configure this by entering the commands:	

	git config --global diff.tool bc3
	git config --global difftool.bc3.path "C:\Program Files (x86)\Beyond Compare 3\BComp.exe"
上面就是用命令，把bc3 改成 diff 的工具下面是配置文件，直接改这个文件，效果一样的。在我这，路径应该是 C:\Users\用户名\.gitconfig
###Windows
	notepad C:\Program Files\git\etc\config


	[user]
	    name = First Last
	    email = <a href="mailto:email@address.com">email@address.com</a>
	[color]
	    ui = true
	[core]
	    editor = nano
	[merge]
	    tool = bc3
	[mergetool "bc3"]
	    cmd = 'C:\Program Files (x86)\Beyond Compare 3\BComp.exe' \
	    "$PWD/$LOCAL" \
	    "$PWD/$REMOTE" \
	    "$PWD/$BASE" \
	    "$PWD/$MERGED"
	    keepBackup = false
	    trustExitCode = false

设置好之后，git mergetool 就可以用beyond compare的GUI 来显示合并了，看起来舒服多了。


PS：  用 git gui 也不错，但那个没用过，看不大懂。



---
我的个人博客地址：

[http://tidus5.github.io](http://tidus5.github.io)

我的CSDN博客目录：

[http://blog.csdn.net/tidus5](http://blog.csdn.net/tidus5)
