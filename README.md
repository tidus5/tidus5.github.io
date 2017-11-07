# tidus5.github.io

hexo搭建github静态博客（记录下本博客搭建的流程）  
[CrazyMilk的基于github保存文章的教程](https://www.zhihu.com/question/21193762/answer/79109280)  
[GitHub Pages + Hexo搭建博客](http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more)
一、关于搭建的流程
准备工作，github申请账号，安装git 和 nodejs.
1. 建立 自己用户名.github.io 的仓库 （比如 CrazyMilk.github.io）。
2. 创建两个分支：master 与 hexo；
3. 设置hexo为默认分支（因为我们只需要手动管理这个分支上的Hexo网站文件）；
4. 使用git clone git@github.com:CrazyMilk/CrazyMilk.github.io.git拷贝仓库；
5. 在本地 CrazyMilk.github.io文件夹下通过Git bash依次执行npm install hexo-cli -g、hexo init、npm install 和 npm install hexo-deployer-git（此时当前分支应显示为hexo）;
6. 修改_config.yml中的deploy参数，分支应为master；
7. 依次执行git add .、git commit -m "..."、git push origin hexo提交网站相关的文件；
8. 执行hexo g -d生成网站并部署到GitHub上。
这样一来，在GitHub上的http://CrazyMilk.github.io仓库就有两个分支，一个hexo分支用来存放网站的原始文件，一个master分支用来存放生成的静态网页。完美( •̀ ω •́ )y！  

二、关于日常的改动流程
1. 依次执行git add .、git commit -m "..."、git push origin hexo指令将改动推送到GitHub（此时当前分支应为hexo）；
2. 然后才执行hexo g -d发布网站到master分支上。

三、本地资料丢失后的流程
当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：  
1. 使用git clone git@github.com:CrazyMilk/CrazyMilk.github.io.git拷贝仓库（默认分支为hexo）；
2. 在本地新拷贝的http://CrazyMilk.github.io文件夹下通过Git bash依次执行下列指令：npm install hexo-cli -g、npm install、npm install hexo-deployer-git（记得，不需要hexo init这条指令）。

四、配置主题的流程
然后，因为我hexo用了yilia主题，存放在hexo的themes目录下。  
关于项目更新，作者的回复是：
>其实是一个git使用的问题。  
>普通用户基本只会修改_config.yml文件，这时只要git pull下来即可；  
>如果你有修改源文件，做了一些编译工作，建议自行fork，再选择性merge我这边的更新。  

我这里首先要从yilia项目上进行fork，然后这里使用了subtree来在项目中嵌入其他项目。
(上传配置小心会泄漏秘钥等敏感数据，如果不在意就没事。。）  
[在github里用subtree同步hexo主题](http://tidus.site/2016/01/29/hexo%E7%94%A8subtree%E5%90%8C%E6%AD%A5%E4%B8%BB%E9%A2%98/)  
[使用git subtree集成项目到子目录](https://aoxuis.me/bo-ke/2013-08-06-git-subtree)  

1. 把yilia项目fork 到自己账号下。  
2. 如果本地在hexo项目想已经在使用yilia主题，则先备份yilia下的_config.yml文件，然后执行 git rm themes/yilia 、git commit -m "delete yilia" 、git push origin hexo 删除yilia 目录
3. git remote add -f yilia git@github.com:tidus5/hexo-theme-yilia.git
4. git subtree add --prefix=themes/yilia yilia master --squash
5. git fetch yilia master
6. git subtree pull --prefix=themes/yilia yilia master --squash

修改了主题配置后，提交推送
git add .  
git commit -am 'update some'  
git subtree push --prefix=themes/yilia/ yilia master  

博客项目提交推送  
git add.  
git commit -am 'update some'  
git push origin master  

博客日常发表，生成，发布  
hexo n "文章标题"   #新建文章  
hexo g    #生成静态页面至public目录  
hexo s    #开启预览访问端口  
hexo d    #部署到GitHub  
hexo g -d #生成并部署  


Q&A:  
1. hexo 的depoly怎么填  
好像是不同的hexo版本填的有点区别，反正我填的是  
deploy:  
  type: git  
  repo: git@github.com:tidus5/tidus5.github.io.git  
  branch: master  
  
2. gitment 评论怎么配置  
yilia已经集成gitment，只需要填4个参数。要自己集成可参考 [gitment](https://imsun.net/posts/gitment-introduction/)  
owner 填github 的 id  
repo  存储评论的 repo， 可以直接用博客的repo， 比如 'tidus5.github.io'  
id 和key 填oath 生成的就行。  
OAuth 生成后如果id和key忘了，可以在github 点自己头像的setting，点左边的 Deleloper Setting 然后可以看到自己创建的OAuth Apps  
OAuth 的callback URL 需要配置成博客的实际绑定域名，比如绑定到 imsun.net 就要填 https://imsun.net ，不然登录就会跳回首页，并提示地址不匹配。  

3. fork之后theme更新了，怎么拉取最新的theme呢？git命令太麻烦了，看不懂  
我也觉得fork之后同步更新的命令比较麻烦，所以找了个ui操作的版本，这篇文章前面讲提交PR给原作者，后半部分讲拉取更新到自己的repo  
[如何同步fork项目后作者的更新](http://blog.csdn.net/t111t/article/details/45894381)  

首先讲一下，何为Pull Request. 这个翻译过来叫做“拉取请求”，你对别人的项目有修改，然后提交一个PR，“请求”作者来“拉取”你的提交。  
然后，PR可以是双向的，意思是可以从别人的项目创建PR，发给自己fork的项目。  
要同步更新，可以去自己fork的项目发起一个Pull Request， 然后会显示两个分支。  
如果只看到分支，看不到项目地址，点那个小字“ compare across forks.” 
然后左边设置成左边是自己的项目，右边是作者的项目。  

*注意*  
**PR合并方向是从右向左的**  
**PR合并方向是从右向左的**  
**PR合并方向是从右向左的**  

重要的事情说三遍。因为现在PR 页面没有那个 ← 的箭头指示了。 我都已经两次搞错方向，自己给别人提了个老版本的PR。然后尴尬的去关掉 o(╯□╰)o    
选好了之后，下面会看到作者和别人的提交（如果选错方向，下面会显示自己的提交）    
然后提交，就合并到最新的代码了。    
本地再 git pull 或者 git subtree pull --prefix=themes/yilia yilia master --squash 就可以拉取下来了。    

