---
title: "zsh从源码编译安装"
date: 2020-12-20T17:23:28+08:00
draft: false
tags: ["linux", "zsh", "centos"]
categories: ["linux"]
---

## 原因

Oh-my-zsh想用新的Powerlevel10k主题，安装后发现主题需要zsh版本>=5.1

CentOS7的repo的最新版本为zsh 5.0.2，遂卸载之前的版本，安装新版本。

> You are using ZSH version 5.0.2. The minimum required version for Powerlevel10k is 5.1.
> Type 'echo $ZSH_VERSION' to see your current zsh version.

```sh
$ which zsh
/usr/bin/zsh

$ zsh --version
zsh 5.0.2 (x86_64-redhat-linux-gnu)
```

## 备份原有zshrc配置文件，切换到bash

```sh
$ cp ~/.zshrc ~/.zshrc-backup
# 如果当前是zsh，先切换原有的login shell为bash，防止发生意外，不能Login
$ chsh -s $(which bash)
```

## 卸载原有zsh

```sh
$ yum remove zsh
```

## 下载源码并编译

```sh
# install requirements
yum install git make ncurses-devel gcc autoconf man

# 从Github镜像clone下载最新的release版本5.8源码
git clone -b zsh-5.8 --depth=1 https://github.com/zsh-users/zsh.git temp-zsh
cd temp-zsh

# Pre-configuration
./Util/preconfig

# To configure zsh, from the top level directory, do the command:
./configure

# After configuring, to build zsh, execute the command:
make

# It's then a good idea to check that your build is working properly:
make check

# Install all the necessary files except for the info files.
make install
```

### .configure 配置选项

```sh
Options For Configure
---------------------

The `configure' program accepts many options, not all of which are useful
or relevant to zsh.  To get the complete list of configure options, run
"./configure --help".  The following list should contain most of the
options of interest for configuring zsh.

Configuration:
  --cache-file=FILE     # cache test results in FILE
  --help                # print a help message
  --version             # print the version of autoconf that create configure
  --quiet, --silent     # do not print `checking...' messages
  --no-create           # do not create output files

Directories:
  --prefix=PREFIX       # install host independent files in PREFIX [/usr/local]
  --exec-prefix=EPREFIX # install host dependent files in EPREFIX [PREFIX]
  --bindir=DIR          # install user executables in DIR [EPREFIX/bin]
  --infodir=DIR         # install info documentation in DIR [PREFIX/info]
  --mandir=DIR          # install man documentation in DIR [PREFIX/man]
  --srcdir=DIR          # find the sources in DIR [configure dir or ..]
  --datadir=DATADIR     # install shared files in DATADIR [PREFIX/share]

Features:
  --enable-FEATURE      # enable use of this feature
  --disable-FEATURE     # disable use of this feature

Here is the list of FEATURES currently supported.  Defaults are shown in
brackets, though a value shown as `yes' (equivalent to --enable-FEATURE)
will be ignored if your OS doesn't support that feature.

zsh-debug            # compile debugging features into zsh [no]
zsh-mem              # use zsh's memory allocators [no]
zsh-mem-debug        # debug zsh's memory allocators [no]
zsh-mem-warning      # turn on warnings of memory allocation errors [no]
zsh-secure-free      # turn on memory checking of free() [no]
zsh-hash-debug       # turn on debugging of internal hash tables [no]
etcdir=directory     # default directory for global zsh scripts [/etc]
zshenv=pathname      # the path to the global zshenv script [/etc/zshenv]
zshrc=pathname       # the path to the global zshrc script [/etc/zshrc]
zlogin=pathname      # the path to the global zlogin script [/etc/zlogin]
zprofile=pathname    # the path to the global zprofile script [/etc/zprofile]
zlogout=pathname     # the path to the global zlogout script [/etc/zlogout]
fndir=directory      # the directory where shell functions will go
                     # [DATADIR/zsh/VERSION/functions]
site-fndir=directory # the directory where site-specific functions can go
                     # [DATADIR/zsh/site-functions]
additional-path      # add directories to default function path [<none>]
function-subdirs     # if functions will be installed into subdirectories [no]
dynamic              # allow dynamically loaded binary modules [yes]
largefile            # allow configure check for large files [yes]
locale               # allow use of locale library [yes]

# 这是.configure不加选项生成的配置
zsh configuration
-----------------
zsh version               : 5.8
host operating system     : x86_64-pc-linux-gnu
source code location      : .
compiler                  : gcc
preprocessor flags        :
executable compiler flags :  -Wall -Wmissing-prototypes -O2
module compiler flags     :  -Wall -Wmissing-prototypes -O2 -fPIC
executable linker flags   :   -s -rdynamic
module linker flags       :   -s -shared
library flags             : -lgdbm -ldl -lncursesw -lrt -lm  -lc
installation basename     : zsh
binary install path       : /usr/local/bin
man page install path     : /usr/local/share/man
info install path         : /usr/local/share/info
functions install path    : /usr/local/share/zsh/5.8/functions
See config.modules for installed modules and functions.
```

## 安装后，查看当前zsh版本

```sh
❯ which zsh
/usr/local/bin/zsh
❯ zsh --version
zsh 5.8 (x86_64-pc-linux-gnu)
```

## 切换login shell到新编译的zsh

```sh
# zsh 路径添加到 /etc/shells 中
which zsh >> /etc/shells

# 切换当前用户的默认shell到新编译的zsh
chsh -s $(which zsh)

# 可选的，Change other user's login shell.
chsh -s {{path/to/shell_binary}} {{username}}
```

