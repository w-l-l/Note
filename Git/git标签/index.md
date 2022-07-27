# git标签

`git` 可以给历史中的某一个提交打上标签，以示重要。

比较有代表性的是人们会使用这个功能来标记发布节点（v1.0.0等等）。

```bash
git tag
git tag -l 'v1.0*'
# 列出标签
```

## 创建标签

- `轻量标签`：很像一个不会改变的分支，它只是一个特定提交的引用。

```bash
git tag v1.0.0
git tag v1.0.0 commitHash
```

- `附注标签`：存储在 `git` 数据库中的一个完整对象，它们是可以被校验的。  
其中包含打标签者的名字、电子邮件、地址、日期时间、标签信息。  
通常建议创建 `附注标签`，这样你就可以拥有以上的所有信息。

```bash
git tag -a v1.0.0
git tag -a v1.0.0 commitHash
git tag -a v1.0.0 commitHash -m '标签信息'
```

## 查看特定标签

使用 `git show` 命令可以显示任意类型的对象（`git` 对象、树对象、提交对象、`tag` 对象）。

```bash
git show tagName
```

## 远程标签

默认情况下，`git push` 命令并不会传送标签到远程仓库服务器上。

在创建完标签后你必须显式地推送标签到远程服务器上。

```bash
git push <origin> [tagName]
```

如果想一次性推送很多标签，也可以使用带有 `--tags` 选项的 `git push` 命令。

这将会把所有不在远程仓库服务器上的标签全部传送过去。

```bash
git push <origin> --tags
```

## 删除标签

```bash
git tag -d v1.0.0
# 删除本地标签

git push <origin> :refs/tags/v1.0.0
# 删除远程标签
```
