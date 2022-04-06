### git和github（git的一个代码托管中心）（分布式版本控制工具）

#### 一、git的功能

##### 1、协同修改

##### 2、数据备份（可以保存提交过的每一个历史状态）

##### 3、版本管理（文件系统文件快照的方式）

##### 4、权限控制（对开发团队以外贡献的代码可以审核*<u>-git独有</u>*）

##### 5、历史记录（修改人、修改时间、修改内容、日志，**可以将文件恢复到某一个历史状态**）

##### 6、分支管理（支持开发团队在工作过程中多条生产线同时推进任务）

#### 二、git的优势

##### 1、大部分可以在本地完成，不需要联网

##### 2、完整型的保存（哈希运算）

##### 3、尽可能的增加数据而不是删除或者修改数据

##### 4、与Linux命令全面兼容

#### 三、git的结构

##### 1、工作区（写代码），通过**<u>git add</u>**添加到暂存区

##### 2、暂存区（临时存储），通过**<u>git commit</u>**添加到本地库

##### 3、本地库（包含历史版本）

![image-20210509105023722](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509105023722.png)

#### 四、git和github的区别

——git是**分布式版本控制工具**，而github是一个git的一个**代码托管中心**

——对于局域网环境下，可以有**gitlab**，对于网络环境（外网）有**github**和**码云**。

——**git的主要功能就是为了管理本地库和远程库之间的联系。**

##### **git本地库和远程库工作的方式**

###### ***方式一：***

![image-20210508193801243](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210508193801243.png)

###### ***方式二***：

![image-20210508194308661](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210508194308661.png)

#### 五、git命令行操作

##### **1、本地库初始化**

###### （1）*命令以及效果：*

```git
git init
# 在e:/git/weChat/的目录下初始化一个git仓库，初始化以后生成一个.git文件
```

![image-20210508195755923](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210508195755923.png)

```git
ls -lA
# 显示隐藏的.git目录
```



![image-20210508195940404](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210508195940404.png)

```git
ll .git/
# 显示.git目录下所有的文件
```

![image-20210508200152524](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210508200152524.png)

**注意：**<u>git目录中存放的是本地库相关的子目录和文件，不要删除也不要胡乱修改</u>

###### **（2）设置签名**

**→**    *形式*

​	    	用户名：tom

​			Email地址:2277390949@qq.com

**→**	作用：区分不同开发人员的身份

**→**	辨析：**这里设置的签名和登录远程库（代码托管中心）账号密码没有任何关系**。

**→**	命令：

**$    项目级别/仓库级别**（仅在当前本地范围内有效）

```git
git config user.name tom_pro
git config user.email 2277390949@qq.com
# 实际修改在了.git/config的文件中
```

![image-20210508203925685](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210508203925685.png)

**$    系统用户级别**（登录当前操作系统的用户范围）

```git
git config --global user.name MoonSky
git config --global user.email 22773909492qq.com
# 文件保存在了c:/Users/22773/.gitconfig 这个文件中
```

![image-20210509100140544](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509100140544.png)

**$ 级别的优先级**

 	@ 就近原则：项目优先级 > 系统用户优先级，二者都存在时采用项目优先级的签名

​	 @ 如果只有系统用户级别的签名，就以系统用户级别的签名为准

​	 @ 二者都没有不允许

##### 2、基本操作

###### (1) 添加提交以及查看状态

**查看状态命令：**

```git
git status
'''
On branch master  处于master的分支上
No commits yet    还未提交过文件（本地库是空的）
nothing to commit 没有东西能提交（暂存库是空的）
'''
```

![image-20210509100938488](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509100938488.png)

**新建一个文件**

```git
vim hello.py
git status
'''
其他一样
hello.py 变红，表示还没有被放入到暂存区
'''
```

![image-20210509101209117](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509101209117.png)

**将文件上传到缓存区**

```git
git add hello.py
'''
有一个警告，是关于windows和linux的句末结束的区别
'''
```

![image-20210509101434737](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509101434737.png)

**将文件从缓存区移除**

```git
git rm --cached hello.py
git status
```

![image-20210509101900271](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509101900271.png)

**调加到本地库**

```git
git commit hello.py
git status
'''
git commit hello.py 会启动vim 来写入本次提交的内容
'''
```

![image-20210509103325231](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509103325231.png)

**然后修改hello.py文件**

```git
vim  hello.py
git status
'''
加一行代码
print("hello,world")

出现新的提示
hello.py文件已经被修改 可以使用git add和git commit 两个命令将文件重新提交到暂存区
					 也可以使用git commit -a 将文件重新提交到暂存区
'''
```

![image-20210509103622736](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509103622736.png)

**将修改的文件重新提交到暂存区**

```git
git add hello.py
git status
```

![image-20210509103916560](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509103916560.png)

**将修改的文件从暂存区提交到本地库**

```git
git commit -m "this is my second commit,modified hello.py" hello.py
'''
这个方法可以不用进入vim编辑器
'''
```

![image-20210509104723514](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509104723514.png)

**总结**

a、查看状态(查看工作区、暂存区的状态)

```
git status
```

b、添加操作（将工作区的"新建/修改"添加到暂存区）

```git
git add [file name]
```

c、提交操作（将暂存区的内容提交到本地库）

```git
git commit -m "commit message" [file name]
```

###### (2) 查看历史记录（log命令）

**最完整的查看(可能多屏，空格向下翻页，b向上翻页，q退出)**

```git
git log
```

![image-20210509111143950](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509111143950.png)

**将每一条log只显示一行**

```git
git log --pretty=oneline
```

![image-20210509111348924](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509111348924.png)

**将上边的显示精简**

```git
git log --oneline# 只能查看HEAD之前的版本，不能查看后边的
```

![image-20210509111759404](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509111759404.png)

**能查看返回哪一个版本需要几步的命令，head@{移动到当前版本需要的步数}**

```
git reflog
```

![image-20210509111502142](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509111502142.png)

###### （3）版本的前进和后退（reset命令）

**本质**

![image-20210509112141636](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509112141636.png)

**方法**

**<1> 基于索引值操作（推荐）**

```git
git reset --hard [局部索引值]
git reset --hard fdf9d81
'''
前进后退都用git reset --hard 来完成
'''
```

![image-20210509112936307](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210509112936307.png)

**<2> 使用^符号：只能后退**

​		·git reset --hard HEAD^

​		·注：一个^只能后退一步，n个^表示后退n步

```git
git reset --hard HEAD^
tail -n 2 hello.py # 查看hello.py文件的最后两行
```

![image-20210510112157628](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210510112157628.png)

**<3> 使用~符号:   只能后退**

​		·git reset --hard HEAD~n

​		·注:表示后退n步

```git
git reset --hard HEAD~2
tail -n 2 hello.py # 查看hello.py文件的最后两行
```

![image-20210510112844757](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210510112844757.png)

**对比reset不同命令的三个参数**

**--soft 参数**

​	仅在本地库移动HEAD指针

**--mixed参数**

​	在本地库移动HEAD指针

​	重置暂存区（相当于暂存区同本地库的HEAD一起移动）

**--hard参数**

​	在本地库移动HEAD指针

​	重置暂存区

​	重置工作区

###### （4）永久删除文件后找回

将文件在工作区写好提交到暂存区和工作区以后，就会产生一个版本日志，即使删掉了文件，只要回退一个版本，就可以重新找到文件。

a、新建一个文件，将其提交到暂存区和工作区

```git
vim hi.py
git add hi.py
git commit -m "new hi.py" hi.py
```

![image-20210510122356055](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210510122356055.png)

b、删除hi.py文件，查看其状态

```git
rm hi.py
git status
```

![image-20210510122452173](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210510122452173.png)

c、将删除hi.py这条记录添加进本地库

```git
git add hi.py 
git commit -m "deleted hi.py" hi.py
git status
```

![image-20210510122811796](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210510122811796.png)

d、查看版本日志并且通过后退版本恢复hi.py

```git
git reflog 
git reset --hard [版本索引]
cat hi.py
```

![image-20210510123206952](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210510123206952.png)

