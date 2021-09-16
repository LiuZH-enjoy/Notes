# GitHub简单使用指南

### 1. 将本地代码上传到GitHub上的远端仓库（完整过程）。

​	**直接截取知乎上的回答，先看那个详细的回答，篇幅较长，在笔记最后**。



### 2. 将本地代码上传到GitHub上的远端仓库（浓缩版）

#### 2.1 首先要对git进行全局设置 

**这里其实在知乎回答就已经讲到了，在这里主要是为了强调，如果你在另外一台电脑上想提交时，就需要把当前电脑与自己的GitHub账号相关联。**

指定用户和邮箱，Git是分布式版本控制系统，所以需要填写用户名和邮箱作为一个标识，用户和邮箱为你github注册的账号和邮箱；有了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然你也可以对某个仓库指定的不同的用户名和邮箱。

```bash
git config --global user.name "ASxx" 
git config --global user.email "123456789@qq.com"
```

#### 2.2 为不同电脑设备设置SSH key

​	**参考知乎回答**，同理在另一台电脑上生成密钥对，在自己GitHub上的SSH列表中新建一个SSH key，然后上传公钥到这个key里面。

​	**另外有个很奇怪的地方是，好像由于'master'有主人的意思，有点冒犯黑人？？？？现在GitHub上默认的主分支改成了'main'，但我又发现默认主分支为'main'的时候会遇到一些奇奇怪怪，不方便的问题(具体我也不知怎么说，貌似GitHub有不少东西还是按照master为主分支这样去设置，就不太理解，得继续查资料)。**

​	**官方给出default branch的含义是这样的：You can choose the default branch for a repository. The default branch is the base branch for pull requests and code commits.** **简而概之，就是在pull和commit时都比较重要的**。

​	**所以我在本地新建一个项目，在GitHub也新建了一个对应repository后，在push上去之前，干脆就直接在GitHub上把主分支改回master了，如下图所示，在setting的branches里面，修改默认分支的名字为master.**


#### 2.3 建立本地仓库并关联远程仓库

**通常我们把代码弄上GitHub基本属于以下两种情况之一：**

1. **GitHub上已经有项目（远端仓库）**，我们直接git clone 下来，建立本地仓库，实现对远端仓库进行更新。
2. **Github上没有项目**，我们自己在本地新建一个本地仓库，在GitHub上新建一个远端仓库，自己将他们关联起来，然后实现本地仓库对远端仓库的更新。

**如果是第一种情况，在进行了2.1和2.2的操作后**，可以参考下面执行命令：

```bash
git clone '项目链接'
git init //把这个目录变成Git可以管理的仓库
git remote add '项目链接' //关联远程仓库，就是你想要更新的项目，这里要把链接改成你的仓库
git add . //不但可以跟单一文件，还可以跟通配符，更可以跟目录。一个点就把当前目录下所有未追踪的文件全部add了 
git commit -m "first commit" //把文件提交到仓库，这里只是本地的仓库，还没push到远程仓库。
git push -u origin master //把本地库的所有内容推送到远程库上，这里push到了master分支；
```

**如果是第二种情况，其实差别不大，不用 git clone**

```bash
git init //把这个目录变成Git可以管理的仓库
git add README.md //文件添加到仓库（也可以在GitHub上新建repository时添加，它有这个选项的）
git remote add origin git@github.com:wangjiax9/practice.git //关联远程仓库，就是你想要更新的项目，这里要把链接改成你的仓库，只是一个例子
git add . //不但可以跟单一文件，还可以跟通配符，更可以跟目录。一个点就把当前目录下所有未追踪的文件全部add了 
git commit -m "first commit" //把文件提交到仓库，这里只是本地的仓库，还没push到远程仓库。
git push -u origin master //把本地库的所有内容推送到远程库上，这里push到了master分支；
```

**需要注意的是，像上面例子一样，我查阅到不少教程都是让你直接push到master分支，如果你自己没有建立一个master分支的话，那么它会帮你新建一个；但是这样就造成了你每次push时，都需要选择push到master分支（因为你第一次就是push到master分支的，而它不是default的；我试过直接到push到main好像有问题，所以就有了上面就干脆直接改成master为default分支了）**，**不然的话每次push和pull都需要选择maser分支，过于麻烦。**



> **如果想要push到一个新的分支，例如main，可以尝试以下方法：**

```bash
1. 先使用git checkout -b main 在本地信新建main分支，并进入到main分支
2. 然后git push 的时候如果出现一堆hint那个问题。是因为在github那个远程的main仓库中，有你本地main没有的文件，需要先pull到本地，再把本地的push上去。（我的例子是远程库创建了一个README.md文件，而本地却没有。因此需要先将Github上面的README拉取下来。）
3. 使用命令：git pull origin main --allow-unrelated-histories
4. 然后再：git push -u origin main
这样在GitHub上就可以看到main分支有更新了。
```



**而在push的时候，有可能会遇到无法push的情况，比如我遇到过下面这种：**

![在这里插入图片描述](https://camo.githubusercontent.com/ae2334ed4115c836bce9465a4d0f1df1eec55b651863346b6aaf4724be1eec8c/68747470733a2f2f696d672d626c6f672e6373646e696d672e636e2f32303230303931373130353735373436312e706e67237069635f63656e746572)

一堆hint提示，查了一下主要是因为：**这本地仓库与线上仓库不一致产生的报错！** **网上搜罗了一下，主要有以下解决办法,我是通过方案二得到解决的**

1. **解决方案一**

产生的原因是本地仓库与线上仓库的内容不匹配，或者说本地相对于远程不是最新，先pull更新本地，再把自己的push上去。** ps：如果不想代码清光光的话,切忌使用-f 强制push（希望人没事🙏）


命令：

```
git pull xxx(网址或者别名) xxx分支
```

2. **解决方案二**

若方案一为解决push仍然失败，尝试使用以下代码重新pull

命令：

```
git pull xxx(网址或者别名) xxx分支 --allow-unrelated-histories
```

而在输入该命令后，它给我弹出了这样的界面：

![这里写图片描述](https://img-blog.csdn.net/20170718091526810?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvV2Jpb2ty/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这好像就是**Linux**的东东，已经忘了怎么操作了，又搜了一下：

**git pull命令之后git如上报错，解决步骤，可以不管(直接下面3,4步)：**，这完成了pull之后就可以push了。

```
//1.按键盘字母 i 进入insert模式

//2.修改最上面那行黄色合并信息,可以不修改

//3.按键盘左上角"Esc"

//4.输入":wq",注意是冒号+wq,按回车键即可
```

3. **解决方案三**（**这个貌似就是强制push，大家都不太建议**）

若github项目提交失败，报错： master -> master (non-fast-forward)

命令

- 先执行git pull
- 然后再执行 git push --force origin master（代替git push origin master）



###  3. 在Vscode里面进行代码同步(包括pull拉取)

##### 基本步骤

在**vscode的源代码管理工具中**，可以将**更改的代码**提交到**暂存区**(`git add`)，接着可把**暂存区的代码**提交到**本地的仓库**(`git commit -m"注释消息"`)，最后可以**推送（不要忘记推送了）**当前最新分支到远程仓库(即github远端仓库里面)；在github上就能看到更新。

**当在另外一台进行操作时，在已经建立好本地仓库的前提下，可以直接将代码从远端pull回来进行更新，这里可以直接用vscode的源代码管理器选拉取，亲测有效！！！我用两台电脑搞过，可以的。也可以直接用git bash 终端进行操作。**



### 4. git commit 和 git push的区别

git commit操作的是本地库，git push操作的是远程库。

git commit是将本地修改过的文件提交到本地库中。
git push是将本地库中的最新信息发送给远程库。

那有人就会问，为什么要分本地commit和服务器的push呢？

因为如果本地不commit的话，修改的纪录可能会丢失。
而有些修改当前是不需要同步至服务器的，所以什么时候同步过去由用户自己选择。什么时候需要同步再push到服务器。



### 5.  常用命令

用到的命令：

克隆仓库：git clone git地址
初始化仓库：git init 

添加文件到暂存区：git add -A
把暂存区的文件提交到仓库：git commit -m 提交信息
查看提交的历史记录：git log --stat

工作区回滚：git checkout filename
撤销最后一次提交：git reset HEAD^1

以当前分支为基础新建分支：git checkout -b branchname
列举所有的分支：git branch
单纯地切换到某个分支：git checkout branchname
删掉特定的分支：git branch -D branchname
合并分支：git merge branchname



### 6. .....

