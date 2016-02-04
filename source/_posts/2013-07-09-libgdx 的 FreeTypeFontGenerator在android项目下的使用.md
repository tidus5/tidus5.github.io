title: libgdx 的 FreeTypeFontGenerator在android项目下的使用
date: 2013-07-09 00:42:00
categories:
- 
tags:
- 
---
桌面版，使用FreeTypeFontGenerator  没什么问题，但android项目老是报错。
网上搜了一下解决方案，把jar什么的包含了进去，还是有些问题。这里把解决的方案大致写上来。

首先，参考这里：http://www.badlogicgames.com/wordpress/?p=2300
把 gdx-freetype.jar 复制到libs下， 包含到android 项目中。然后在  build path 的order and export 把勾打上。
 带有native的是给桌面版项目用的，android项目不需要。 android项目需要的是  .so 文件。
去libgdx的 gdx-freetype 文件夹下， 把armeabi 和 armeabi-v7a 复制到libs下，和原来的这两个目录合并。
还有一些问题。额，新建了一个项目验证，才解决掉。就是，TTF 字体文件名是区分大小写的。
若文件名是 font.TTF    代码用的是 font.ttf  是会出错滴。。。。 就这个了。



