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
