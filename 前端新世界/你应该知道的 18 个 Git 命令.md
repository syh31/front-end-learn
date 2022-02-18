# 你应该知道的 18 个 Git 命令

## 简介

毫无疑问，Git是用于跟踪源代码更改的最流行的版本控制系统。

git的创始人是Linus Torvalds，同时他也是Linux的创始人。

## 前提要求

为了能够进行后续操作，你需要一个git终端或安装了git的bash shell。

## Git配置

在初始化新的git仓库或开始提交之前，应先配置git身份。

为了更改与提交关联的name，你可以使用`git config`命令：

```bash
git config --global user.name "Your Name"
```

同样，这也适用于更改与提交关联的电子邮件地址：

```bash
git config --global user.email "yourmail@example.com"
```

这样，在配置了以上内容之后，在你进行提交时以及查看git日志后，你将能够看到此提交与上面的配置信息相关联。

```bash
git log
```

我的电脑中，输出如下所示：

```bash
commit 45f96b8c2ef143011f11b5f6cc7a3ae20db5349d (HEAD -> main, origin/master, origin/HEAD)
Author: Bobby Iliev <bobby@bobbyiliev.com>
Date:   Fri Jun 19 17:03:53 2020 +0300

    Nginx server name for www version (#26)
```

## 初始化项目

要初始化新的本地git项目，请打开git或bash终端，通过`cd`命令转到你想要存储项目的目录，然后运行：

```bash
git init .
```

如果你已经在GitHub中有一个现有项目，则可以使用`git clone`命令对其进行克隆：

```bash
git clone your_project_url
```

## 当前状态

为了检查本地git仓库的当前状态，你需要使用以下命令：

```bash
git status
```

这可能是最常用的命令之一，因为你需要经常检查本地代码仓库的状态，以便能知道更改、暂存或删除了哪些文件。

## 将文件添加到暂存区

假设你有一个静态的HTML项目，并且已经初始化了git仓库。

现在，你决定添加一个叫做`about-me.html`的新HTML文件，当然你已经添加好了一些HTML代码。为了添加新文件，以便也可在git中对其进行跟踪，你首先需要使用`git add`命令：

```bash
git add file_name
```

这将暂存新文件，实际上意味着下次你进行提交时，所做的更改将成为提交的一部分。

为了确认这一点，你可以再次运行`git status`命令：

```bash
git status
```

你将看到以下输出：

```bash
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   about-me.html
```

## 删除文件

为了从git项目中删除文件，可以使用以下命令：

```bash
git rm some_file.txt
```

然后，如果再次运行`git status`，将会看到`some_file.txt`文件已被删除：

```bash
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        deleted:    some_file.txt
```

## 放弃文件更改

万一你犯了一个错误，并且想要放弃对特定文件的更改，并将该文件的内容重置为最近一次提交时的内容，则需要使用以下命令：

```bash
git checkout -- file_name
```

这是一个非常方便的命令，可以快速将文件恢复为初始内容。

## 提交到本地

一旦你完成更改并使用`git add`命令添加到暂存区，你还需要提交更改。

为此，你必须使用`git commit`命令：

```bash
git commit
```

这将打开一个文本编辑器，你可以在此文本编辑器中输入提交说明。

并且，你可以使用`-m`标志直接在命令中指定提交说明：

```bash
git commit -m "Nice commit message here"
```

## 列出分支

为了列出所有可用的本地分支，只需运行以下命令：

```bash
git branch -a
```

你将获得本地和远程分支的列表，其输出如下所示：

```bash
  bugfix/nginx-www-server-name
  develop
* main
  remotes/origin/HEAD -> origin/master
  remotes/origin/bugfix/nginx-www-server-name
  remotes/origin/develop
  remotes/origin/main
```

`remotes`关键字表示这些分支是远程分支。

## 从远程获取更新并将当前分支与上游（upstream）合并

如果你和团队中的其他开发人员一起工作于同一项目，则经常需要获取这些同事所做的更改，以便于本地存储到你的PC上。

为此，你需要做的就是使用`git pull`命令：

```bash
git pull origin branch_name
```

请注意，这也会把新更改合并到你签入到的当前分支中。

## 创建一个新分支

要创建一个新分支，你需要做的就是使用`git branch`命令：

```bash
git branch branch_name
```

如果不用上面的方法，我更喜欢使用以下命令，因为这会创建一个新分支，并将你切换到新创建的分支：

```bash
git checkout -b branch_name
```

如果`branch_name`已经存在，你将收到一条警告，表明该分支名称已存在，并且你将不会被签出到此分支。

## 将本地更新推送到远程

最后，一旦完成所有更改，就可以使用`git add`命令将文件添加到暂存区（staged），然后使用`git commit`命令提交更改，并且你必须将这些更改推送到远程git仓库。

为此，只需使用`git push`命令：

```bash
git push origin branch_name
```

## 删除分支

```bash
git branch -d branch_name
```

## 切换到新分支

```bash
git checkout branch_name
```

如上所述，如果此分支不存在，添加`-b`标志将创建分支。

## 结论

了解以上命令将使你能够像专业人士一样管理自己的项目！

希望这篇文章能对大家有所帮助！