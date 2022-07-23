# git远程操作

## 创建远程库地址别名

```bash
git remote add [别名] [远程地址]

git remote -v
# 查看当前所有远程地址别名
```

## 推送（push）

```bash
git push [别名] [分支名]

git push -u [别名] [分支名]
# -u：用 git push 代替 git push [别名] [分支名]
```

## 克隆（clone）

```bash
git clone [远程地址]
# 默认克隆 master 分支上的内容

git clone -b [远程分支名] [远程地址]
# 克隆远程指定分支上的内容
```

效果：

- 完整把远程库下载到本地。

- 创建 `origin` 远程地址别名。

- 初始化本地库。

## 拉取（pull）

```bash
# pull = fetch + merge

git fetch [远程库地址别名] [远程分支名]
git merge [远程地址别名/远程分支名]

git pull [远程地址别名] [远程分支名]
```

## 解决冲突

如果不是基于 `git` 远程库的最新版所做的修改，不能推送，必须先拉取。

拉取下来后如果进入冲突状态，则按照“分支冲突解决”操作解决即可。

## SSH 免密登录

运行命令生成 `.ssh` 密钥目录。

```bash
ssh-keygen -t rsa -C 'youremail'
```

进入 `.ssh` 目录查看文件列表，复制 `id_rsa.pub` 文件内容。

登录 `GitHub`，点击用户头像 --> Settings --> SSH and GPG keys，点击 New SSH key，输入复制的密钥信息。

回到 `git bash` 创建远程地址别名。

```bash
git remote add [别名] git@github.com:xxx.git
# 注意：这里的远程地址必须使用 git 开头的地址才能实现免密登录，不能使用 https 开头的地址
```

## 远程跟踪分支

克隆仓库，会自动为 master 做分支。

- 本地没有分支。

```bash
git checkout --track origin/远程分支名
# 注意：远程刚创建分支，可能没有和本地同步，所以还需要执行下面这条命令

git remote update origin --prune
# 更新本地和远程分支同步
```

- 本地已经创建了分支。

```bash
git branch -u origin/远程分支名
# 注意：远程分支名必须存在，没有的话可以使用 git push origin [localBranch]:[remoteBranch] 命令创建远程分支

git push --set-upstream origin localBranch
# 创建远程分支，并建立本地和远程的跟踪，push 代码
# 注意：localBranch 必须存在
```

- 查看设置的所有跟踪分支。

```bash
git branch -vv
```

- 查看本地和远程的分支。

```bash
git branch -a

git branch -r
# 仅查看远程分支
```

- 删除远程分支。

```bash
git push origin --delete [branchName]

git remote prune <origin> --dry-run
# 列出仍在远程跟踪但是远程已被删除的无用分支

git remote prune <origin>
# 清楚上面命令列出来的远程跟踪
```

- 本地新建分支推送远程并建立分支跟踪。

```bash
git push -u origin localBranch
```
