# Linux使用技巧与注意事项
> Collected by liuyujie0136

## Docker
> Docker is a tool to run Linux containers based on selected images.

### [Docker从入门到实践](https://yeasy.gitbook.io/docker_practice/)

### Win10家庭版用WSL2运行Docker Desktop，将数据从C盘迁移到其他目录

Win10开始使用Hyper-V运行docker，家庭版无此功能，所以只能pro版本才能用docker desktop。后来升级到2004系统后，家庭版可升级WSL(Windows Subsystem for Linux)至WSL2，便可基于WSL2运行docker desktop，但相较于Hyper-V的区别在于，其数据由WSL2代为管理，无法在docker里设置镜像存储路径。

安装docker后，docker会自动创建2个发行版：`docker-desktop`和`docker-desktop-data`，其默认安装在C盘，在`%LOCALAPPDATA%/Docker/wsl`目录里，而docker的运行数据、镜像文件都存在`%LOCALAPPDATA%/Docker/wsl/data/ext4.vhdx`中，比较占用C盘空间，故考虑将其迁移至D盘。

对于WSL发行版的迁移，网上教程基本均需使用LxRunOffline，但其虽然可以迁移例如Ubuntu的发行版，但迁移不了docker自动创建的2个发行版。故考虑如下两种方式：

**方法一：使用wsl命令**

* 关闭docker desktop
* 关闭所有发行版
```
wsl --shutdown
```
* 导出docker-desktop-data
```
wsl --export docker-desktop-data D:\docker-desktop-data.tar
```
* 注销docker-desktop-data
```
wsl --unregister docker-desktop-data
```
* 重新导入docker-desktop-data
```
wsl --import docker-desktop-data D:\Tools\WSL\docker-desktop-data\ D:\docker-desktop-data.tar --version 2
```
* _注：只迁移docker-desktop-data即可，另一个占用空间较小_

**方法二：修改注册表（未直接验证，理论上及其他操作证明应当可行）**

* 关闭docker desktop
* 打开`powershell`，输入`whoami /user`，获得[SID]；用`wsl --shutdown`命令关闭所有发行版
* 运行`regedit`，跳转至`HKEY_USERS\[SID]\SOFTWARE\Microsoft\Windows\CurrentVersion\Lxss`
* 在下属的文件文件夹中（文件夹名为一个UUID）找到`DistributionName`为`docker-desktop-data`的项
* 将其`BasePath`改为目标目录，如`\\?\D:\Tools\WSL\docker-desktop-data`，并将原目录下`ext4.vhdx`拷贝至该目录
* 重启即可使用

### Docker创建共享文件夹

* 检查docker是否安装成功
```
docker info
```
* 装载镜像
```
docker load -i ~/Downloads/bioinfo_PartI-PartII-PartIII1-3.tar.gz
```
* 创建共享文件夹
```
mkdir ~/Documents/bioinfo_tsinghua_share
```
* 创建容器（**win10下共享文件夹需用绝对路径**）
```
docker run --name=bioinfo_tsinghua -dt -h bioinfo_docker --restart unless-stopped -v C:/Users/[username]/Documents/bioinfo_tsinghua_share:/home/test/share bioinfo_tsinghua
```
* 将docker中的`/home/test/share`由`root`所有改为`test`所有
```
docker exec -u root bioinfo_tsinghua chown test:test /home/test/share
```
* 以`test`用户运行docker
```
docker exec -it bioinfo_tsinghua bash
```
* _注：以`root`用户运行docker_
```
docker exec -it -u root bioinfo_tsinghua bash
```

## [鳥哥的Linux私房菜](http://linux.vbird.org/linux_basic/)

## [Linux常用命令](https://mp.weixin.qq.com/s/cri9CEbVJizZIO9WwiCJ0g)

## [Linux特殊符号](https://mp.weixin.qq.com/s/IO8Ckahig14RIvyDPX5lhw)

## [三十分钟学会AWK](https://github.com/mylxsw/growing-up/blob/master/doc/%E4%B8%89%E5%8D%81%E5%88%86%E9%92%9F%E5%AD%A6%E4%BC%9AAWK.md)

## [三十分钟学会SED](https://github.com/mylxsw/growing-up/blob/master/doc/%E4%B8%89%E5%8D%81%E5%88%86%E9%92%9F%E5%AD%A6%E4%BC%9ASED.md)

## [Learn Vim Progressively](http://yannesposito.com/Scratch/en/blog/Learn-Vim-Progressively/)

## Linux查看系统基本信息

* 查看版本当前操作系统内核信息
```bash
uname -a
```
* 查看当前操作系统版本信息
```bash
cat /proc/version
```
* 查看版本当前操作系统发行版信息
```bash
cat /etc/issue
```
* 查看cpu相关信息，包括型号、主频、内核信息等
```bash
cat /proc/cpuinfo
```
* 查看服务器名称
```bash
hostname
```
* 查看网络信息
```bash
cat /etc/sysconfig/network-scripts/ifcfg-eth0
cat /etc/sysconfig/network-scripts/ifcfg-l0
```
* 查看磁盘信息
```bash
lsblk		#查看磁盘信息 - 列出所有可用块设备的信息，而且还能显示他们之间的依赖关系，但是它不会列出RAM盘的信息
fdisk -l	#观察硬盘实体使用情况，也可对硬盘分区
df -k		#用于显示磁盘分区上的可使用的磁盘空间
```
* 查看进程与用户信息
```bash
ps -ef		#查看所有进程
top		#实时显示进程状态
w		#查看活动用户
id <username>	#查看指定用户信息
```
* [更多内容](https://blog.csdn.net/qq_31278903/article/details/83146031)

## Bash和Sh的区别
> Bash is the most commonly used linux shell.

* sh(或Shell命令语言)是由POSIX标准描述的一种编程语言。它有很多实现(ksh88, dash，…)。bash也可以被认为是sh的实现。因为sh是规范，不是实现，`/bin/sh`是在大部分POSIX系统上实际实现的符号连接（or 硬链接）。
* bash是兼容sh的一种实现（虽然在几年之前被视为POSIX标准），但随着时间流逝，它需要更多的扩展。这里面的一些扩展会改变有效的POSIX shell脚本的行为，所以bash本身不是有效的POSIX shell，而是POSIX shell语言的方言。但bash可以执行`--posix`切换，使得它更加的兼容POSIX，同时也尝试通过调用sh来模仿POSIX。
* 长期以来，在大部分GNU/Linux系统上，`/bin/sh`都是指向`/bin/bash`。结果，几乎可以忽略两者之间的区别了。但是这种情况最近开始改变。
* 在`/bin/sh`不指向`/bin/bash`（在某些情况下甚至都不存在`/bin/bash`）的系统中，一些常见的例子是：
  * 现代的debian和ubuntu系统上，`sh`默认是`dash`的符号链接
  * Busybox，它通常在Linux系统引导时作为initramfs的一部分运行。它使用了ash shell实现。
  * BSDs，以及通常所有非linux系统。OpenBSD 使用pdksh，Korn shell的后代。FreeBSD的sh是原始UNIX Bourne shell的后代。Solaris有它自己的sh，但长期以来都不是与POSIX兼容的，是一种Heirloom项目提供的一个开源实现。
* 如何找到`/bin/sh`在我们系统上的指向
  * `/bin/sh`的复杂之处是：它可以是符号链接也可以是硬链接。
  * 如果是符号链接，可以尝试：
```bash
test@bioinfo_docker:~$ file -h /bin/sh
/bin/sh: symbolic link to dash
```
  * 如果是硬链接，可以尝试：
```bash
test@bioinfo_docker:~$ find -L /bin -samefile /bin/sh
/bin/sh.distrib
/bin/dash
/bin/sh
```
  * 注：实际上`-L`标志同时包括符号链接和硬链接，但是这种方法的缺点是它不是可移植的，POSIX不需要`find`来支持`-samefile`选项，尽管GNU `find`和FreeBSD `find`都支持它。
* Shebang：在计算领域中，Shebang（也称为Hashbang）是一个由井号和叹号构成的字符序列#!，其出现在文本文件的第一行的前两个字符。最终，通过在脚本的第一行编写Shebang来决定使用sh还是bash。
```bash
# 1. 使用sh
#!/bin/sh
# 2. 使用bash（如果不可用会失败并带上错误信息）
#!/bin/bash
# 3. 使用dash
#!/bin/dash
```

## 利用.bashrc个性化配制bash环境

**示例一**

```vim
# .bashrc
# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi

# User specific environment and startup programs
if [ -f $HOME/shortcuts ]; then
        source $HOME/shortcuts
fi
PATH=$HOME/bin:$PATH
export PATH

# User specific aliases and functions
alias qstat="qstat -u '*'"
#alias screen="/usr/bin/screen -D -R"
#alias rm="$HOME/bin/del.sh"
#alias undel="$HOME/bin/del.sh -u"
#alias ls="ls --color"
alias ld="ls -d"
alias c="clear"
alias l="ls -alh"
alias lf="ls -F|grep /"
alias lt="ls -tlr"
alias mv="mv -i"
alias cp="cp -pi"
alias diff="diff -b"

#PERL5LIB=$MYHOME/perllib:$MYHOME/perllib/lib64/perl5/site_perl/5.8.5:$MYHOME/perlib/lib/perl5/site_perl/5.8.5
#export PERL5LIB
#export R_LIBS_USER=~/R:/data/apps/R_library
```

**示例二**

```vim
# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples

# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return;;
esac

# don't put duplicate lines or lines starting with space in the history.
# See bash(1) for more options
HISTCONTROL=ignoreboth

# append to the history file, don't overwrite it
shopt -s histappend

# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTSIZE=1000
HISTFILESIZE=2000

# check the window size after each command and, if necessary,
# update the values of LINES and COLUMNS.
shopt -s checkwinsize

# If set, the pattern "**" used in a pathname expansion context will
# match all files and zero or more directories and subdirectories.
#shopt -s globstar

# make less more friendly for non-text input files, see lesspipe(1)
[ -x /usr/bin/lesspipe ] && eval "$(SHELL=/bin/sh lesspipe)"

# set variable identifying the chroot you work in (used in the prompt below)
if [ -z "${debian_chroot:-}" ] && [ -r /etc/debian_chroot ]; then
    debian_chroot=$(cat /etc/debian_chroot)
fi

# set a fancy prompt (non-color, unless we know we "want" color)
case "$TERM" in
    xterm-color|*-256color) color_prompt=yes;;
esac

# uncomment for a colored prompt, if the terminal has the capability; turned
# off by default to not distract the user: the focus in a terminal window
# should be on the output of commands, not on the prompt
#force_color_prompt=yes

if [ -n "$force_color_prompt" ]; then
    if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
	# We have color support; assume it's compliant with Ecma-48
	# (ISO/IEC-6429). (Lack of such support is extremely rare, and such
	# a case would tend to support setf rather than setaf.)
	color_prompt=yes
    else
	color_prompt=
    fi
fi

if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
unset color_prompt force_color_prompt

# If this is an xterm set the title to user@host:dir
case "$TERM" in
xterm*|rxvt*)
    PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
    ;;
*)
    ;;
esac

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# colored GCC warnings and errors
#export GCC_COLORS='error=01;31:warning=01;35:note=01;36:caret=01;32:locus=01:quote=01'

# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# Add an "alert" alias for long running commands.  Use like so:
#   sleep 10; alert
alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'

# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi

# personal alias
alias c="clear"
alias ca="cat -A"
```

## 利用.vimrc个性化配置vim

* 复制一份vim配置模板到个人目录下
```bash
cp /usr/share/vim/vimrc ~/.vimrc
```
* 利用`vim`编辑该文件
```vim
"" 系统自带文件关键内容如下
"
" Vim5 and later versions support syntax highlighting. Uncommenting the next
" line enables syntax highlighting by default.
if has("syntax")
  syntax on
endif
"
" If using a dark background within the editing area and syntax highlighting
" turn on this option as well
set background=dark
"
" Uncomment the following to have Vim jump to the last position when
" reopening a file
if has("autocmd")
  au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif
endif
"
" Uncomment the following to have Vim load indentation rules and plugins
" according to the detected filetype.
"if has("autocmd")
"  filetype plugin indent on
"endif
"
" The following are commented out as they cause vim to behave a lot
" differently from regular Vi. They are highly recommended though.
"set showcmd		" Show (partial) command in status line.
"set showmatch		" Show matching brackets.
"set ignorecase		" Do case insensitive matching
"set smartcase		" Do smart case matching
"set incsearch		" Incremental search
"set autowrite		" Automatically save before commands like :next and :make
"set hidden		" Hide buffers when they are abandoned
"set mouse=a		" Enable mouse usage (all modes)
"
" Source a global configuration file if available
if filereadable("/etc/vim/vimrc.local")
  source /etc/vim/vimrc.local
endif
"
"" 实用设置
"
set nocompatible	"去掉有关vi一致性模式，避免以前版本的bug和局限
set nu!			"显示行号
set guifont=Luxi/ Mono/ 9	"设置字体，字体名称和字号
filetype on		"检测文件的类型
set history=1000	"记录历史的行数
set background=dark	"背景使用黑色
syntax on		"语法高亮度显示
set autoindent		"vim使用自动对齐，也就是把当前行的对齐格式应用到下一行(自动缩进）
set cindent		"cindent是特别针对C语言语法自动缩进
set smartindent		"依据上面的对齐格式，智能的选择对齐方式，对于类似C语言编写上有用
set tabstop=4		"设置tab键为4个空格
set shiftwidth=4	"设置当行之间交错时使用4个空格
set ai!			"设置自动缩进
set showmatch		"设置匹配模式，类似当输入一个左括号时会匹配相应的右括号
set guioptions-=T	"去除vim的GUI版本中的toolbar
set vb t_vb=		"当vim进行编辑时，如果命令错误，会发出警报，该设置去掉警报
set ruler		"在编辑过程中，在右下角显示光标位置的状态行
set nohls		"默认情况下，寻找匹配是高亮度显示，该设置关闭高亮显示
set incsearch		"在程序中查询一单词，自动匹配单词的位置；如查询desk单词，当输到/d时，会自动找到第一个d开头的单词，当输入到/de时，会自动找到第一个以ds开头的单词，以此类推，进行查找；当找到要匹配的单词时，别忘记回车
set backspace=2		"设置退格键可用
```
* 保存退出
* [更多内容](https://blog.csdn.net/amoscykl/article/details/80616688)

## Bash下使用rsync和crontab备份文件

* Install `rsync` and `crontab`
```bash
# in Ubuntu
sudo apt -y install rsync
sudo apt -y install crontabs
# in CentOS
yum -y install rsync
yum -y install crontabs
```
* Prepare backup directory
```bash
mkdir /mac/backup
```
* Prepare a backup script, for example, `~/backup.sh`
```bash
#!/bin/bash
#backup.sh
#1. Local backup  
RSYNC="rsync --stats  --compress --recursive --times --perms --links --delete --max-size=100M --exclude-from=/home/john/.rsync/exclude"
echo "1. Backup of /home/john start at:"
date
$RSYNC /home/john/data/  /mac/backup/
echo "Backup end at:"
date
#2. Remote backup 
RSYNC="rsync --stats  --compress --recursive --times --perms --links --delete --max-size=100M"
echo "2. Backup 172.22.220.20:/data/ to /mac/backup2/ start at:"
date
$RSYNC john@172.22.220.20:/home/john/data/ /mac/backup2/
echo "Backup end at:"
date
exit 0
```
* Using `crontab` command to execute the backup script routinely, and record in a log file, for example, execute the command `~/backup.sh > ~/backup.log` in 5:10am everyday:
  * add executable permission
```bash
chmod +x ~/backup.sh
```
  * open crontab and edit it by the following command: 
```bash
crontab -e	# or crontab ~/cronjob
```
  * type in the following lines or write the following in a file (i.e. `~/crontab`):
```bash
# minute hour day_in_month month day_in_week command
     10   5  * * *   ~/backup.sh > ~/backup.log
```
  * exit and save

## Linux中for循环的几个常用写法

* 数字型循环
```bash
#!/bin/bash
# 1
for((i=1;i<=10;i++));
do
	echo $(expr $i \* 3 + 1);
done
# 2
for i in $(seq 1 10)
do
	echo $(expr $i \* 3 + 1);
done
# 3
for i in {1..10}
do
	echo $(expr $i \* 3 + 1);
done
# 4
awk 'BEGIN{for(i=1; i<=10; i++) print i}'
exit 0
```
* 字符型循环
```bash
#!/bin/bash
# 1
for i in `ls`;
do 
	echo $i is file name\! ;
done
# 2
for i in $* ;
do
	echo $i is input chart\! ;
done
# 3
for i in f1 f2 f3 ;
do
	echo $i is appoint ;
done
# 4
list="rootfs usr data data2"
for i in $list;
do
	echo $i is appoint ;
done
exit 0
```
* 路径查找型循环
```bash
#!/bin/bash
# 1
for file in /proc/*;
do
	echo $file is file path \! ;
done
# 2
for file in $(ls *.sh)
do
	echo $file is file path \! ;
done
exit 0
```

## Linux下将文件夹命名为今天的日期的方法

```bash
alias today="date +F%"	# +F% format is like 2020-01-01
mkdir results-$(today)
```

## Linux下使ls命令只显示目录的方法

```bash
ls -F | grep '/$'	#最易用，若将其结果保存在变量里，可用循环遍历并用cd访问
ls -l | grep '^d'	#显示信息最完整
```
附：ls与cd连用示例
```bash
#!/bin/bash
dir=`ls -F | grep "/$"`
for i in $dir
do
	cd $i
	files=`ls`	# or files=$(ls)
	for j in $files
	do
		cat $j >> /home/test/share/all.out
	done
	cd ..
done
exit 0
```

## Linux下替换^M字符方法

在Linux下使用`vi`或`cat -A`查看一些在Windows下创建的文本文件，有时会发现在行尾有一些`^M`，既影响文件的查看，也影响利用`awk`等命令对文件进行操作，见下：
```bash
test@bioinfo_docker:~/share$ cat -A text.txt
1^M$
2^M$
3^M$
4^M$
5^M$
6^M$
7^M$
8^M$
```
有如下解决方法：

* 使用`dos2unix`命令（**建议所有在win下创建的文件均先在root用户下运行此命令转换一下格式**）
```bash
dos2unix text.txt
```
* 使用vi的替换功能，在vi的命令模式下输入:
```
:%s/^M$//g			#去掉行尾的^M
:%s/^M//g			#去掉所有的^M
:%s/^M/[ctrl-v]+[enter]/g	#将^M替换成回车
:%s/^M/\r/g			#将^M替换成回车
```
* 使用`sed`命令
```bash
sed -e 's/^M/\n/g' text.txt	#注意：^M需使用[ctrl-v] [ctrl-m]生成，并非直接输入
```
* 注：在vim的.vimrc文件中把fileformat=unix去掉便不会显示（默认不显示^M）

## Linux下将多行文件合并为一行

* 注意：对于win下文件（非linux创建），建议先运行`dos2unix`命令
* 使用`awk`命令
```bash
awk 'BEGIN{ORS=" "} {print}' text.txt
# ORS：输出行分隔符，默认\n，此处改为空格，故可使两行合并
awk 'BEGIN{RS=EOF} {gsub(/\n/," "); print}' text.txt
# RS：输入行分隔符，默认\n，此处改为EOF文件结尾，故可一次读入整个文件，方便替换操作
```
* 使用`sed`命令：`sed`默认按行处理，`N`可以让其读入下一行，再对\n进行替换，这样就可以将两行并做一行。但为了将所有行并作一行，需要采用其跳转功能。`:flag`在代码开始处设置一个标记`flag`，在代码执行到结尾处时利用跳转命令`t flag`重新跳转到标号`flag`处，重新执行代码，这样就可以递归将所有行合并成一行。
```bash
sed ':flag; N; s/\n/ /g; t flag;' text.txt
```
* 使用`xargs`命令
```bash
cat text.txt | xargs
```
* 附：文件合并、行筛选、多行合并脚本示例
```bash
#!/bin/bash
dir=`ls -F | grep "/$"`
for i in $dir
do
	echo $i >> all.out
	cd $i
	files=`ls`
	for j in $files
	do
		dos2unix $j
		echo $j >> ../all.out
		awk 'BEGIN{ORS=" "}; /^[^0-9]/{print}' $j >> ../all.out
		echo -e "\n" >> ../all.out
	done
	cd ..
done
exit 0
```

## Linux中cut命令

* cut是一个选取命令，其以行为单位，选择性输出符合条件的内容到标准输出，使用格式为：
```bash
cut <option> <file>
```
* 命令选项：
```bash
-b <输出范围>, --bytes=LIST：设置输出的字节数或范围
-c <输出范围>, --characters=LIST：设置输出的字符数或范围
-d <分隔符>, --delimiter=DELIM：指定列（或字段）的分隔字符。默认分隔符是制表符Tab。只能和-f选项一起使用
-f <输出范围>, --fields=LIST：设置输出字段，默认字段分隔符是空格
-n：与命令选项-b一起使用，不分割宽字符
--complement：反向选择输出字节、字符或字段
-s, --only-delimited：若行没有分隔符，则不显示该行。此选项只能和-f选项一起使用
--output-delimiter=STRING：使用字符串作为输出分隔符，默认是输入分隔符
--help：显示帮助信息
--version：显示版本信息
```
* 使用示例
```bash
cut -c 3 text.txt		#输出第三位上的字符
cut -c 3-5 text.txt		#输出第三至五位（均含）上的字符
cut -c 3-4,6 text.txt		#输出第三至五位、第六位上的字符
cut -c 3- text.txt		#输出第三个字符到最后一个字符
cut -c -2,5- text.txt		#输出开始至第二个字符、第五个字符至最后一个字符
cut -b 3-5 text.txt		#使用字节为单位来进行，若文件以单字节编码字符，则与c结果一致
cut -f 2 text.txt		#输出第二列，默认列分隔符为Tab
cut -f 2,3,5 text.txt		#输出第二、三、五列，默认列分隔符为Tab
cut -d ' ' -f 2-5 text.txt	#输出第二至五列，列分隔符改为空格
cut -d ',' -f 2- text.txt	#输出第二至最后，列分隔符改为逗号（常见于csv文件）
```

## Ubuntu镜像使用帮助

* [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/)
* Ubuntu的软件源配置文件是`/etc/apt/sources.list`。将系统自带的该文件做个备份，再将该文件替换为下面内容，即可使用TUNA的软件源镜像。
```vim
# ubuntu版本: 18.04 LTS
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# 以下为预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

## Ubuntu下使用apt install XX(or sudo apt install XX)报错Unable to locate package
* 正常情况下，只需要更新软件列表
```bash
sudo apt update
```
* 当上述命令无效时，尝试升级
```bash
sudo apt upgrade
```
* 注：一般情况下更改了软件源之后需要重新`update`

## Linux管理员修改普通用户密码

* root修改普通用户的密码
```bash
sudo passwd <user_name>
```
* [root查看普通用户密码](https://blog.csdn.net/lws123253/article/details/89228589)
* 普通用户修改自己的密码
```bash
passwd
```

## Linux下使用sudo命令出现not in the sudoers file

* 使用`su -`命令切换到root身份。注意该命令有`-`，与`su`不同，在用命令`su`的时候只是切换到root，但没有把root的环境变量传过去，还是当前用户的环境变量，用`su -`命令将环境变量也一起带过去，就像和root登录一样
* 使用`visudo`命令编辑`sudo`权限文件。注意，该命令为一个单词
* 出现vim编辑窗口，在文件合适位置加入`<user_name>	ALL=(ALL) ALL`语句
* 保存退出后，便把自己加入了`sudo`组，可以使用`sudo`命令了

## Linux下安装Miniconda

1. 进入[清华大学开源软件镜像站 - Anaconda镜像使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/anaconda/)，下翻找到Miniconda镜像使用帮助
2. 进入[Miniconda安装包下载地址](https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/)
3. 直接下载对应版本的`sh`文件，或将光标定位在所需文件，复制链接地址，再使用`wget -c <url>`下载
4. 运行该文件，即执行命令`bash Miniconda***.sh`，根据提示按Enter键或者输入yes即可
5. 安装完成后，会在家目录`~`下生成一个`Minconda3`文件夹
6. 运行`source .bashrc`，激活`conda`，这时会发现终端前出现了`(base)`，表示已进入`conda`环境
7. `conda`安装成功之后，逐行运行下面的命令，添加国内镜像，方便以后下载软件。
  ```
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda
  conda config --set show_channel_urls yes
  ```
8. 注：若不希望终端前一直显示`base`，仅在需用`conda`时才调用，可在`.bashrc`最后加上一句`conda deactivate`使其默认不运行。需使用时输入`conda activate`激活之。

## WSL(Windows Subsystem for Linux)更改登录用户

* 对于Ubuntu系统，在`Windows PowerShell`中运行`ubuntu config --default-user <username>`即可

## WSL中Vim字体改变的解决方案

* 问题：安装好WSL版本的Ubuntu1804之后，由于默认字体不适合长时间阅读且不美观，便从属性窗口设置了新字体，但是发现在启动Vim的时候会出现字体变回原来的字体的情况。
* 解决方法：运行`regedit`修改注册表，定位至`HKEY_CURRENT_USER\Console\C:_Program Files_WindowsApps_CanonicalGroupLimited.Ubuntu18.04onWindows_1804<...>ubuntu1804.exe`，方括号中内容根据实际情况修改，在其中添加：`CodePage`（`DWORD`类型、值`0x01b5`）

## Linux下的变量操作

### 变量替换

* 巧记方法：`#`和`%`是删除符号。在键盘上，`#`在`$`的左边，所以是从左边开始删除，`%`在`$`的右边，所以是从右边开始删除。`/`是替换符。
```bash
${var#pattern}  从变量头部开始匹配模式，将符合的最短数据删除
${var##pattern} 从变量头部开始匹配模式，将符合的最长数据删除
${var%pattern}  从变量尾部开始匹配模式，将符合的最短数据删除
${var%%pattern} 从变量尾部开始匹配模式，将符合的最长数据删除
${var/oldPattern/newPattern}  将第一个符合旧模式的数据替换为新模式
${var//oldPattern/newPattern} 将全部符合旧模式的数据替换为新模式
```
* 示例
1. 输出文件的后缀
```bash
# 用#会匹配上第一个点和之前的内容，删除之后就获得了后缀名txt
var1=me.txt
echo ${var1#*.} 
# 假如文件名中本身含有点(.)就会让上一种写法失效，此时应该用##来匹配最长数据，同样获得txt
var2=me.pdf.txt
echo ${var2##*.} 
```
2. 输出文件名
```bash
# %删除了从右边起第一个点及其右边的字符，仅剩文件名
echo ${var1%%.}
me
# 假如文件名中本身含有点也是会让上一种写法失效，此时需用%来做最短匹配
echo ${var2%.}
me.pdf
```
3. 替换部分字符
```bash
var=hello123hello123
# 替换第一个
echo ${var/123/456}
hello456hello123
# 全部替换
echo ${var//123/456}
hello456hello456
```

### 变量声明操作
![变量声明操作](https://liuyujie0136.github.io/Sci-Tech-Notes/linux/linux-var.png)

## Linux下nohup命令

`nohup`英文全称no hang up（不挂起），用于在系统后台不挂断地运行命令，退出终端不会影响程序的运行。在默认情况下（非重定向时），程序会输出一个名叫 nohup.out 的文件到当前目录下，如果当前目录的 nohup.out 文件不可写，则输出重定向到 $HOME/nohup.out 文件中。

**语法格式：**
```bash
nohup Command [ Args ] [ & ]
```

**参数说明：**
```bash
Command  要执行的命令。
Args     一些参数，可以指定输出文件。
&        让命令在后台执行，终端退出后命令仍旧执行。
```

**示例：**

以下命令在后台执行1.sh脚本，并重定向输入到1.log文件：

```bash
nohup 1.sh > 1.log 2>&1 &

# 2>&1: 将标准错误2重定向到标准输出&1，标准输出&1再被重定向输入到1.log文件中
# 0 – stdin (standard input，标准输入)
# 1 – stdout (standard output，标准输出)
# 2 – stderr (standard error，标准错误输出)
```

如果要停止运行，你需要使用以下命令查找到`nohup`运行脚本到PID，然后使用`kill`命令来删除：
```bash
ps -aux | grep "1.sh" 
kill -9 <PID>

# 参数说明：
a   显示所有程序
u   以用户为主的格式来显示
x   显示所有程序，不区分终端机
```

## awk同时处理多个文件

`awk`的数据输入有两个来源，标准输入和文件，后一种方式支持多个文件。

1. `shell`的`Pathname Expansion`方式
```bash
awk '{...}' *.txt
# *.txt先被shell解释，替换成当前目录下的所有*.txt，如当前目录有1.txt和2.txt，则命令最终为awk '{...}' 1.txt 2.txt
```

2. 直接指定多个文件
```bash
awk '{...}' a.txt b.txt c.txt ...
# awk对多文件的处理流程是，依次读取各个文件内容，先读a.txt，再读b.txt....
```

那么，在多文件处理的时候，如何判断`awk`目前读的是哪个文件，而依次做对应的操作呢？

1. 当`awk`读取的文件只有两个的时候，比较常用的方法有：
   1. `awk 'NR==FNR{...}NR>FNR{...}' file1 file2`
   2. `awk 'NR==FNR{...}NR!=FNR{...}' file1 file2`
   3. `awk 'NR==FNR{...;next}{...}' file1 file2`
2. 了解`FNR`(已读入当前文件的记录数)和`NR`(已读入的总记录数)这两个`awk`内置变量的意义就很容易知道这些方法是如何运作的：
   1. 对于`awk 'NR==FNR{...}NR>FNR{...}' file1 file2`，读入file1的时候，已读入file1的记录数FNR一定等于awk已读入的总记录数NR，因为file1是awk读入的首个文件，故读入file1时执行前一个命令块{...}。读入file2的时候，已读入的总记录数NR一定大于读入file2的记录数FNR，故读入file2时执行后一个命令块{...}。
   2. 对于`awk 'NR==FNR{...;next}{...}' file1 file2`，读入file1时，满足NR==FNR，先执行前一个命令块，但因为其中有next命令，故后一个命令块{...}是不会执行的。读入file2时，不满足NR==FNR，前一个命令块{..}不会执行，只执行后一个命令块{...}。
3. 当`awk`处理的文件超过两个时，显然上面那种方法就不适用了。因为读第3个文件或以上时，也满足`NR>FNR (NR!=FNR)`，显然无法区分开来。所以就要用到更通用的方法了：
   1. 利用当前被处理参数标志: `awk 'ARGIND==1{...}ARGIND==2{...}ARGIND==3{...}...' file1 file2 file3 ...`
   2. 利用命令行参数数组: `awk 'FILENAME==ARGV[1]{...}FILENAME==ARGV[2]{...}FILENAME==ARGV[3]{...}...' file1 file2 file3 ...`
   3. 把文件名直接加入判断，但不通用: `awk 'FILENAME=="file1"{...}FILENAME=="file2"{...}FILENAME=="file3"{...}...' file1 file2 file3 ...`
