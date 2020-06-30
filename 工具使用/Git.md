```
git init 创建新的 git 仓库

检出仓库（把代码克隆到本地）
执行如下命令创建一个本地仓库的克隆版本
git clone /path/to/repository  

如果是远端服务器上的仓库，用以下命令
git clone username@host:/path/to/repository
示例：git clone git@192.168.1.18:Henry_zou/wallet-for-ios.git


添加与提交
如果你代码修改好了也增加了很多文件
git add <filename>     添加指定文件到缓存区
git add *                      添加所有文件到缓存区

提交
git commit -m “代码提交信息”      现在代码已经提交到head，但还没到远端仓库
示例：git commit -m "add README"

推送改动 到服务器
git push origin master   
- git push -u origin master

把本地项目  推到git远程服务器里
git remote add origin <server>


创建一个分支
git branch <分支名>

切换到指定分支
git checkout -b <分支名>

查看所有分支
git branch -a

切换到主分支
git checkout master

删除指定分支
git branch -d <分支名>

创建分支后保存在本地缓存区远程是没有的 所以需要将本地分支推送到服务器上
git push origin <分支名>


更新与合并

更新本地仓库为最新改动(把服务器代码更新本地)
git pull


git merge <分支名>  合并指定分支到当前分支


同步指定文件到本地缓存区(如果不小心改错了xx.c 需要把服务器的文件同步下来)
git checkout -- xx.c

丢弃所有本地改动与提交，
git fetch origin

```
