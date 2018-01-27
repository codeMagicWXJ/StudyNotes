# git和github总结

> [廖雪峰老师的教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

## 1. 什么是git？

> Git是目前世界上最先进的分布式版本控制系统
>
> Git有什么特点？简单来说：高端大气上档次



那么现在就有两个问题

1. **什么是版本控制**

   > 版本控制（Revision control）是一种软体工程技巧，籍以在开发的过程中，确保由不同人所编辑的同一档案都得到更新。
   >
   > 版本控制透过文档控制（documentation control）记录程序各个模组的改动，并为每次改动编上序号。   

   ​

   现在出现了如下的情况：

   ​	假定你写了一篇文章，写完之后神清气爽。过两天之后来看，你觉得还有有些地方能够改进，你就对文章进行了修改。

   ​	但又过了两天，你回过头在观察是，发现还是第一次写得好。现在问题就出现了:

   ​		**如果你是在源文件上进行修改，那么你就不能回到上个版本。**

   ​	也有人说我可以复制一个副本，在副本上修改。是的这个方法可以做到，但是如果有100个版本，怎么办？

   而版本控制系统就是管理版本的。

   ​

2. **什么是布式的版本控制系统**

    说分布式就不得不说集中式,因为分布式出现在集中式之后。

   集中式:

   > 版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。中央服务器就好比是一个图书馆，你要改一本书，必须先从图书馆借出来，然后回到家自己改，改完了，再放回图书馆。


   ![ ](https://raw.githubusercontent.com/codeMagicWXJ/StudyNotes/master/pic/git/1.jpg)

   分布式：

   ​	首先，分布式没有“中央服务器”，每个人机器上都有个仓库，这样的好处是不用每次工作开始都去中央仓库下载最新版本

   ​	其次，如果不把修改上传到“中央服务器”，每个人怎么知道其他人的进度呢？只需要把自己修改的部分推送给其他人。

   	![](https://raw.githubusercontent.com/codeMagicWXJ/StudyNotes/master/pic/git/2.jpg)

3. **分布之相对于集中式有什么优势呢**

   1.效率高：不用每次开始工作都去中央仓库下载最新版本

   2.安全：集中式如果中央处理器出现问题了，那么就没法干活了。


## 2.git能做什么？

版本控制。





## 3.git工作原理







## 4.git的一些命令

1. git的一些配置指令

   ```
   $ git config --global user.name "Your Name"
   $ git config --global user.email "email@example.com"
   ```

2. 创建仓库

   仓库其实就是文件夹，只不过git在该文件夹中创建了 .git 文件夹。只要进入想要成为仓库目录，调用``git init`` 

   ```
   $ git init
   ```

   也可以直接用``git clone``命令从其他仓库克隆一份过来，自动成为仓库

   ```
   $ git clone git@github.com:michaelliao/gitskills.git
   ```

   clone后面的参数是github上仓库的ssh协议地址

   ![](https://raw.githubusercontent.com/codeMagicWXJ/StudyNotes/master/pic/git/3.jpg)

   ​

3. 添加文件至stage，提交stage中的文件

   ```
   $ git add readme.txt
   $ git commit -m "wrote a readme file"
   ```

4. 查看状态

   ```
   $ git status
   ```

5. 版本退回

   首先你要知道有多少个版本，用``git log``查看

   ```
   $ git log
   commit 3628164fb26d48395383f8f31179f24e0882e1e0
   Author: Michael Liao <askxuefeng@gmail.com>
   Date:   Tue Aug 20 15:11:49 2013 +0800

       append GPL
   ```

   其中commit后面跟着的是HSA1计算出的序列，作为版本好存在。而退回某一版本指令是``git reset --hard ``

   ```
   $ git reset --hard 3628164
   $ git reset --hard HEAD^
   $ git reset --hard HEAD100
   ```

   上面展示了3中方式：

   ​	第一种后面参数是版本号，可以不用写全，但要却别其他版本

   ​	第二种参数，HEAD只想当前分支版本，^表示上一个版本，如果是前2个则写^^

   ​	第三种参数，HEAD后面的数字表示前几个版本

   ​

6. 撤销修改

   场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令``git checkout -- file``

   ```
   $ git checkout -- readme.txt
   ```

   场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD file`，就回到了场景1，第二步按场景1操作。

   ```
   $ git reset HEAD readme.txt
   $ git checkout -- readme.txt
   ```

7. 删除文件

   删除文件用``git rm test.txt``

   ```
   $ git rm test.txt
   ```

   这句话表示删除工作去中的文件，同时把删除动作提交至缓存区。如果确认删除版本库中的文件用``git commit -m ''remove''``

   ```
   $ git commit -m "remove test.txt"
   ```

   这样版本库总的文件才伤处，如果上错了，就执行5，退回到版本库

8. 添加github远程仓库