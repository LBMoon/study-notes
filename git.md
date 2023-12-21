#### 一、git（版本控制工具）

##### 1.配置用户信息

- 打开 GIt Bash
- 设置用户信息
	- git config --global user.name "MoonSky"
	- git config --global user.mail "2277390949@qq.com" 邮箱可以不存在
- 查看配置信息
	- git config --global user.name
	- git config --global user.email
##### 2.为常用指令配置别名（可选）

- 打开用户目录（user/MoonSky）创建.bashrc文件，或者使用touch ~/.bashrc来创建
- 在.bashrc 文件中输入：

```
# 用于输出git提交日志
alias git-log = 'git log --pretty=online --all --graph --abbrev-commit

# 用于输出当前目录所有文件及基本信息
alias ll='ls -al'
```

- 打开 gitBash 执行 source ~/.bashrc
##### 3.解决gitBash中文乱码问题

- 打开GitBash执行下边命令
```
git config --global core.quote.path false
```

- ${githome}/etc/bash.bashrc文件最后加入下面两行
```
export LANG = "zh_CN.UTF-8"
export LC_ALL="zh_CN.UTF-8
```
#### 二、git的使用
##### 1.获取本地仓库

- 在电脑任意位置创建一个空目录作为本地的Git仓库
- 进入这个目录，右键打开GitBash 窗口
- 执行命令 git init
- 如果创建成功了后可在文件夹下看到隐藏的git目录

##### 2.基础操作指令

Git工作目录下对于文件的**修改**(增加、删除、更新)会存在几个状态，这些修改的状态会随着我们执行Git的命令而发生变化。工作目录指的就是上边创建的目录

![[Pasted image 20231220200712.png]]

本章节主要讲解如何使用命令来控制这些状态之间的转换
- git add (工作区-->暂存区)
- git commit(暂存区-->本地仓库)

###### 2.1 查看修改的状态 (status)

- 作用: 查看的修改的状态 (暂存区、工作区)
- 命令形式: git status

###### 2.2 提交暂存区到本地仓库(commit)

- 作用: 提交暂存区内容到本地仓库的当前分支
- 命令形式：git commit -m 注释内容' 
###### 2.3 添加工作区到暂存区(add)

- 作用: 添加工作区一个或多个文件的修改到暂存区
- 命令形式:git add 单个文件名|通配符
	- 将所有修改加入暂存区: git add .

###### 2.3 查看提交日志(log)

在一、2中配置的别名git-log就包含了这些参数，所以后续可以直接使用指令git-1og

- 作用:查看提交记录
-  命令形式: git log [option]
	- --all 显示所有分支
	- --pretty=oneline 将提交信息显示为一行
	- --abbrev-commit 使得输出的commitld更简短
	- --graph 以图的形式显示

###### 2.6 版本回退

- 作用:版本切换
- 命令形式: git reset --hard commitID
	- commitID 可以使用git-log或git 1og指令查看
- 如何查看已经删除的记录?
	- git reflog
	-  这个指令可以看到已经删除的提交记录

###### 2.7 添加文件到忽略列表

一般我们总会有些文件无需纳入Git 的管理，也不希望它们总出现在未跟踪文件列表。通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。在这种情况下，我们可以在工作目录中创建一个名为gitignore 的文件(文件名称固定) ，列出要忽略的文件模式。下面是一个示例:
```
# no .a files
*.a
# but do track lib.a, even though you're ignoring .a files above
!1ib .a
# only ignore the ToDo file in the current directory, not subdir/ToDo  # 这里应该是引号
/TODO
# ignore a11 files in the build/ directory
bui1d/
# ignore doc/notes ,txt, but not doc/server/arch.txt
doc/*.txt
# ignore a11 .pdf files in the doc/ directory
doc/**/*.pdf
```


##### 3.分支（branch）

几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

3.1 查看本地分支
- 命令: git branch

3.2 创建本地分支
- 命令: git branch 分支名

3.3 切换分支(checkout)
- 命令: git checkout 分支名

我们还可以直接切换到一个不存在的分支 (创建并切换)
- 命令: git checkout -b 分支名

3.4 合并分支(merge)
一个分支上的提交可以合并到另一个分支
命令: git merge 分支名称（将分支名称合并到当前的分支上，一般先切换到master上然后再将其他分支合并上来）

3.5 删除分支

**不能删除当前分支，只能删除其他分支**

- git branch d [b1] 除分支时，需要做各种检查
- git branch -D [b1] 不做任何检查，强制删除

##### 4. git和github的互动

###### 4.1 创建远程仓库别名

- 查看当前所有远程地址别名
	- 命令：git remote -v
- 别名 远程地址
	- 命令：git remote add 

###### 4.2 推送本地分支上的内容到远程仓库

- 命令：git push 别名 分支

###### 4.3 将远程仓库对于分支的最新的内容拉取下来与本地分支合并

- 命令：git pull 远程仓库地址别名 远程分支名

###### 4.4 将远程仓库的内容克隆到本地

- 命令：git clone 远程地址 
- 这个命令相当于pull ，init，commit的三合一命令




#### 三、git和IDEA


