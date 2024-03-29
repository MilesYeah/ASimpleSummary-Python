#  Python pip配置国内源 


众所周知，Python使用pip方法安装第三方包时，需要从 https://pypi.org/ 资源库中下载，但是会面临下载速度慢，甚至无法下载的尴尬，这时，你就需要知道配置一个国内源有多么重要了，通过一番摸索和尝试，总结了一些经验，分享给大家：

给大家推荐几个值得拥有的国内镜像站  [ 个人推荐清华大学pypi镜像站(https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)，每五分钟同步一次，资源丰富，下载速度很快 ] :

* 清华大学：https://pypi.tuna.tsinghua.edu.cn/simple/
* 阿里云：http://mirrors.aliyun.com/pypi/simple/
* 豆瓣：http://pypi.douban.com/simple/

　　接下来，按照不同需要和不同平台依次演示安装方法：

##  方式一：临时使用国内pypi镜像安装
```sh
pip install -i http://pypi.douban.com/simple/ numpy
pip install -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com  
#此参数“--trusted-host”表示信任，如果上一个提示不受信任，就使用这个
```

## 方式二：永久使用国内pypi镜像安装

### 1、 Linux平台安装方式：

（1）创建pip.conf文件

首先运行以下命令
```sh
cd ~/.pip   
# 运行此命令切换目录
```
如果提示目录不存在，自行创建一个(如果目录存在，可跳过此步)，如下：
```sh
mkdir ~/.pip
cd ~/.pip
```
在 .pip目录下创建一个 pip.conf 文件，如下：
```sh
touch pip.conf
```
（2）编辑 pip.conf 文件

首先打开文件，命令如下：
```sh
sudo vi ~/.pip/pip.conf
```
接着，写入以下内容：
```ini
[global] 
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host = https://pypi.tuna.tsinghua.edu.cn  
# trusted-host 此参数是为了避免麻烦，否则使用的时候可能会提示不受信任
```

```ini
[global]

index-url=http://mirrors.aliyun.com/pypi/simple/

[install]

trusted-host=mirrors.aliyun.com
```
然后保存退出即可。




### 2、Window平台安装方式：

（1）新建 pip 配置文件夹，直接在user用户目录中创建一个名为 pip 的文件夹( 即%HOMEPATH%\pip)，如下图所示：

（2）接着在 pip 文件夹中创建一个名为 pip 的文本文件(后缀名由" .txt "改为 " .ini ")，格式如下所示：

文件内容如下：
```ini
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple

[install]
trusted-host = https://pypi.tuna.tsinghua.edu.cn  
```
**trusted-host 此参数是为了避免麻烦，否则使用的时候可能会提示不受信任**


```ini
[global]
index-url=http://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```
修改完成后保存，启动cmd，使用 " pip install xxx "(xxx为你要下载的包名)，即可默认使用国内源下载。

　　


## ref
* [ Python pip配置国内源 ](https://www.cnblogs.com/schut/p/10410087.html)
