

## 1: 多个commit合多为一，合成一个ci记录

假设有这几个commit，如图：

![2多分支](E:\internship\note\git\image\2多分支.png)

希望可以把test01-test04合并为一个提交，可以使用`git rebase -i c896626c6fbd `，其中`-i`的意思是弹出界面可以编辑完成合并操作，

![1](E:\internship\img\1.png)

把test02，test03,test04合并提交到test01上，（s:内容保存，把提交信息往上一个commit合并进去）然后wq保存后需要继续进入界面，写入最后的commit message，编辑完成后wq保存就完成了commit的合并了

![3](E:\internship\img\3.png)

## 2: a分支上有个commmit，b分支上没有这个commit，但是我b分支想用，这时候咋办？

### git cherry-pick：

`git cherry-pick`命令的作用就是将指定的提交（commit）应用于其他分支。

```
$ git cherry-pick <commitHash>
```

上面命令就会将指定的提交`commitHash`，应用于当前分支。

假设master有a,b两个commit，如果dev分支想用master分支中的b分支，可以使用`git cherry-pick 314d01c98d7312789c `将b commit合并到dev分支上

![dev分支上有了bcommit](E:\internship\img\合并\dev分支上有了bcommit.png)

Cherry pick 支持一次转移多个提交。

```
$ git cherry-pick <HashA> <HashB>
```

上面的命令将 A 和 B 两个提交应用到当前分支。这会在当前分支生成两个对应的新提交。

+ 常用配置项：

| 简写 | 配置项      | 描述                                                         |
| ---- | ----------- | ------------------------------------------------------------ |
| -e   | --edit      | 打开外部编辑器，编辑提交的信息                               |
| -n   | --no-commit | 只更新工作区和暂存区，不产生新的提交                         |
| -x   |             | 在提交信息的末尾追加一行，方便一行查到这个提交时如何产生的   |
| -s   | --signoff   | 在提交信息的末尾追加一行操作者的签名，表示是谁进行了这个操作 |

## 3.git merge & git rebase区别

在 Git 中整合来自不同分支的修改主要有两种方法：`merge` 以及 `rebase`。

使用merge合并：

git会使用两个分支的末端所指的快照（master修改和小修改2）以及最近的共同祖先（first commit）进行三方合并，合并的结果是生成一个新的快照，如图：

![merge](E:\internship\img\合并\merge.png)

使用rebase合并：

**rebase**会把你当前分支的 commit 放到公共分支的最后面，不会产生新的commit，如图：

检出`dev`分支，将它变基到`master`分支上.

![区别](E:\internship\img\合并\区别.png)

但是如果提交存在于自己的仓库之外，而别人可能基于这些提交进行开发，那么不要执行变基。

所以我认为：

rebase更适合在自己的开发分支上一直做，如果想要把主线的修改合并到自己的分支上做一次的集成

merge更适合在远程仓库上，不同的人基于同一个提交进行开发

