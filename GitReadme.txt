GIT 是一个版本控制工具（主要用于团队下多人协作的代码管理——提交、合并、分支、回溯。。。）

VSS(Visual Source Safe) 微软推出（与VS.NET工具集成）
SVN(Subversion)
CVS(Open Source Version Control)

上面三种版本控制工具我们称作集中式管理，
因为它们的操作必须要网络和中央服务器的介入，
因此安全性和操作性不高（大家共享一台服务器且必须在联网情况下）！


GIT
称作分布式管理，
不需要联网，也不需要依赖于某台主服务器，
代码仓库可以进行克隆，因此任何人的电脑都可以作为仓库进行版本控制，
因此安全性和操作性较高！

-----------------------------------------------------------------------------------

原理：
依赖于三阶段管理

区名                    状态名                  意义
工作区  Working         Untracked未追踪         程序员自己的代码，与git系统没有关系
暂存区  Staging         Staged已索引            代码即将要提交，已经被git记录在案，文件内容被快照和注册（预处理工作，便于以后提交管理）
仓库区  Repostory       Commited已提交          代码被git管理，可以进行合并、分支、回溯、发布、上传

---------------------------------------------------------------------------------------

操作：

查看版本
git --version

查看常用命令帮助
git --help

查看完整命令列表
git help -a

查看具体命令的详细帮助
git help commit
git cmmmit --help
git commit -h       只显示参数列表和简单说明

把一个目录（项目所在文件夹）创建为仓库
git init
会自动创建一个.git目录，这个目录就是管理代码的暂存区和仓库区！

查看当前工作区状态
git status
红色
    显示的是Untracked状态的文件（需要加入暂存区）
    或者是该文件被暂存后又被修改（需要重新加入暂存区）
绿色
    显示的是Staged状态的文件（已经加入暂存区）

将代码文件加入暂存区
git add Readme.txt
git add Readme.txt index.htm *.js src/
git add .
git add --all
git add -A

移除不希望被暂存的内容（移至工作区，之后不会被提交，因为只有被暂存区管理的内容才会被提交到仓库区）
git rm --cached Git-2.18.0-64-bit.exe hello.txt      // 移除一个或多个文件
git rm --cache -r src                                // 移除一个目录（包括里面的所有内容）

创建一个忽略文件（里面的文件列表都被git忽略不被管理和提示）
.gitignore      // 一定要位于项目根下
以后每次git status就不会罗列出里面的文件在未暂存列表
以后直接git add .即可，不用考虑不希望被暂存提交的文件

注册个人信息
1、所有仓库的（全局）
    当前仓库如果没有指定个人信息，就用全局的
    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"

2、当前仓库的（个人）
    当前仓库如果指定个人信息，就用个人的
    git config user.name "tom"
    git config user.email "tom@123.com"

提交到仓库
git commit Readme.txt
git commit hello.txt haha.log -m "add hello.txt and haha.log files"
git commit -m "add src/"    // 不写文件列表，就表明将当前暂存区中的所有内容都被提交到仓库区

查看日志
git log
git log --author=frank      // 查找user.name和user.email包含frank的提交信息
git log --pretty=oneline    // 单行显示提交信息

删除仓库中的文件（本地也同时删除）
git rm haha.log
git commit -m "del haha.log"

如何你只希望删除仓库，但是保留本地，可以先将该文件重命名，然后git rm，然后将该文件再改为原名加入.gitignore列表！

重命名仓库/本地文件
先改本地文件名
执行一次git status看看变化
然后git add .
执行一次git status看看变化
最后git commit -m ""

比较暂存区和工作区
git diff GitReadme.txt

比较暂存区和仓库区
git diff --cached GitReadme.txt

比较工作区和仓库区
git diff head GitReadme.txt

-----------------------------------------------------------------

回溯版本
git reset --hard c46907     // 6位提交标识符

注意：
一旦回溯，那么git log只能查看你回溯点之前的提交信息，不能查看回溯自身信息！
如果要查看整个操作历史（包括历次回溯信息），使用git reflog！

-----------------------------------------------------------------

克隆仓库
先进入仓库
再git clone . ../Git2

注意：只会复制当前仓库中托管的内容（.gitignore里的文件和不在仓库中的文件是不会被克隆的）

---------------------------------------------------------------------

在团队协作中，一个项目是多路行进的！
一个团队负责新功能的研发
一个团队负责bugs的修复

它们所依赖的代码都是同一个项目

这时对代码的修改就会产生冲突或者不可避免的问题！

因此，我们每个团队可以将项目做一个镜像，然后在这个镜像上去修改，这样各自的修改 ，既可以保持在原项目上操作，又不会在开发过程中影响对方！

那么这个所谓的开发镜像，我们就称作分支！

当项目初始化仓库，就会自动生成一个主分支master！

查看当前项目中的分支
git branch

在当前分支上切出一个新的分支（其实就是当前分支的镜像）
git branch new_feature

切换/检出分支
git checkout new_feature

切出并检出分支（相当于git branch + git checkout）
git checkout -b test

删除分支
git branch -d test
git branch --delete test

重命名分支
git branch -m test test2

注意：
不能删除当前正在使用的分支！
如果某个分支已经被你修改提交到仓库，那么在没有合并到主分支前，不能删除！

强制删除尚未合并（但是有修改并已提交到仓库）的分支
git branch -D test

分支合并（将new_feature和fix_bugs合并到master）
git checkout master
git merge new_feature
git merge fix_bugs

如果产生冲突，根据提示找到冲突的文件，然后删除自动识别冲突的代码部分（<<<<<< >>>>>>），编辑为最终提交的代码，再加入暂存再提交！

-------------------------------------------------------------------------------------------------

使用Git管理远程仓库
github 国外 https://github.com/
gitee 国内  https://gitee.com/（中文资源）

在本地仓库注册远程仓库的地址
git remote add wbs18111 https://github.com/yufeng2/wbs18111.git

查看本地仓库注册的远程地址
git remote -v
git remote --verbose

删除本地仓库的远程地址
git remote rm wbs18111
git remote remove wbs18111

上传本地仓库的分支到远程
git push wbs18111 master

上传本地仓库的分支到远程并重命名（如果远程fix_bug不存在相当于重命名，如果存在是合并）
git push wbs18111 fix_bugs:fix_bug

下载并合并远程分支到本地（注意当前你所正位于的分支！！！）
（如果当前你在本地正位于master执行下面的命令，相当于将远程的new_feature合并到本地所正位于的master）
（如果当前你在本地正位于new_feature执行下面的命令，相当于将远程的new_feature合并到本地的new_feature）
git pull wbs18111 new_feature

因此，如果当前你不论在哪个分支下，就是要下载远程new_feature并合并到本地new_feature，后面加上:new_feature（显式指明就是合并到本地的new_feature分支）
git pull wbs18111 new_feature:new_feature

删除远程分支
git push wbs18111 -d new_feature
git push wbs18111 --delete new_feature

远程分支重命名（没有直接方法）
1、先删除远程分支
2、再上传时重命名

查看远程有哪些分支
git remote show wbs18111

-------------------------------------------------------------------------------------------------

远程合并的冲突
=============

1、先将远程的分支pull到本地合并

2、在本地修改合并时遇到的冲突（提交到本地仓库）

3、再push到远程

---------------------------------------------------------------------------------

使用github去发布个人静态站点（只能是htm页面、css、js、图片资源等）

1、
    远程仓库名为wbs18111（Repository命名随意）
    将本地分支上传到远程的gh-pages（分支名一定如此）
    https://yufeng2.github.io/wbs18111/switcher.htm

    第一种方式可以设置任意多个个人仓库的发布页面！

2、
    远程仓库名为yufeng2.github.io（Repository一定如此）
    将本地分支上传到远程的master（分支名一定如此）
    https://yufeng2.github.io/switcher.htm

    第二种方式只能有一个（因为Repository就一定是yufeng2.github.io，当然远程只能有一个该名的仓库）

如果页面命名为index.htm，那么直接访问地址（默认作为路径首页显示）！

















