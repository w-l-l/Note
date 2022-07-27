# git存储

有时候，当你在项目的一部分上已经工作一段时间后，所有东西都进入了混乱的状态，而这时你想要切换到另一个分支做一点别的事情。

问题是，你不想仅仅因为过会儿回到这一点而为做了一半的工作创建一次提交。

这个时候我们就可以使用 `git stash` 命令。

```bash
git stash
# 将未完成的修改保存到一个栈上
# 而你可以在任何时候重新应用这些改动（git stash apply）

git stash list
# 查看存储列表

git stash apply stash@{n}
# 如果不指定一个存储，git 会认为指定的是最近的存储

git stash pop
# 来应用存储然后立即从栈上扔掉它

git status drop stash@{n}
# 删除指定的存储
```
