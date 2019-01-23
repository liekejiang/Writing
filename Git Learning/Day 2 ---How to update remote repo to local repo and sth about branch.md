## How to update remote repo to local repo

我昨天通过下线编辑器更新了github上的笔记，但是我想要同步到本地，该怎么做呢？

首先我们需要学习几个新命令：

* git status（查看本地分支文件信息，确保更新时不产生冲突）

* git checkout – [file name] （若文件有修改，可以还原到最初状态; 若文件需要更新到服务器上，应该先merge到服务器，再更新到本地）

* git checkout [branch name] (当前切换到本地还是远程仓库)

* git branch （查看本地分支情况, 也可以用这个命令新建本地分支，后面跟新branch name）
* git branch -r（查看远程分支情况, ）

* git pull （直接将远程仓库同步到本地）


学习了上面的命令之后，使用**git pull**是最简单的办法，但是它隐藏了部分细节，尤其是分支的变化，参见[不要使用pull](https://www.oschina.net/translate/git-fetch-and-merge?print)。 取而代之，我们应该使用
* git fetch (download)
* git merge (合并分支)

因为你可以看到分支的变化过程，更加安全，需要养成习惯。具体的顺序为：
1. git statue (查看当前状态)

2. git fetch origin (从远程仓库下载)

3. git checkout master (切换到本地仓库)

4. git merge origin/master (合并)

5. git diff master origin/master (查看本地和远程的区别)

