# git底层概念

## 基础的 linux 命令

`git` 窗口实际输入的就是 `linux` 命令。

- `clear`：清除命令窗口的信息。

- `echo 'xxx'`：往控制台输出信息。

- `echo 'xxx' > fileName`：输出内容到文件并创建。

- `ll`：将当前文件下的子文件和子目录平铺在控制台。

- `find directoryName`：将对应目录下的子孙文件和子孙目录平铺在控制台。

- `find directoryName -type f`：将对应目录下的文件平铺在控制台。

- `rm fileName`：删除文件。

- `mv 源文件 重命名文件`：重命名。

- `cat 文件的url`：查看对应文件的内容。

- `vim 文件的url（在英文环境下）`：  
按 `i` 进入插入模式，进行文件的编辑。  
按 `esc` 键&按 `:` 键，进行命令的执行：  
&emsp;&emsp;`q!` --> 强制退出（不保存）。  
&emsp;&emsp;`wq` --> 保存退出。  
&emsp;&emsp;`set nu` --> 设置行号。
