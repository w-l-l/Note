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
