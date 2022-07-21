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

- 查看项目分叉历史。

```bash
git log --online --decorate --graph --all
```

- 分支重命名：`git branch -M branchName`。

- `git rebase(变基)`：将 `git` 的提交历史变成一条干净的直线。  
多人开发的时候，提交历史会变得很混乱，我们可以使用 `git rebase` 使提交记录变成一条干净的直线。  
`rebase` 操作可以把本地未 `push` 的分叉提交历史整理成直线。  
`rebase` 的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。
