## 基础命令

退出bash: `exit`, `logout`, ctrl-d

命令由单个或多个word组成, 由空格(blanks)或TABS分隔, 第一个称为'command', 后续为'arguments'

'arguments'中有一些特殊类型

'option': 通常由一个'-'和一个'letter'组成, 比如命令`lp -h myfile`中的'-h', 代表not to print the "banner page" before it prints the file

*`lp`是'line printer '的意思, 用于print文件*

'option'有时会带有一个'argument', `lp -d lp1 -h myfile`中'-d'带有参数'lpl', "Send the output to the printer (destination) called lp1"

遇事不决, 不知道某个命令怎么用, 使用'--help'选项, 通常是个命令都有这个选项

## 文件

三种: Regular files, Executable files, Directories

常见概念: The working directory, 当前环境/所在文件夹, 命令输入都会处在一个文件夹下, 可以使用`pwd`获知

路径类型: 相对路径, 绝对路径

路径分隔符: '/', 当前目录: '.', 上层目录: '..', 根目录: '/'

相对路径: 当前目录内文件获取: './file', 上层文件获取, '../file'

绝对路径: 获取文件'/home/username/file'

当登入系统时, The working directory初始化为一个特殊的目录, 称作: 'home'或'login'目录

例如: 使用用户'aqua'登入系统, 会处在'/home/aqua'下

由于不同系统放置'home'的位置不同(虽然通常都在'/home'或'/users'下), 不好记, 有一个简单的缩写, '~'后带上用户名, 如: '~aqua', 这是一个绝对路径. 甚至更简略的, 单个'~'就代表当前用户的文件夹, 即若登入用户为'aqua', 其用户目录可以是'~aqua'或'~'

跳转目录的常用命令: 'cd', 除了参数带一个路径正常地跳来跳去, `cd -`可以跳到上一次的目录

常用通配符

?: 单个任意字符(不能为零)

*: 零个或多个任意字符

\[set]: 一些字符中的任意一个

\[! set]: 不在这个set中的任意一个

-: 表示范围, 比如[a-c], 表示a, b, c任意一个, [a-zA-Z0-9_-]表示任意大小写字母, 任意数字, 下划线(underscore)或'-'(dash)

!: 前置, 表否定. 比如[!0-9]表示非数字, 而[0-9!]表示任意数字或'!'

\: 转义, [\\!]就会匹配到'!'

查看当前目录内容命令: `ls`, 加上一个参数(目录名)代表查看某个目录, 配合通配符使用比如: `ls b*`表示当前目录下所有以'b'开头的目录里的所有内容

大括号语法, 比如`echo b{ed,olt,ar}s`会输出'beds bolts bars'. 注意, ','左右不能有空格

依据惯例, 一个unix程序有一个接收输入的途径称为'standard input', 一个输出的途径'standard output', 一个输出错误的途径standard error output', 通常三者对应文件描述符: '0, 1, 2'

一些常用的输入输出命令

cat: **Concatenate** files to standard output

grep: Search for strings in the input

sort: Sort lines in the input

cut: Extract columns from input

sed: Perform editing operations on input

tr: Translate characters in the input to other characters

I/O重定向

command < filename会把输出输出到standard output

command > filename会把输出输出到一个file内, 比如`echo hello world > hello`会把'hello world'输出到在当前文件夹内的hello文件内

管道

'|', 会把输出从左到右一路传输

后台运行

在命令后