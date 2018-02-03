[TOC]

# 初次运行Git前的配置

## 用户信息

> 第一个要配置的是你个人的用户名称和电子邮件地址。这两条配置很重要，每次 Git 提交时都会引用这两条信息，说明是谁提交了更新，所以会随更新内容一起被永久纳入历史记录：

git修改配置文件用`git comfig`命令来修改。

配置有两种：全局和局部

全局：`git comfig --global`

对应修改的配置文件在	`c:\用户\username\ .gitcomfig文件`

```
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

局部：`git comfig`

对应修改的配置文件在	`Working Directory\.git\comfig文件`

```
$ git config user.name "John Doe"
$ git config user.email johndoe@example.com
```

## 文本编辑器

> 接下来要设置的是默认使用的文本编辑器。Git 需要你输入一些额外消息的时候，会自动调用一个外部文本编辑器给你用。默认会使用操作系统指定的默认编辑器，一般可能会是 Vi 或者 Vim。如果你有其他偏好，比如 Emacs 的话，可以重新设置：

```
$ git config --global core.editor emacs
```

## 差异分析工具

> 还有一个比较常用的是，在解决合并冲突时使用哪种差异分析工具。比如要改用 vimdiff 的话：

```
$ git config --global merge.tool vimdiff
```

## 查看配置信息

- 查询全部配置信息

```
$ git config --list
```

- 查询制定配置信息

```
$ git config user.name
```

## 获得帮助

```
$ git help config
$ git config --help
```



# 取得项目的Git仓库

## 在工作目录中初始化新仓库

```
$ git init
```



## 从现有仓库克隆

> 你可能已经注意到这里使用的是 `clone` 而不是 `checkout`。这是个非常重要的差别，Git收取的是项目历史的所有数据（每一个文件的每一个版本），服务器上有的数据克隆之后本地也都有了.

```shell
$ git clone git://github.com/schacon/grit.git
```

自己定义要新建的项目目录名称，可以在上面的命令末尾指定新的名字：

```shell
$ git clone git://github.com/schacon/grit.git mygrit
```



# 记录每次更新到仓库

## 跟踪新文件、暂存已修改文件

```shell
$ git add benchmarks.rb
```

## 忽略某些文件

> 一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。我们可以创建一个名为 `.gitignore`的文件，列出要忽略的文件模式。来看一个实际的例子：

```
$ cat .gitignore
    *.[oa]
    *~
```

>第一行告诉 Git 忽略所有以 `.o` 或 `.a` 结尾的文件。一般这类对象文件和存档文件都是编译过程中出现的，我们用不着跟踪它们的版本。第二行告诉 Git忽略所有以波浪符（`~`）结尾的文件，许多文本编辑软件（比如 Emacs）都用这样的文件名保存副本。此外，你可能还需要忽略 `log`，`tmp` 或者 `pid`目录，以及自动生成的文档等等。要养成一开始就设置好 `.gitignore` 文件的习惯，以免将来误提交这类无用的文件。
>
>文件 `.gitignore` 的格式规范如下：
>
>- 所有空行或者以注释符号 `＃` 开头的行都会被 Git 忽略。
>- 可以使用标准的 glob 模式匹配。
>- 匹配模式最后跟反斜杠（`/`）说明要忽略的是目录。
>- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（`!`）取反。
>
>所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。星号（`*`）匹配零个或多个任意字符；`[abc]` 匹配任何一个列在方括号中的字符（这个例子要么匹配一个a，要么匹配一个 b，要么匹配一个 c）；问号（`?`）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 `[0-9]` 表示匹配所有0 到 9 的数字）。
>
>我们再看一个 `.gitignore` 文件的例子：

```shell
# 此为注释 – 将被 Git 忽略
    # 忽略所有 .a 结尾的文件
    *.a
    # 但 lib.a 除外
    !lib.a
    # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
    /TODO
    # 忽略 build/ 目录下的所有文件
    build/
    # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
    doc/*.txt
```





## 查看工作目录和暂存区快照的差异

```
git diff
```

## 查看已暂存文件和上次提交的快照差异

```
git diff --staged
git diff --cached
```

两条语句功能一样

## 跳过暂存，直接提交

```
$ git commit -a -m 'added new benchmarks'
```

## 移除文件

```
$ git rm grit.gemspec
$ git commit -m 'delete file'
```

这种是先从已跟踪文件清单中移除，连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。

还有一种情况是，我们指向让版本库不在跟踪文件，但该文件并未删除

```shell
$ git rm --cached readme.txt
```

还可以用glob模式（shell所用简化的正则表达式Regular Expresssion）

```
$ git rm log/\*.log
$ git rm \*~
```

## 移动和重命名

在linux文件的移动和重命名都是`mv`

对git已跟踪文件重命名

```
$ git mv README.txt README
```

实际上是执行了3条语句

```
$ mv README.TXT README
$ git rm README.txt
$ git add README
```

# 查看历史提交

## 回顾提交历史

```shell
$ git log
```

常用`-p`选项显示每次提交的内容差异，用`-2`则仅显示最近两次更新

```
$ git log -p -2
```

`git log`支持的命令选项

```
选项 说明
    -p 按补丁格式显示每个更新之间的差异。
    --stat 显示每次更新的文件修改统计信息。
    --shortstat 只显示 --stat 中最后的行数修改添加移除统计。
    --name-only 仅在提交信息后显示已修改的文件清单。
    --name-status 显示新增、修改、删除的文件清单。
    --abbrev-commit 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。
    --relative-date 使用较短的相对时间显示（比如，“2 weeks ago”）。
    --graph 显示 ASCII 图形表示的分支合并历史。
    --pretty 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式）。
```



# 撤销操作

## 修改最后一次提交

- 如果暂存区的内容和上次提交的内容一致，那么使用`git commit -amend`命令只修改提交的信息

  ```
  $ git commit -maned
  ```

- 如果暂存区的内容与上次提交的内容不一样，那么就相当于修改的上次提交

  ```
  $ git commit -m 'initial commit'
  $ git add forgotten_file
  $ git commit --amend
  ```

  上面的三条命令最终只是产生一个提交，第二个提交命令修正了第一个的提交内容。



## 取消已暂存的文件

把暂存区的内容退回到上一次暂存区的内容

```
$ git reset HEAD benchmarks.rb
```

其实使用`git status`命令有提示怎么做

## 	取消对文件的修改

把工作区中修改的文件，退回到上次提交的内容，也就是取消修改

```
$ git checkout -- benchmarks.rb
```

其实使用`git status`命令有提示怎么做

>可以看到，该文件已经恢复到修改前的版本。你可能已经意识到了，这条命令有些危险，所有对文件的修改都没有了，因为我们刚刚把之前版本的文件复制过来重写了此文件。所以在用这条命令前，请务必确定真的不再需要保留刚才的修改。如果只是想回退版本，同时保留刚才的修改以便将来继续工作，可以用下章介绍的    stashing 和分支来处理，应该会更好些。
>
>记住，任何已经提交到 Git 的都可以被恢复。即便在已经删除的分支中的提交，或者用 `--amend` 重新改写的提交，都可以被恢复（关于数据恢复的内容见第九章）。所以，你可能失去的数据，仅限于没有提交过的，对    Git 来说它们就像从未存在过一样。

# 使用远程仓库

## 克隆远程仓库

```
$ git clone git://github.com/schacon/ticgit.git
```

## 查看当前的远程库

可以使用` git remote`命令，它会列出每个远程库的简短名字。

```
$ git remote
```

也可以加上 `-v` 选项（译注：此为 `--verbose` 的简写，取首字母），显示对应的克隆地址：

```
$ git remote -v
```

## 从远程仓库抓取数据

```
$ git fetch pb
```

其中pb是远程库的名称

> 如果是克隆了一个仓库，此命令会自动将远程仓库归于 origin 名下。所以，`git fetch origin` 会抓取从你上次克隆以来别人上传到此远程仓库中的所有更新（或是上次 fetch以来别人提交的更新）。有一点很重要，需要记住，fetch 命令只是将远端的数据拉到本地仓库，并不自动合并到当前工作分支，只有当你确实准备好了，才能手工合并。
>
> 如果设置了某个分支用于跟踪某个远端仓库的分支（参见下节及第三章的内容），可以使用 `git pull`命令自动抓取数据下来，然后将远端分支自动合并到本地仓库中当前分支。在日常工作中我们经常这么用，既快且好。实际上，默认情况下 `git clone` 命令本质上就是自动创建了本地的 master分支用于跟踪远程仓库中的 master 分支（假设远程仓库确实有 master 分支）。所以一般我们运行 `git pull`，目的都是要从原始克隆的远端仓库中抓取数据后，合并到工作目录中的当前分支。

## 推送数据到远程仓库

`git push [remote-name] [branch-name]`

如果要把本地的master 分支推送到 `origin` 服务器上（再次说明下，克隆操作会自动使用默认的 master 和 origin 名字），可以运行下面的命令：

```
$ git push origin master
```

> 只有在所克隆的服务器上有写权限，或者同一时刻没有其他人在推数据，这条命令才会如期完成任务。如果在你推数据前，已经有其他人推送了若干更新，那你的推送操作就会被驳回。你必须先把他们的更新抓取到本地，合并到自己的项目中，然后才可以再次推送。

## 查看远程仓库信息

