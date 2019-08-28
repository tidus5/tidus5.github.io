title: hexo添加hexo-math插件以支持LaTeX公式
date: 2016-03-06 00:00:19
categories:
- hexo
tags:
- hexo-math
- LaTex
- MathJax
---

翻译四圣龙神录的时候，遇到图片里包含了公式的，正好最近在学markdown，而CSDN的md编辑器包含支持LaTeX公式的MathJax插件。  
但转到自己的Hexo博客，就得单独添加插件了。网上找了点资料，已经有人做好了hexo的MathJax插件了。安装也很简单。  
可以参考 [利用MathJax来渲染LaTeX数学公式][1]进行安装，具体可以去[hexo-math github项目页面][2] 看上面有安装方法。

看下效果：

$a^2+b^2=c^2$   
$E=mc^2$

最近发现一个问题。我的hexo编译有可能出现错误。提示DS.store的 compile 找不到之类的问题。
反复参照正确的设置，发现是hexo的版本发生了变化。  
之前没问题的，是hexo 3.1.1版本。有问题的是hexo 3.2.0版本。  
安装3.1.1版本的hexo， 用命令npm install hexo@3.1.1， 可以覆盖本地之前安装的3.2.0。  
可以用 npm ls 查看已安装的组件，安装回hexo 3.1.1 版本，hexo g 就不报错了。
这个bug已经有人建立了一个 [issue](https://github.com/hexojs/hexo/issues/1793)  
持续关注中。。

<!--more-->

hexo作者对这个问题作了回复了。因为.DS.Store 是Mac系统生成的文件，这里直接删掉就行。
`git rm ./themes/yilia/layout/.DS_Store`
`git rm ./themes/yilia/layout/_partial/.DS_Store`
`git commit -am "delete .DS_Store`
OK,然后就可以把hexo升级到3.2.0了，直接 npm install hexo 就行。
升级之后，hexo g 生成以后如果没有变化，重新hexo g好像不会再全部生成了，有文章修改也只生成修改部分的文件，大大提高生成的速度。
[https://github.com/hexojs/hexo/issues/1807](https://github.com/hexojs/hexo/issues/1807)
[https://github.com/litten/hexo-theme-yilia/issues/189](https://github.com/litten/hexo-theme-yilia/issues/189)


PS:[官网](https://hexo.io/zh-cn/) 上是用的 npm install -g hexo-cli  
这是全局安装hexo-cli, 然后在blog文件夹执行hexo init 的时候，也会安装到hexo@3.2.0去。
npm ls 是查看本地安装的组件， npm ls -g 是查看全局安装的组件。

参考页面：
[hexo-math 中文说明](http://catx.me/2014/03/09/hexo-mathjax-plugin/) （已过时，新的参考插件的github页）  
[玩转hexo博客 添加音乐播放器](http://www.tuicool.com/articles/NneMnuF)  
[Node.js安装教程和NPM包管理器使用详解](http://www.jb51.net/article/53813.htm)  
[利用MathJax来渲染LaTeX数学公式][1]  
[Mathjax与LaTex公式简介](http://mlworks.cn/posts/introduction-to-mathjax-and-latex-expression/)



[1]: http://blog.csdn.net/emptyset110/article/details/50123231
[2]: https://github.com/akfish/hexo-math



---

我的个人博客地址：

[http://tidus5.github.io](http://tidus5.github.io)

我的CSDN博客目录：

[http://blog.csdn.net/tidus5](http://blog.csdn.net/tidus5)

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="http://music.163.com/outchain/player?type=2&id=208902&auto=0&height=66"></iframe>
