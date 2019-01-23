## How to connect Local repo with your Github

### 准备工作
**准备工作分为两部分：建立本地仓库与建立和github的链接**
#### 建立本地仓库
将本地git与github通过ssh相关联, 首先选择一个本地文件夹建立本地仓库
```git
$ git init  #build local repo
```

然后将该文件夹下的想要的文件加入本地仓库的缓存区，或者直接加入全部文件到缓存区。
缓存区的概念是我们对代码做出了修改，但是并没有完成或者不认为可以被认可，所以并不能算一个版本，不能正式提交到本地仓库。
```git
$ git add <file name>
or
$ git add -A
```

然后我们还需要一个步骤，将已经被加入的文件提交给本地仓库
```git
$ git commit -m "Note here"
```
-m 表明后面引号内的文字代表本次提交的说明，为了方便版本管理，准确无误地填写提交说明是必要的

#### 建立链接
建立SSH-KEY
```git
$ ssh-keygen -t rsa -C "youremail@example.com"
```
上面命令里面的邮箱一般为github的邮箱地址，然后将新建的ssh填入github **Account Setting** 里面的ssh选项。至此，与github的联机就建立了。

### 添加远程仓库
现在我们可以将本地仓库（repo）的内容与github上的远程仓库进行同步，防止数据丢失同时也可以与他人进行分享、管理。
一般情况下，一个本地仓库与一个远程仓库对应。在文件夹目录下运行git
```git
$ git remote add origin git@github.com:youremail/reponame.git
```
后面一长串是从github远程仓库页面上复制粘贴下来的链接。代码中Origin一般指代远程仓库。

如果想把远程仓库的文件下载下来，直接clone即可
```git
$ git clone
```

现在开始将本地仓库文件推送到远程仓库
```git
$ git push -u origin master
```
由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
```git
$ git push origin master
```

而我在第一次push的时候遇到了许多问题
**问题一**
```git
$ git push -u origin master
Warning: Permanently added the RSA host key for IP address 'xx.xx.xxx.xxx' to the list of known hosts.
To git@github.com:hahaha/ftpmanage.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:hahahah/ftpmanage.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
在push的时候遇到了问题，系统提示我先pull（拉取）。
pull这里表示从远程仓库拉取数据到本地仓库，为了进行匹配。
```git
$ git pull --rebase origin master
```

可能先需要这一次匹配才能完成push。代码提交顺序为commit -> pull(为了解决冲突) -> push





