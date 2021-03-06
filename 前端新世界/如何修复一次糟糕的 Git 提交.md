## 如何修复一次糟糕的 Git 提交

哦no！这次提交没弄好！

在使用Git的时候，我们大家都碰到过不顺心的时候，可能是忘记添加文件或者是忘记写注释了，也有可能是合并未按预期进行了。幸运的是，Git有一些命令可以帮助处理这些常见的情况，一起来了解一下吧。

## 修改提交信息

糟糕——在提交消息中你发现了拼写错误。不用担心，这个我们是可以修改的：

```bash
git commit --amend -m "new message"
```

## 添加文件到最后一次提交

更改已经提交，但又忘记添加文件了。没问题，我们仍然可以将文件添加到这次提交中：

```bash
git add <file_name>
git commit --amend HEAD~1
```

## 撤消提交

如果要撤消最近一次提交但保留更改，可执行以下操作：

```bash
git reset --soft HEAD~1
```

如果要撤消提交和更改，可执行以下操作：注意，确定是要丢弃更改。

```bash
git reset --hard HEAD~1
```

还有一种情况是，如果要撤消所有的本地更改，则可以重置为分支的原始版本：

```bash
git reset --hard origin/<branch_name>
```

如果要撤消提交而不修改现有历史记录，则可以使用`git revert`，此命令通过创建新的提交来撤消提交。

```bash
git revert HEAD
```

如果你刚解决了冲突，完成了合并，并且推送到了原始版本。在这个节骨眼上出了点问题……

撤消已经推送到远程分支的合并提交的安全方法是使用`git revert`命令：

```bash
git revert -m 1 <commit_id>
```

`commit_id`是要还原的合并提交id。

**注意要点：**

1.可以撤消任意数量的提交。例如：

- `git reset HEAD~3`（返回HEAD之前的3个提交）。
- git reset --hard <commit_id>（返回特定的提交）。

2.如果尚未推送提交，并且你不想引入糟糕的提交到远程分支，可以使用`git reset`。

3.使用`git revert`还原已经推送到远程分支的合并提交。

4.使用`git log`查看提交历史。

希望这些关于git修复提交的小技巧能对大家有用。谢谢阅读！