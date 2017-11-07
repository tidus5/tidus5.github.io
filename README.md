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

我这里首先要从yilia项目上进行fork，然后这里使用了subtree来在项目中嵌入其他项目。(上传配置小心会泄漏秘钥等敏感数据，如果不在意就没事。。）
(在github里用subtree同步hexo主题)[http://tidus.site/2016/01/29/hexo%E7%94%A8subtree%E5%90%8C%E6%AD%A5%E4%B8%BB%E9%A2%98/]
(使用git subtree集成项目到子目录)[https://aoxuis.me/bo-ke/2013-08-06-git-subtree]
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


