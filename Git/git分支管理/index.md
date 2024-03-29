# git分支管理

在版本控制过程中，使用多条线同时推进多个任务。

分支的好处：

- 同时并行推进多个功能开发，提高开发效率。

- 各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。

## 分支操作

- 创建分支。

```bash
git branch [分支名]
git branch [分支名] commitHash
# 新建一个分支并且使分支指向对应的提交对象
```

- 查看分支。

```bash
git branch -v
git branch --merged
# 查看哪些分支已经合并到当前分支

git branch --no-merged
# 查看所有包含未合并工作的分支
```

- 切换分支。

```bash
git checkout [分支名]
# 注意：分支切换会改变你工作目录的文件

git checkout -b [分支名]
# 创建并切换分支
```

- 删除分支。

```bash
git branch -d [分支名]
# 删除已经被合并的分支

git branch -D [分支名]
# 强制删除分支
```

- 合并分支。

```bash
git merge [分支名]
# 切换到接受修改的分支（进行合并，增加新内容）上。
#     git checkout [需要进行合并的分支名]
# 执行 merge 命令
#     git merge [有新内容的分支名]
```

- 根据 `commit-id` 查找对应分支。

```bash
git branch --merged <commit-id>
# --merged: 列出合并到 HEAD（或指定的 commit）中的分支

git branch --no-merged <commit-id>
# --no-merged: 列出尚未合并到 HEAD（或指定的 commit）中的分支

# 配合 -r 即可查看合并/未合并的「远程」分支
# 配合 -a 即可查看合并/未合并的「本地」和「远程」分支
```

- 查看项目分叉历史。

```bash
git log --online --decorate --graph --all
```

- 分支重命名：`git branch -M branchName`。

- `git rebase(变基)`：将 `git` 的提交历史变成一条干净的直线。  
多人开发的时候，提交历史会变得很混乱，我们可以使用 `git rebase` 使提交记录变成一条干净的直线。  
`rebase` 操作可以把本地未 `push` 的分叉提交历史整理成直线。  
`rebase` 的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

## 合并冲突解决

冲突的表现。

```js
/*
 <<<<<<<<<<<<<< HEAD
 xxxx  --> 当前分支内容
 ==============
 xxxx  --> 另一个分支内容
 >>>>>>>>>>>>>> merge 的分支名
*/
```

冲突的解决：

- 编辑文件，删除特殊符号（<<< HEAD === >>>）。

- 把文件修改到满意的程度，保存退出。

- `git add [文件名]`。

- `git commit -m '日志信息'`  
注意：此时的 `commit` 一定不能带具体文件名。

## 分支本质

`git` 的分支，其实本质上仅仅是指向提交对象的可变指针。

`git` 的默认分支名字是 `master`。

在多次提交操作后，你其实已经有一个指向最后那个提交对象的 `master` 分支。

它会在每次的提交操作中自动向前移动。

**注意事项：**

`git` 的 `master` 分支并不是一个特殊的分支。

它跟其他的分支没有任何区别。

之所以几乎每一个仓库都有 `master` 分支，是因为 `git init` 命令默认创建它的，并且大多数人都懒得去改动它。
