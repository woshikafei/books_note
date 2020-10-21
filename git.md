## 安装配置、其他

```
配置个人信息
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
配置ssh，id_rsa.pub公钥
ssh-keygen -t rsa -C "youremail@example.com"

拉取远程仓库
git clone git@github.com:michaelliao/gitskills.git
将当前目录变为仓库
git init
关联远程仓库
git remote add origin git@github.com:michaelliao/learngit.git

查看状态
git status
git log
git log --pretty=oneline
git log --graph --pretty=oneline --abbrev-commit
git diff xxx.txt
git reflog                回退log
git remote                查看远程信息，-v详细信息
git tag                   查看所有有标签的commit
git show <tag name>       查看该tag的信息

基础操作
git add <file>            文件放入暂存区
git rm <file>              
git commit -m "add file"  放入工作区
git push origin master    版本push
git push -u origin master 与远程仓库的master关联并push
git branch --set-upstream-to=origin/dev dev  与远程建立链接
git pull
git push origin <tagname>  把指定tag推送到远程
git push origin --tags     把所有tag都推送
git push origin :refs/tags/v0.9 删除指定远程tag

回退版本
git reset --hard HEAD^     工作区的前一个版本
git reset --hard HEAD~100  前100个版本
git reset <commit id>
git checkout -- readme.txt 丢弃工作区的修改
git reset HEAD <file>      丢弃暂存区的修改

分支
git branch dev             创建分支
git checkout dev           切换分支 git switch dev
git checkout -b dev        创建并切换分支 git switch -c dev
git checkout -b dev origin/dev  创建远程origin的dev分支
git branch                 查看所有分支
git merge dev              在master分支上，把dev合入
git merge --no-ff -m "merge with no-ff" dev  以创建而非指针的方式合入
git branch -d <name>       删除分支（FF模式下只改指针，故不能删分支）-D强行
git rebase                 把本地提交的历史修改，整理成一条直线，方便看
git tag v1.0               给commit打标签
git tag <tag name> <commit id>
git tag -a <tag name> -m <comment> <commit id>
git tag -d <tag name>      删除tag

暂存挂起
git stash                   挂起
git stash list              
git stash pop               唤起并pop
git stash apply             只唤起不删除
git stash drop              删除
git stash apply <stash id>  指定名字stash@{0}
git cherry-pick <commit id> 复制特定提交到当前分支（比如切出去修bug）


```











