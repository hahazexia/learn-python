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
