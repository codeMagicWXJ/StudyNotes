> [Reset Demystified](https://git-scm.com/blog/2011/07/11/reset.html)
>
> 这是自己理解翻译上边引用的文章



# Reset Demystified（Reset揭秘）











# The Workflow 工作流程

Let's visualize this process.  Say you go into a new directory with a single file in it. We'll call this V1 of the file and we'll indicate it in blue.

让我们想象这个过程。假设您进入一个包含一个文件的新目录。我们把它命名为V1，用蓝色表示。

Now we run `git init`, which will create a Git repository with a HEAD reference that points to an unborn branch (aka, *nothing*)

现在我们运行`git init`，它将创建一个git repository ，它的HEAD 引用指向未出生的分支(也就是，没有)



![ex2](https://raw.githubusercontent.com/codeMagicWXJ/pic/master/pic/git工作流程/git-1.png)

At this point, only the **Working Directory** tree has any content.

此时，只有工作目录树有一些内容。

Now we want to commit this file, so we use `git add` to take content in your Working Directory and populate our Index with the updated

现在我们要提交这个文件，所以我们使用``git add``来在您的工作目录中获取内容,并使用更新后的内容填充index。

content

![ex3](https://raw.githubusercontent.com/codeMagicWXJ/pic/master/pic/git工作流程/git-2.png)



Then we run `git commit` to take what the Index looks like now and save it as a permanent(永久的) snapshot pointed to by a commit, which 

然后，我们运行``git commit``，以获取该Index 现在的样子,并将commit的内容存储为永久性快照，然后将HEAD 指向该快照。

HEAD is then updated to point at.

![ex4](https://raw.githubusercontent.com/codeMagicWXJ/pic/master/pic/git工作流程/git-3.png)

At this point, all three of the trees are the same.  If we run `git status` now, we'll see no changes because they're all the

在这个时候，这三个区域都是一样的，如果我们运行`git status`,我们会看见没有变化，那是应为他们都是一样的

same.



Now we want to make a change to that file and commit it.  We will go through（经历） the same process. First we change the file in our 

现在我们修改文件并提交，我们会经历同样的过程，首先在你的工作区中修改文件

working directory.

![ex5](https://raw.githubusercontent.com/codeMagicWXJ/pic/master/pic/git工作流程/git-4.png)

If we run `git status` right now （现在）we'll see the file in red as "changed but not updated" because that entry differs between our Index 

如果我们现在运行`git status`,我们会看见红色的"changed but not updated" ，这是因为我们Working Directory和index不一样，

and our Working Directory. Next we run `git add` on it to stage it into our Index.

下一步，我们运行`git add`命令，"stage"文件加入index中

![ex6](https://raw.githubusercontent.com/codeMagicWXJ/pic/master/pic/git工作流程/git-5.png)

At this point if we run `git status` we will see the file in green under 'Changes to be Committed' because the Index and HEAD differ - that is, 

现在我们运行`git status`，我们会发现绿色文件名 file .text 在 'Changes to be Committed' 之下，这是因为Index和HEAD不一样

our proposed next commit is now different from our last commit.  Those are the entries we will see as 'to be Committed'. 

我们将要提交的内容和最后一次提交的内容不一样，这些是我们看到的一些将要提交的内容。

Finally, we run `git commit` to finalize the commit.

最后，我们运行`git commit`来完成提交。

![ex7](https://raw.githubusercontent.com/codeMagicWXJ/pic/master/pic/git工作流程/git-6.png)

Now `git status` will give us no output because all three treesare the same. Switching branches or cloning goes through a similar process.

现在运行`git status`没有输出，因为3个区域相同，交换分支或克隆和以上过程相似

When you checkout a branch, it changes **HEAD** to point to the new commit,populates your **Index** with the snapshot of that commit,

 但你检出一个分支，它会将HEAD指向新的commit，会用这commit快照填充你的index区

 then checks out the contents of the files in your **Index** into your **Working Directory**.

然后把检出**Index**中内容到 **Working Directory**



# The Role of Reset

So the `reset` command makes more sense when viewed in this context.  It directly manipulates these three trees in a simple and predictable

因此，在这种情况下，`reset`命令有很多功能，它以简单和可预测的方式之间操作这3个区域，它有3个基本操作

way.  It does up to three basic operations.

### Step 1: Moving HEAD   killing me --softly（温柔的）

The first thing `reset` will do is move what HEAD points to.  Unlike`checkout` it does not move what branch **HEAD** points to, 

`reset`首先会移动HEAD指针的指向，不像`checkout`不会移动指针的指向

it directly changes the SHA of the reference itself.  This means if HEAD is pointing to the 'master' branch, 

它会直接改变引用本身的SHA值。

running `git reset 9e5e6a4` will first of all make 'master' point to `9e5e6a4` before it does anything else.

这意味着如果HEAD指向 master分支，运行`git reset 9e5e6a4`首先会把 master指向9e5e6a4,之后在做其他事情

![reset-soft1](https://raw.githubusercontent.com/codeMagicWXJ/pic/master/pic/git工作流程/git-7.png)

No matter what form of `reset` with a commit you invoke, this is the first thing it will always try to do. 

无论你使用reset什么参数命令，这都是它首先尝试做的事情。

If you add the flag `--soft`,this is the **only** thing it will do. With `--soft`, `reset` will simply stop there.

如果你在`reset`命令后添加`--soft`,这是它唯一会做的事情。用`--soft`,`reset`只会停在那里。



Now take a second to look at that diagram and realize what it did.  It essentially undid the last commit you made. 

现在会一点实现来看图了解发生了什么，它本质上破坏了你的最后一次commit

 When you run `git commit`, Git will create a new commit and move the branch that `HEAD` points to up to it. 

当你运行`git commit`,Git会创建一个新的commit并且移动HEAD指向的分支指向它。

 When you `reset` back to `HEAD~` (the parent of HEAD), you are moving the branch back to where it was without changing the Index

当你`reset`返回到上一个`HEAD`，你将移动分支返回到上一个`HEAD`,而不改变Index(staging area)或Working Directory

(staging area) or Working Directory.  You could now do a bit more work and`commit` again to accomplish basically what 

你现在能做更多工作并再次提交，可以用`git commit amend`来实现。

`git commit --amend`would have done.



### Step 2: Updating the Index    having --mixed feelings

Note that if you run `git status` now you'll see in green the difference between the Index and what the new HEAD is.

注意，如果你运行`git status`，你会看见绿色字体的difference在Index和新的HEAD之间

The next thing `reset` will do is to update the Index with the contents of whatever tree HEAD now points to so they're the same.

