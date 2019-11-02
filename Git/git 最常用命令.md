在 mac 上 新建一个 文件夹，并进入该文件夹

```js
git branch -a // 查看本地和远程分支
git add -A  // 一次性提交所有修改内容到 暂存区
git checkout -b issue-1 // 创建新的 bug 分支
git merge dev // 在 master 分支下，执行 合并 dev 分支的命令
git merge --no-ff -m "merge with no-ff" dev // 准备合并dev分支，
git log --pretty=oneline // 美化log 显示
git branch -d dev // 删除 dev 分支，合并后，就可以删除 dev 分支了
git switch -c dev // 切换并创立新的分支 dev。 使用新的git switch命令，比git checkout要更容易理解。
git remote add origin git@github.com:RenPeng-CN/learngit.git  // 添加远程仓库
git push -u XiaoMiStore master // 推向 github
rm -f ./.git/index.lock  // git 清除 另一个git进程似乎在这个仓库中运行
```



```js
git init // 在指定的文件夹里，执行 git 初始化
ls -ah // 可以看到 创建 git 产生的 隐藏文件
// 在终端窗输入  vi readme.txt  再回车 会跳出文本编辑窗口
//在文本里输入一些内容后 按  :qw 保存并退出
ls // 查看当前文件夹是否有 readme.txt 文件
git add readme.txt // 回车后，发现没有变化，但已经从工作区提交到缓存区了
git commit -m 'wrote a readme file' // git 提交到当前分支 -m 后面的内容为 版本解释，可以自定义输入
git log // 查看修改记录
git log --pretty=oneline // 美化log 显示
git reset --hard HEAD^ // 退回到上一个版本
git reset --hard 46b6b7667f0c032aa53bdc99a23b8115d741aab0  // 跳到指定版本号的版本
git reflog  //用来记录你的每一次命令：可以找到之前操作过的命令
git status // 查看 git 状态
rm .DS_Store // 删除系统自动生成 的 没用的文件
cat readme.txt // 显示readme.txt 文件内容
git diff HEAD -- readme.txt // 查看工作区和版本库里面最新版本的区别
git checkout readme.txt // 把工作区的修改全部撤销
//git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令
git reset HEAD readme.txt // 把暂存取的修改撤销掉，重新放回工作区
git rm test.txt // 如果你删除了工作区的 test.txt 文件，git status 会提醒你，你就可以删除 分支里的该文件，并重新 commit一次
git commit -m "remove test.txt"
git remote add origin git@github.com:RenPeng-CN/test.git // 在 github 上建立仓库，获取 ssh 地址 ，添加远程仓库
git push -u origin master // 把本地 git 内容推送到远程，origin 是远程库的默认叫法，也可以改成其他名字，由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

// 提示：如果不能访问 github ，就需要去 github 配置 跟你电脑相关的 公钥 私钥
git push origin master // 从现在起，只要本地作了提交，就可以通过命令：把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

git clone git@github.com:RenPeng-CN/test.git // 如果先在 github 上建立了 库，就 在本地建立文件夹，然后就可以 克隆到本地来，$ cd gitskills  $ ls   README.md

git checkout -b dev // 创建一个新的 dev 分支，并切换到 dev 分支上 
//以上这条命令 相当于 
 git branch dev // 创建新 分支
 git checkout dev  // 转换到新分支
Switched to branch 'dev'

git branch // 查看分支情况
// 我们在 dev 分支 修改 readme.txt 文件后， add 并且 commit 后，返回 master 分支
git checkout master // 返回master 分支后，发现 readme.txt 没有改
git merge dev // 在 master 分支下，执行 合并 dev 分支的命令
git branch -d dev // 删除 dev 分支，合并后，就可以删除 dev 分支了
git switch -c dev // 切换并创立新的分支 dev。 使用新的git switch命令，比git checkout要更容易理解。
git log --graph --pretty=oneline --abbrev-commit // 查看 log
  //解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。用git log --graph命令可以看到分支合并图。
  
git merge --no-ff -m "merge with no-ff" dev // 准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：
  
git stash //把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
git stash list // 查看储藏起来的工作现场
git stash apply // 一、恢复工作现场，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
git stash pop // 二、用这种方式恢复，恢复的同时把stash内容也删了：
git stash list // 在用此方法查看，看不到任何 stash 内容了
  //你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：
 git stash apply stash@{0} // 恢复指定的 stash

//在master分支上修复了bug后，我们要想一想，dev分支是早期从master分支分出来的，所以，这个bug其实在当前dev分支上也存在, 我们只需要把4c805e2 fix bug 101这个提交所做的修改“复制”到dev分支。注意：我们只想复制4c805e2 fix bug 101这个提交所做的修改，并不是把整个master分支merge过来。为了方便操作，Git专门提供了一个cherry-pick命令，让我们能复制一个特定的提交到当前分支：
git cherry-pick 4c805e2 // 把在 master 上修复 bug 的编号 4c805e2 在 dev分支上也运行一下，dev 的bug 也就改了
git branch -D feature-vulcan // 强行删除一个分支，尽管有内容没有被合并，也被删除

git remote // 查看 远程库 的信息
git remote -v // 显示更详细的信息
git remote add pb git://github.com/RenPeng-CN/ticgit.git // 添加远程库
git remote rename pb paul // 给远程库重命名
git remote rm paul // 删除远程库

git push origin master // 推送 master 分支
git push origin dev // 推送 dev 分支
git checkout -b dev origin/dev // 创建本地dev分支：
git branch --set-upstream-to=origin/dev dev // 设置dev和origin/dev的链接：
git pull // 在 pull 远程 dev，就会自动合并远程和本地的 dev 分支

git rebase //(变基) rebase操作可以把本地未push的分叉提交历史整理成直线；rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

git tag v1.0 // 切换到需要打标签的 分支上，执行命令 给本分支打上标签
git tag // 查看 所有标签
git log --pretty=oneline --abbrev-commit // 现在已经是周五了，但应该在周一打的标签没有打，怎么办？方法是找到历史提交的commit id，然后打上就可以了
git tag v0.9 f52c633 // 对指定的 commit 编号进行 打标签
git show v0.9 // 查看标签信息：
git tag -a v0.1 -m "version 0.1 released" 1094adb // 创建带有说明的标签，用-a指定标签名，-m指定说明文字：
git show v0.1 // 可以看到说明文字：
//  注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。
git tag -d v0.1 // 如果标签打错了，也可以删除：
git push origin v1.0 // 推送某个标签到远程，
 git push origin --tags //一次性推送全部尚未推送到远程的本地标签
 
 // 如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
git tag -d v0.9 // 删除本地 标签
git push origin :refs/tags/v0.9 // 删除远程 标签

// 在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。
//不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore

//最后一步就是把.gitignore也提交到Git，就完成了！当然检验.gitignore的标准是git status命令是不是说working directory clean。
git add -f App.class // 被忽略的文件，可以用-f强制添加到Git：
git check-ignore -v App.class // Git会告诉我们，.gitignore的第3行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。

git config --global alias.st status //告诉Git，以后st就表示status：

git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"  // 甚至还有人丧心病狂地把lg配置成了以上长条

 cat .git/config  // 每个仓库的Git配置文件都放在.git/config文件中：别名就在[alias]后面，要删除别名，直接把对应的行删掉即可。
cat .gitconfig //当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中：

```











































