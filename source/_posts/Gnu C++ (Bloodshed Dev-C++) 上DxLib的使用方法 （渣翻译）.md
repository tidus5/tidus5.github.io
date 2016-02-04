title: Gnu C++ (Bloodshed Dev-C++) 上DxLib的使用方法 （渣翻译）
date: 
categories:
- 
tags:
- 
---

原文地址：
[http://homepage2.nifty.com/natupaji/DxLib/dxuse_gcc.html](http://homepage2.nifty.com/natupaji/DxLib/dxuse_gcc.html)
由于日文原文较长，所以这里不贴原文，就大致理解，给出个人译文吧。

要在Gnu C++ 上 使用DxLib 制作软件，请按照下面的步骤进行。
或者说，这里不仅是Dev-C++的使用方法，而是在Dev-C++ 上用DxLib使用的步骤。

1.要使用 DxLib所必须要准备的东西
2.Dev-C++ 上，使用DxLib的设置
3.项目的创建
4.为了使用DxLib，而进行项目的设置
5.程序的代码组织
6. 编译项目，运行

1. 要在Dev-C++ 上使用DxLib，需要以下的东西。
      Bloodshed Dev-C++
      给Gnu C++用的DxLib版本
     首先，Bloodshed Dev-C++ 是使用Gnu C++进行软件开发的使用最简单易用的软件。
    这是免费公开的软件，如果电脑上没有安装Dev-C++的话，直接搜索一下，找到它的下载网址。
  （本说明 主要是为了说明Dev-C++的使用配置方法，所以安装教程就不讲了）
     然后，Gnu C++ 版本的DxLib 可以在本站点的[这里](http://homepage2.nifty.com/natupaji/DxLib/dxdload.html) 下载到。没有这个的话，DxLib 是使用不了的，如果还没有下载的话，请先下载。

2. Dev-C++ 上使用DxLib 所需的设置
  必须的东西都准备好了之后，只要在Dev-C++上稍作设置，电脑上任意位置的项目都可以使用DxLib了。
   ① 选择 Dev-C++ 的菜单里的Tools -》  Compiler Option
   ② 选择 Compiler Option 窗口 的 Directorie
   ③ 下面出来的 标签里面  选择 Libraries
   ④  Directory  List  （文件夹列表） 里，添加上  DxLib的 包
   『プロジェクトに追加すべきファイル_GCC(Dev-cpp)用』 GCC（Dev-cpp）用的项目 添加文件夹 ) 把这个文件夹的路径，填进去
    注意，要点击Add按钮，才能进行添加
⑤ 现在，选择 C++ Includes 这个标签
⑥  和第四步的 文件夹列表一样，加上 『プロジェクトに追加すべきファイル_GCC(Dev-cpp)用』（GCC（Dev-cpp）用的项目添加文件夹）
     这个文件夹的路径。 （注意，点击Add 按钮，才能进行添加）
⑦ 点击OK 按钮，设置就完毕了。

3. Dev-C++ 已经设置好了的话，赶紧用DxLib制作软件吧
   因为主要是说使用步骤，这里就简单在画面的中心画一个点，以制作这样的程序作为说明吧
   用 Dev-C++ 制作 软件，首先要创建一个项目，下面就是具体方法的说明。
    ① Dev-C++ 的菜单，选择 文件 -》新建-》  项目
   ②  新建项目 窗口中，选择 windows 程序。
   ③  在项目 的名称一栏中，填入 项目名称，然后点击OK按钮。  （在这里就填入 DrawPixel 作为项目名）
   ④ 然后，选择项目的保存位置文件夹，选择好了之后，点击保存按钮。
  ⑤ 打开项目的话，最初是 示例代码，打开main.cpp   选择 Dev-C++的菜单 的 文件--》 保存，在 项目文件夹里，把main.cpp文件保存一下。

4.  DxLib的文件往项目文件夹复制过去之后，为了要让项目使用DxLib，需要进行一些设定

　　①　选择 Dev-C++ 的『Project』→『Project Options』。

　　②　选择 Project Options窗口的『Parameters』标签。

　　③　『C++ compiler』这一栏，填入以下两行。

-DDX_GCC_COMPILE
-DDX_NON_INLINE_ASM

　　④　『Linker』项目里，填入以下13行。

-lDxLib
-lDxUseCLib
-lDxDrawFunc
-ljpeg
-lpng
-lzlib
-ltheora_static
-lvorbis_static
-lvorbisfile_static
-logg_static
-lbulletdynamics
-lbulletcollision
-lbulletmath

行的顺序改变了可能导致无法正常链接，请按照上面所述的顺序输入。
⑤  点 OK 按钮，设定就OK了

5.接下来就是代码的编写了。
　Dev-C++ 的项目一开始就添加好 main.cpp 的， 因为代码是写在这里的、在 Dev-C++ 菜单的『Edit』→『Select All』选择，按Delete键全部删掉。
　然后把下面的代码输入 main.cpp 。这是在画面中心画一个点的程序。



```cpp
#include "DxLib.h"

// 程序是从 WinMain 开始的
int WINAPI WinMain( HINSTANCE hInstance, HINSTANCE hPrevInstance,
						LPSTR lpCmdLine, int nCmdShow )
{
	if( DxLib_Init() == -1 )		// DxLib的初始化
	{
		return -1 ;			// 发生错误的话，直接退出
	}

	DrawPixel( 320 , 240 , 0xffff ) ;	// 画一个点

	WaitKey() ;				// 等待按键输入

	DxLib_End() ;				// DxLib使用结束

	return 0 ;				// 程序结束 
}

```

程序就这么短，这里解释一下这段代码做了什么。 首先第一行是把使用DxLib所必需的文件包含进来。
　然后『int WINAPI WinMain( HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow )』是windows程序的入口函数的声明。这些词语的具体是什么含义，可以不必太关心。 Windows环境下的程序的入口函数都是这样声明的。
　中间括号里面的『if( !DxLib_Init() ) return -1;』是DxLib的是调用DxLib的初始化函数『DxLib_Init』。这个函数，是使用DxLib的程序，在除了特殊例外情况下，都是需要最先调用的。因为有『if(...』的判断，如果初始化失败了，那么程序就退出。
　『DrawPixel』就是画一个点的函数。
　然后『WaitKey』是等待按一个键的暂停函数。
　最后的『DxLib_End() ;』注释上也写了，是DxLib使用结束的处理函数。使用DxLib的程序最后必须要调用这个函数。没有调用这个函数程序就结束了的话，会粗大事的，请注意。


6. 输入完成后，运行一下吧

　　①　Dev-C++ 的菜单里，选『Execute』→『Compile』 ，编译程序。
　　　编译成功的话， Compile Progress 窗口里有『Done.』来表示。
　　　发生错误的话下面的窗口会输出错误的信息，按照上面的错误提示进行改正吧。
　　②　然后 Dev-C++ 的菜单里面选择『Execute』→『Run』。程序就开始运行了

　 然后程序画面中心是不是生成一个点呢？

　编译生成了一个可执行文件，他在项目的文件夹下面。
这就是我们目前工作的成果。
　于是了解使用DxLib开发程序的方法了吧。 接下来就可以按自己想喜好编写程序，组合成一个游戏了。 但目前我们只知道DxLib的初始化，结束，和画一个点的函数。 　其他的函数在[DxLib的函数手册页面](http://homepage2.nifty.com/natupaji/DxLib/dxfunc.html)说明，请去那里参考一下吧。

