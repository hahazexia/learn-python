# 解释器

## python 命令

安装 python 后，在终端输入 `python` 命令，就会调用 python 解释器，进入交互模式中运行。<br>

在 windows 系统上如果安装了 py.exe 启动器，则可以使用 py 命令。

![进入交互模式](../img/interactive_mode.png)

`python`命令有其他参数可以指定，在终端中输入 `python --help` 或 `python -h` 查看`python`命令帮助信息

```python
python --help
usage: python [option] ... [-c cmd | -m mod | file | -] [arg] ...

```

会显示出 python 命令的语法，和参数，还有环境变量的列表。

* `python file`

file 是 python 脚本的绝对或相对地址。`python` 命令最常用的形式就是执行脚本：

```python
python hello.py
```

* `python -c`

`python -c` 可以在命令行中执行 python 代码，举例如下：

```python
python -c "print('aaa')"
# aaa

python -c '
import sys
print(sys.argv[0])'
# -c
```

* `python -m`

-m 后面是模块，就是把模块当作脚本来运行。

```python
python -m http.server 8080
# Serving HTTP on :: port 8080 (http://[::]:8080/) ...
```

* `python -h` `python --help`

显示帮助信息

* `python -V` `python --version`

显示 python 版本

* `python`

直接进入交互模式

## 交互模式

* 如果想要退出交互模式，输入 EOF （文件结束符，UNIX 中按 Ctrl-D，Windows 中按 Ctrl-Z, Enter）时终止。

## python 代码执行过程

1. 当执行程序的时候，python 先讲代码编译成`字节码`的形式，字节码与平台无关，可以提高运行速度。如果python有写入权限，会把`字节码`保存成`.pyc`为扩展名的文件，并且存放在`__pycache__`的子目录中，这个子目录和源文件在相同路径下。如果python无法在机器上写入`字节码`，那么就在内存中生成`字节码`，程序结束时丢弃。源文件改变或者python版本的变化，都会导致重新生成`字节码`。注意，`字节码`只会针对那些被`导入的（import）`文件而生成。

2. 然后将`字节码`发送给python虚拟机`（python virtual machine，简称PVM）`来执行。`PVM` 是python的运行时引擎。 