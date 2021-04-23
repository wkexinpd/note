

### 1: 多个commit合多为一，合成一个ci记录

假设有这几个commit，如图：

![2多分支](E:\internship\note\git\image\2多分支.png)

希望可以把test01-test04合并为一个提交，可以使用`git rebase -i c896626c6fbd `，其中`-i`的意思是弹出界面可以编辑完成合并操作，

![1](E:\internship\img\1.png)

把test02，test03,test04合并提交到test01上，（s:内容保存，把提交信息往上一个commit合并进去）然后wq保存后需要继续进入界面，写入最后的commit message，编辑完成后wq保存就完成了commit的合并了

![3](E:\internship\img\3.png)

## 2: a分支上有个commmit，b分支上没有这个commit，但是我b分支想用，这时候咋办？

## 3.git merge & git rebase区别

在 Git 中整合来自不同分支的修改主要有两种方法：`merge` 以及 `rebase`。

使用merge合并：

git会使用两个分支的末端所指的快照（master修改和小修改2）以及最近的共同祖先（first commit）进行三方合并，合并的结果是生成一个新的快照，如图：

![merge](E:\internship\img\合并\merge.png)

使用rebase合并：



区别：



