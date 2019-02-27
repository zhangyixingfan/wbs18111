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














