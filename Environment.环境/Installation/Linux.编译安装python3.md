# Linux下编译安装python3

Linux下大部分系统默认自带python2.x的版本，最常见的是python2.6或python2.7版本，默认的python被系统很多程序所依赖，比如centos下的yum就是python2写的，所以默认版本不要轻易删除，否则会有一些问题，如果需要使用最新的Python3那么我们可以编译安装源码包到独立目录，这和系统默认环境之间是没有任何影响的，python3和python2两个环境并存即可

首先去python官网下载python3的源码包，网址：https://www.python.org/

进去之后点击导航栏的Downloads，也可以鼠标放到Downloads上弹出菜单选择Source code，表示源码包，这里选择最新版本3.5.1，当然下面也有很多其他历史版本，点进去之后页面下方可以看到下载链接，包括源码包、Mac OSX安装包、Windows安装包


这里选择第一个下载即可，下载的就是源码包：Python-3.5.1.tgz，下载好之后上传到linux系统，准备安装


## 安装编译环境
python安装之前需要一些必要的模块，比如openssl，readline等，如果没有这些模块后来使用会出现一些问题，比如没有openssl则不支持ssl相关的功能，并且pip3在安装模块的时候会直接报错；没有readline则python交互式界面删除键和方向键都无法正常使用，至于需要什么模块在make完之后python会给出提示，通过提示进行安装即可装全， 另外感谢园友的Glory_Lion的回复；下面是需要提前预装的依赖：

```sh
yum -y install gcc gcc-c++
yum -y install gcc automake autoconf libtool make

yum -y install zlib zlib-devel
yum -y install bzip2 bzip2-devel
yum -y install ncurses ncurses-devel
yum -y install readline readline-devel
yum -y install openssl openssl-devel openssl-static
yum -y install xz lzma xz-devel
yum -y install sqlite sqlite-devel
yum -y install gdbm gdbm-devel
yum -y install tk tk-devel
yum -y install libffi libffi-devel

# yum -y install zlib zlib-devel openssl openssl-devel
yum -y install glib2-devel pcre-devel gzip-devel
# yum -y install libffi-devel
# yum -y install zlib zlib-devel
yum -y install libjpeg libjpeg-devel
yum -y install freetype freetype-devel
```


## 安装 Python

* [Python 3.9.5](https://www.python.org/downloads/release/python-395/)
* [Python 3.9.5 Gzipped source tarball](https://www.python.org/ftp/python/3.9.5/Python-3.9.5.tgz)
* [Python 3.8.10](https://www.python.org/downloads/release/python-3810/)
* [Python 3.8.10 Gzipped source tarball](https://www.python.org/ftp/python/3.8.10/Python-3.8.10.tgz)
* [Python 3.7.10](https://www.python.org/downloads/release/python-3710/)
* [Python 3.7.10 Gzipped source tarball](https://www.python.org/ftp/python/3.7.10/Python-3.7.10.tgz)
* [Python 3.7.9](https://www.python.org/downloads/release/python-379/)
* [Python 3.7.9 Gzipped source tarball](https://www.python.org/ftp/python/3.7.9/Python-3.7.9.tgz)
* []()
* []()
* []()
* []()
* []()


安装上面这些python内置模块基本上就比较全了，如果后续有其他必要的模块，会继续补充的，接下来可以安装python了，编译过程中会自动包含这些依赖.

```sh
[root@localhost ~]# mkdir -p APP/Python
[root@localhost ~]# cd APP/Python/
[root@localhost Python]# wget https://www.python.org/ftp/python/3.8.10/Python-3.8.10.tgz
[root@localhost Python]# tar -zxvf Python-3.8.10.tgz
[root@localhost Python]# cd Python-3.8.10/
[root@localhost Python-3.8.10]#
```

```sh
[root@localhost Python-3.8.10]# ./configure --enable-shared CFLAGS=-fPIC
[root@localhost Python-3.8.10]# make
[root@localhost Python-3.8.10]# make install

```


配置编译，因为上面依赖包是用yum安装而不是自己编译的，所以都是安装在系统默认目录下，因此各种选项不用加默认即可生效：
* `./configure --prefix=/usr/python --enable-shared CFLAGS=-fPIC`
* `./configure --enable-shared CFLAGS=-fPIC`

补充一下：
1. `--prefix=/usr/python` 选项会将 python 安装到 `/usr/python` 目录中，
   1. 默认情况下会安装在 `/usr/local/bin/python`
2. 这里加上 `--enable-shared` 和`-fPIC`之后可以将python3的动态链接库编译出来，
   1. 默认情况编译完lib下面只有python3.xm.a这样的文件，python本身可以正常使用，
   2. 但是如果编译第三方库需要python接口的比如caffe等，则会报错；所以这里建议按照上面的方式配置，
3. 另外如果 openssl 不使用系统 yum 安装的，而是使用自己编译的比较新的版本
   1. 可以使用 `--with-openssl=/usr/local/openssl` 这种方式指定，后面目录为openssl实际安装的目录，
   2. 另外编译完还要将openssl的lib目录加入ld运行时目录中即可. 

接下来编译源码：`make`

执行安装：`make install`

整个过程大约5-10分钟，安装成功之后，安装目录就在 `/usr/python`


## 安装之后的操作
安装完成之后要简单做一下配置：即将python库路径添加到/etc/ld.so.conf配置中，然后执行ldconfig生效；或者添加到$LD_LIBRARY_PATH中，这样在接下来运行python3是就不会报找不到库文件的错误了.

系统中原来的python在/usr/bin/python，通过ls -l可以看到，python是一个软链接，链接到本目录下的python2.7

这里不要把这个删除，不对原来默认的环境做任何修改，只新建一个python3的软链接即可，只是需要执行python3代码时python要改成python3，或者python脚本头部解释器要改为`#!/usr/bin/python3`

这里建立有关的软链接如下：

```
ln -s /usr/python/bin/python3 /usr/bin/python3
ln -s /usr/python/bin/pip3 /usr/bin/pip3
```

这样就建立好了，以后直接执行python3命令就可以调用python3了，执行pip3可以安装需要的python3模块；另外如果仔细看python安装目录下的bin目录，实际上python3也是个软链接，链接到python3.5.1，这样多次链接也是为了多个版本的管理更加方便，

python3新版本的安装就是这些，因为我们之前安装了完整的依赖，所以下面问题不存在了，忽略即可，其中的python readline模块也早已经停止更新了，会出现崩溃问题；这里基础环境都是使用系统的依赖，更稳定.

```sh
[root@localhost Python-3.8.10]# python3
python3: error while loading shared libraries: libpython3.8.so.1.0: cannot open shared object file: No such file or directory
[root@localhost Python-3.8.10]#
[root@localhost Python-3.8.10]# find / -name libpython3.8.so.1.0
find: ‘/proc/91197’: No such file or directory
/root/APP/Python/Python-3.8.10/libpython3.8.so.1.0
/usr/local/lib/libpython3.8.so.1.0
[root@localhost Python-3.8.10]#
[root@localhost Python-3.8.10]# cp /usr/local/lib/libpython3.8.so.1.0 /usr/lib/
[root@localhost Python-3.8.10]# cp /usr/local/lib/libpython3.8.so.1.0 /usr/lib64/
[root@localhost Python-3.8.10]# python3
Python 3.8.10 (default, May  8 2021, 17:15:53)
[GCC 8.3.1 20191121 (Red Hat 8.3.1-5)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()
[root@localhost Python-3.8.10]#
```

```sh
[root@localhost Python-3.8.10]# whereis python3.8
python3: /usr/bin/python3 /usr/bin/python3.6 /usr/bin/python3.6m /usr/lib/python3.6 /usr/lib64/python3.6 /usr/local/bin/python3.8 /usr/local/bin/python3.8-config /usr/local/bin/python3 /usr/local/lib/python3.8 /usr/include/python3.6m /usr/share/man/man1/python3.1.gz
[root@localhost Python-3.8.10]#
[root@localhost Python-3.8.10]# cd /usr/bin/
[root@localhost bin]# mv python3 python3.ori
[root@localhost bin]# mv pip3 pip3.ori
[root@localhost bin]#
[root@localhost bin]# ln -s /usr/local/bin/python3.8 /usr/bin/python3
[root@localhost bin]# ln -s /usr/local/bin/pip3 /usr/bin/pip3
[root@localhost bin]#

```





## 可能遇到的问题

另外可能会遇到一个问题就是，python3交互式界面特别不好用，删除不能正常使用，必须按Ctrl+删除键才可以，上下键重复命令也无法使用，左右调整光标也无法使用，这样调试代码效率很低，原因是python3缺少readline这个依赖，默认使用pip3 install readline会提示找不到包，所以只能是手动安装了

因为readline依赖于ncurses库，如果没有这个库，编译readline的时候会提示can not find -lncurses，如果在centos环境下可以执行下面命令安装：

`yum -y install ncurses ncurses-devel`

安装ncurses库之后，再安装readline，readline可以在pypi下载，地址：<https://pypi.python.org/pypi/readline>，这里一定要下载源码包，其余的只能用于python2，下载之后执行下面命令安装：
```
tar -xvzf readline-6.2.4.1.tar.gz
cd readline-6.2.4.1/
python3 setup.py install
```

这样执行完毕就为python3安装了readline，然后再进入交互式界面所有的操作都好用了





### libpython3.8.so.1.0 找不到
```sh
[root@localhost Python-3.8.9]# python3.8
python3.8: error while loading shared libraries: libpython3.8.so.1.0: cannot open shared object file: No such file or directory
[root@localhost Python-3.8.9]#
[root@localhost Python-3.8.9]#
[root@localhost Python-3.8.9]# whereis libpython3.8.so.1.0
libpython3.8.so.1: /usr/local/lib/libpython3.8.so.1.0
[root@localhost Python-3.8.9]#
[root@localhost Python-3.8.9]# cp /usr/local/lib/libpython3.8.so
libpython3.8.so      libpython3.8.so.1.0
[root@localhost Python-3.8.9]# cp /usr/local/lib/libpython3.8.so /usr/lib
lib/     lib64/   libexec/
[root@localhost Python-3.8.9]# cp /usr/local/lib/libpython3.8.so /usr/lib/
[root@localhost Python-3.8.9]# cp /usr/local/lib/libpython3.8.so /usr/lib64/
[root@localhost Python-3.8.9]# cp /usr/local/lib/libpython3.8.so.1.0 /usr/lib/
[root@localhost Python-3.8.9]# cp /usr/local/lib/libpython3.8.so.1.0 /usr/lib64/
[root@localhost Python-3.8.9]#
[root@localhost Python-3.8.9]#
[root@localhost Python-3.8.9]# python3.8
Python 3.8.9 (default, Apr 28 2021, 16:01:26)
[GCC 8.3.1 20191121 (Red Hat 8.3.1-5)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()
[root@localhost Python-3.8.9]#
[root@localhost Python-3.8.9]#
```



## redhat-hardened-cc1 找不到

You need to install redhat-rpm-config which is required by some of the qt switches, probably:
```
sudo dnf install redhat-rpm-config
```

## Linux安装Python新版本之后会出现的问题
1. Yum依赖Python2，所以，需要更改 yum 执行脚本的 python 解释器标注版本
2. 防火墙也依赖 Python 。


