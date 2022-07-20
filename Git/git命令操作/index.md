# git命令操作

## git 准备工作

一、本地库初始化：`git init`。

生成一个 `.git` 的隐藏目录，`.git` 目录中存放的是本地库相关的子目录和文件，不要删除，也不要胡乱修改。

二、设置签名。

作用：区分不同开发人员的身份。

辨析：这里设置的签名和登录远程库（代码托管中心）的账号、密码没有任何关系。

- 项目级别/仓库级别：仅在当前本地库范围内有效。  
配置信息保存位置：`./.git/config` 文件中。

```bash
git config user.name 'yourname'
git config user.email 'youremail'
```

- 系统用户级别（--global）：登录当前操作系统的用户范围（**推荐**）。  
配置信息保存位置：`~/.gitconfig` 文件中。

```bash
git config --global user.name 'yourname'
git config --global user.email 'youremail'
```

- 系统级别（--system）：当前操作系统范围。  
配置信息保存位置：`/etc/gitconfig` 文件中。

```bash
git config --system user.name 'yourname'
git config --system user.email 'youremail'
```

级别优先级：

- 就近原则：项目级别 --> 系统用户级别 --> 系统级别

检查已有的配置信息：

```bash
git config --list
```

删除配置信息：

```bash
git config [--global/--system] --unset user.name
git config [--global/--system] --unset user.email
```

## git 基本操作

- `git status`：状态查看，查看工作区、暂存区状态。

- `git add [./fileName]`：将工作区的 “新建/修改” 添加到暂存区。

- `git commit -m '描述' [fileName]`：将暂存区的内容提交到本地库。  
可以设置 `-a` 参数，被跟踪的文件可以跳过 `git add` 步骤。

## git 查看历史记录

- `git log`：显示详细提交的历史记录。  
多屏显示控制方式：  
&emsp;&emsp;空格 --> 向下翻页。  
&emsp;&emsp;b --> 向上翻页。  
&emsp;&emsp;q --> 退出。

- `git log --pretty=oneline`：每条提交历史记录一行显示（哈希值、提交信息）。

- `git log --oneline`：显示 7 位哈希和提交信息。

**以上 3 种方式只会显示当前所在版本之前的版本提交历史记录。**

- `git reflog`：查看所有分支的所有操作记录（包括已经删除的 `commit` 记录和 `reset` 的操作）。

- `git reflog --decorate`：查看当前分支所指对象（提供这一功能的参数是 `--decorate`）。

## git 前进后退版本

- 基于索引值操作（推荐）：

```bash
git reset --hard [局部索引值]
```

- 使用 `^` 符号，只能后退：

```bash
git reset --hard HEAD^
# 注意：一个 ^ 表示后退一步，n 个 ^ 表示后退 n 个版本

git reset --hard HEAD^^^
# 后退了 3 个版本
```

- 使用 `~` 符号，只能后退：

```bash
git reset --hard HEAD~n
# 注意：n 表示后退几个版本

git reset --hard HEAD~3
# 后退了 3 个版本
```

**将本地后退的分支同步到远程：git push -f。**

### reset 命令的三个参数对比

- `--mixed` 参数：

```bash
git reset --mixed xxx
# 在本地库移动 HEAD 指针
# 重置暂存区
# 当前文件内容不变，修改的内容在工作区
```

- `--soft` 参数：

```bash
git reset --soft xxx
# 仅仅在本地库移动 HEAD 指针
# 当前文件内容不变，修改的内容在暂存区
```

- `hard` 参数：

```bash
git reset --hard xxx
# 在本地库移动 HEAD 指针
# 重置暂存区和工作区
# 当前文件内容跟 HEAD 指针指向的版本内容一致
```
