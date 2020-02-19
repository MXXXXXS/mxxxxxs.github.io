## 包管理

pip, 类似npm

包的托管平台: pypi( pronounced *Pie Pea Eye* )

python标准库的包: system packages

python第三方的包: site packages

## 虚拟环境

### virtualenv(适用python2)

*pip安装默认都是**全局**的包(npm这点就很好), 这个问题很大!*

隔离式开发, 使用工具: virtualenv. 每个项目独立python的版本和各个包的版本

#### 使用pip安装

```
pip install virtualenv
```

#### 在一个目录里创建环境

```bash
virtualenv envfolder
cd envfolder
```

#### 激活环境

*注意: 在windows环境没有bin目录, 而是Scripts目录*

windows(git bash):

```
source Scripts/activate
```

#### 检验是否激活

```
which python
```

会显示python为当前目录下的Scripts/python

使用解释器'Scripts/python.exe'

#### 怎么工作的

[参考](https://www.dabapps.com/blog/introduction-to-pip-and-virtualenv-python/)

##### 什么是一个虚拟环境

An environment is simply a directory that contains a complete copy of everything needed to run a Python program, including a copy of the `python` binary itself, a copy of the entire Python standard library, a copy of the `pip` installer, and (crucially) a copy of the `site-packages` directory mentioned above. 

##### 怎么装包的

When you install a package from PyPI using the *copy* of `pip` that's created by the `virtualenv` tool, it will install the package into the `site-packages` directory *inside* the virtualenv directory. You can then use it in your program just as before. 

##### active的作用

Instead of typing `env/bin/python` and `env/bin/pip` every time, we can run a script to *activate* the environment. 

This script, which can be executed with `source env/bin/activate`, simply adjusts a few variables in your shell (temporarily) so that when you type `python`, you actually get the Python binary *inside* the virtualenv instead of the global one 

### venv(适用python3, 自从python3.3起包含在标准库中)

## virtualenv多版本python共存

[参考]( https://www.freecodecamp.org/news/installing-multiple-python-versions-on-windows-using-virtualenv/ )

1. Open `Command Prompt` and enter `pip install virtualenv`
2. Download the desired `python` version (do NOT add to PATH!), and remember the `path\to\new_python.exe` of the newly installed version
3. To create a virtualenv, open `Command Prompt` and enter
   `virtualenv \path\to\env -p path\to\new_python.exe`
4. If you are using `PyCharm`, update the `Project Interpreter` and the `Code compatibility inspection`.
5. To install packages:
   (I) Activate virtualenv: open `Command Prompt` and enter `path\to\env\Scripts\activate.bat`
   (II)  Install desired packages
   (III)  Deactivate with `deactivate` .

## 换源

包管理工具在国内日常需要换源, 不然速度极慢

源管理工具: pqi(类似nrm)

