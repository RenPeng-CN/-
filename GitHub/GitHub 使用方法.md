### **Github上 fork了别人的代码 本地更新主分支代码**

在GitHub上我们会去fork别人的一个项目，这就在自己的Github上生成了一个与原作者项目互不影响的副本，自己可以将自己Github上的这个项目再clone到本地进行修改，修改后再push，只有自己Github上的项目会发生改变，而原作者项目并不会受影响，避免了原作者项目被污染。但经过一段时间， 有可能作者原来的代码变化很大， 你想接着在他最新的代码上修改， 这时你需要合并原作者的最新代码过来， 让你的项目变成最新的。 
1、先克隆项目到本地： 
Git clone https://github.com/iakuf/mojo 
cd mojo 
2、添加原作者项目的 remote 地址， 然后将代码 fetch 过来 
git remote add sri https://github.com/kraih/mojo 
git fetch sri 
‘sri’相当于一个别名 
查看本地项目目录： git remote -v 
3、合并 
git checkout master 
git merge sri/master 
如果有冲突的话，需要丢掉本地分支： 
git reset –hard sri/master 
4、这时你的当前本地的项目变成和原作者的主项目一样了，可以把它提交到你的GitHub库 
git commit -am ‘更新到原作者的主分支’ 
git push origin 
git push -u origin master -f –强制提交

[http://blog.csdn.net/u013647382/article/details/53400530](http://blog.csdn.net/u013647382/article/details/53400530)

