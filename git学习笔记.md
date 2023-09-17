# 第一章 基础命令

## 1.0 git init

执行完git init 后会产出一个(.git目录)

```shell
tree .git
```

输出结果: (9 directories, 4 files)

```shell
.git
├── branches
├── config
├── description
├── HEAD
├── hooks
├── info
│   └── exclude
├── objects
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags

9 directories, 4 files
```

> config 	存放git仓库的配置信息
>
> index 	存放文件名
>
> objects 下存放文件的内容
>
> 

## 1.1 创建一个文件

- 通过输出重定向创建一个文件(也可以用vim或其他方式)

```shell
echo "hello file1">file.txt   # 创建一个文件
git status 			# 工作区存在一个红色文件，显示未被追踪
git add file.txt 	# 将文件从工作区存放到暂存区
tree .git/			# 查看现在的目录树
```

结果为：显示(10 directories, 6 files)

```shell
.git/
├── branches
├── config
├── description
├── HEAD
├── hooks
├── index
├── info
│   └── exclude
├── objects
│   ├── a5
│   │   └── 22c7542d9ee6732d5662e9d1d02f4cb12a23d7
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags

10 directories, 6 files
```

- 查看index中存放的文件名

```shell
git ls-files 
```

- 查看index中存放的文件详细信息

```shell
git ls-files -s
```

输出结果：

```shell
liumeng@ubuntu64:~/workspaces/selfGit$ git ls-files -s
100644 a522c7542d9ee6732d5662e9d1d02f4cb12a23d7 0	file.txt
```

> 100644 是文件的类型(普通文件，可执行文件，二进制文件的类型)
>
> a522c7542d9ee6732d5662e9d1d02f4cb12a23d7 表示文件的内容
>
> 0 未知
>
> file.txt  文件名

## 1.2 创建第二个文件

```shell
echo "file2">file2.txt
git add .
git status
```

查看结果：显示两个文件

```shell
liumeng@ubuntu64:~/workspaces/selfGit$ git ls-files -s
100644 a522c7542d9ee6732d5662e9d1d02f4cb12a23d7 0	file.txt
100644 6c493ff740f9380390d5c9ddef4af18697ac9375 0	file2.txt
```

## 1.3 修改文件1的内容

```shell
vim file.txt
git status
```

结果为：

```shell
liumeng@ubuntu64:~/workspaces/selfGit$ git status
位于分支 master

尚无提交

要提交的变更：
  （使用 "git rm --cached <文件>..." 以取消暂存）
	新文件：   file.txt
	新文件：   file2.txt

尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git restore <文件>..." 丢弃工作区的改动）
	修改：     file.txt
```

- 将修改的文件从工作区提交到暂存区

```shell
git add file.txt
```

- 查看文件索引区的内容（index）

```shell
git ls-files -s
```

输出结果为：**(显示两个内容)**

```shell
liumeng@ubuntu64:~/workspaces/selfGit$ git ls-files -s
100644 1aad415ce9ba774ba397092497f8f8cceca8367f 0	file.txt
100644 6c493ff740f9380390d5c9ddef4af18697ac9375 0	file2.txt
```

> 1aad415ce9ba774ba397092497f8f8cceca8367f  就是文件的内容，也就是blob快被更新。

- 对象区但是有三个版本（objects）

```shell
tree .git
```

结果为：(1a  6c a5)

```shell
liumeng@ubuntu64:~/workspaces/selfGit$ tree .git/
.git/
├── branches
├── config
├── description
├── HEAD
├── hooks
├── index
├── info
│   └── exclude
├── objects
│   ├── 1a
│   │   └── ad415ce9ba774ba397092497f8f8cceca8367f
│   ├── 6c
│   │   └── 493ff740f9380390d5c9ddef4af18697ac9375
│   ├── a5
│   │   └── 22c7542d9ee6732d5662e9d1d02f4cb12a23d7
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags

12 directories, 8 files
```

## 1.4 提交本地库

![image-20230917134911134](.\git_img\image-20230917134911134.png)

## 1.5 查看文件内容

- 首先查看文件的blob

```shell
git ls-files -s
```

- 其次查看文件内容;

```shell
git cat-file blob值
```

**blob值只是使用前四位也能查出来，使用全部也行**

输出结果为：

```shell
liumeng@ubuntu64:~/workspaces/selfGit$ git ls-files -s
100644 1aad415ce9ba774ba397092497f8f8cceca8367f 0	file.txt
100644 6c493ff740f9380390d5c9ddef4af18697ac9375 0	file2.txt
liumeng@ubuntu64:~/workspaces/selfGit$ git cat-file -p 1aad
hello file1.
file2
liumeng@ubuntu64:~/workspaces/selfGit$ 
```

# 第二章 git的状态

![image-20230917145421650](.\git_img\image-20230917145421650.png)

# 第三章 分支管理

## 3.1 HEAD指针

- HEAD永远指向工作人员当前所在的分支

可以通过命令查看HEAD的指向，指向的内容也说明工作人员所在的分支；

```shell
git log
```

输出结果：(HEAD -> master)。工作人员现在在master分支上；

```shell
liumeng@ubuntu64:~/workspaces/selfGit$ git log
commit 33c49d5ac62f810a730c90a5ad5636563941fef9 (HEAD -> master)
Author: lm <1611944656@qq.com>
Date:   Sun Sep 17 14:35:44 2023 +0800

    version-1
```



## 3.2 查看HEAD的指向

```shell
cat .git/HEAD
```

输出结果为：

```shell
liumeng@ubuntu64:~/workspaces/selfGit$ cat .git/HEAD 
ref: refs/heads/master
```

> 通过上面结果发现：ref: refs/heads/master
>
> HEAD指向master分支；



- 可以通过如下的命令佐证

```shell
git log
```

输出结果：重点关注 (HEAD -> master)

```shell
liumeng@ubuntu64:~/workspaces/selfGit$ git log
commit 33c49d5ac62f810a730c90a5ad5636563941fef9 (HEAD -> master)
Author: lm <1611944656@qq.com>
Date:   Sun Sep 17 14:35:44 2023 +0800

    version-1
```

## 3.3 创建一个分支

```shell
git branch dev   # 创建一个分支
git branch		# 查看当前有哪些分支
```

输出结果：

```shell
liumeng@ubuntu64:~/workspaces/selfGit$ git branch dev
liumeng@ubuntu64:~/workspaces/selfGit$ git branch
  dev
* master
liumeng@ubuntu64:~/workspaces/selfGit$ 
```

> 可以发现：现在拥有两个分支，并且(* master)带星号表示工作人员现在所在的分支；

- 通过命令再次佐证

```shell
git log
```

输出结果为：

```shell
liumeng@ubuntu64:~/workspaces/selfGit$ git log
commit 33c49d5ac62f810a730c90a5ad5636563941fef9 (HEAD -> master, dev)
Author: lm <1611944656@qq.com>
Date:   Sun Sep 17 14:35:44 2023 +0800

    version-1

```

> (HEAD -> master, dev)   表示当前工作人员所在分支是master,但是master分支和dev分支都指向一个blob; 该blob是(33c49d5ac62f810a730c90a5ad5636563941fef9)
>
> 说明指向同一个文件；

## 3.4 切换分支创建文件

```shell
git checkout dev  # 切换分支
git branch		#查看分支，并且查看当前工作人员所在指向
```

输出结果为：

```shell
liumeng@ubuntu64:~/workspaces/selfGit$ git checkout dev
切换到分支 'dev'
liumeng@ubuntu64:~/workspaces/selfGit$ git branch
* dev
  master
liumeng@ubuntu64:~/workspaces/selfGit$ 
```

> (* dev)      *号在dev分支上，工作人员所在的分支

- 在dev分支上创建一个文件

```shell
echo "dev file3">file3.txt
git add file3.txt
git commit -m "version-3"

git log
```

输出结果：

```shell
liumeng@ubuntu64:~/workspaces/selfGit$ git log
commit 797b6b9e4c9e2c4d3d523c629af11d3d6ce14a15 (HEAD -> dev)
Author: lm <1611944656@qq.com>
Date:   Sun Sep 17 15:47:57 2023 +0800

    version-3

commit 33c49d5ac62f810a730c90a5ad5636563941fef9 (master)
Author: lm <1611944656@qq.com>
Date:   Sun Sep 17 14:35:44 2023 +0800

    version-1
```

> (HEAD -> dev) 工作人员现在在dev分支上；
>
> dev在上面，说明dev已经超过master分支(version-3   在version-1的上面)

# 第四章 上传代码



- git remote add origin git@xxxxxx 此句代码直接在Github里复制
  在本地添加远程仓库地址
  origin是远程仓库的默认名字，建议不要更改
  建议不要使用https://地址，因为每次都需要密码
- git branch -M main
  将分支master重命名为main（GitHub仅支持main）
- git push -u origin main
  推送本地master分支到远程origin的main分支
  如果提示应该 git pull 一下，就 git pull 一下（git pull 是先把远程分支合并到本地对应的分支，如果远程没有更新过，才可省略 git pull）
  ==-u origin main 是设置上游分支，设置过一次就可以，下次直接上传代码直接 git push

## 4.1 点击右上角的+号，创建新仓库

![image-20230917211217671](.\git_img\image-20230917211217671.png)

## 4.2 填写仓库名称和介绍

![image-20230917211358163](.\git_img\image-20230917211358163.png)

## 4.3 增加README和licenses

![image-20230917211627062](.\git_img\image-20230917211627062.png)

## 4.4 完成创建，获取git的url地址

![image-20230917211900197](.\git_img\image-20230917211900197.png)

## 4.5 进入本地工程，使用git init创建本地仓库

