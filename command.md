---
typora-root-url: ./..\Configuration\SomeFile\MdPicture
---

# 1. Computer

> [超算入门课程1 致敬"银河•天河"40年超级计算奋进征途 | 超算小站 (mrzhenggang.com)](https://nscc.mrzhenggang.com/supercomputer-courses/history-40-years/)
>
> [Shell笔记_doordiey的博客-CSDN博客](https://blog.csdn.net/doordiev/article/details/120761851)
>
> [一篇教会你写90%的shell脚本 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/264346586#:~:text=shell脚本就,用编译即可运行。)



```python
### 解压和压缩命令
zip -r /home/myhome.zip /home
unzip /home/myhome.zip
unzip -d /opt/tmp /home/myhome.zip
tar -zxvf ex.tar.gz /opt/VASP #解压 
tar -zcvf ex.tar.gz /opt/ex1.txt /opt/ex2.txt #压缩

### linux删除指定后缀名的命令
find /home/user/files -type f -name "*.txt" -delete

### 卸载wsl版本
wsl --list
wsl --unregister CentOS7
wsl --user root
# https://zhuanlan.zhihu.com/p/466001838
# su命令不能切换root
# sudo passwd root

# screen 关掉本地计算机的同时，而远程服务器继续跑代码。
# 创建一个新的screen：
screen -S test
# 查看刚才创建的screen：
screen -ls
# 退出screen
ctrl+a+d 
# 重新打开screen：
screen -r <15659.test>
# 使用删除命令：
screen -X -S <16283.you> quit


# nohup 实现命令后台运行并输出或记录到指定日志文件
# nohup 是不挂断的意思(no hang up)
# tail 可以实时监视向日志文件增加的信息。
nohup cp -r /Data /zhoufyData > output.log 2>&1 &
tail -f log.txt
# 查找到 nohup 运行脚本的 PID (两种方法)
ps -aux | grep "cp -r /Data /zhoufyData"
ps -def | grep "cp"
# 停止运行
kill -9  进程号PID


# 搜索 Linux 中的文件夹
find ~/ZSW/VASP/DFT-Fracture/zhoufyData -iname "CH3CH3"
# 查找当前目录下的文件，输出绝对路径
find . -name OUTCAR > paths.log
sed -i 's/OUTCAR$//g' paths.log
sed -i "s#^.#${PWD}#g" paths.log
# 输出第一列中大于1的数字
awk '$1>1.00 {print}' 1.txt

# Linux之统计文件夹中文件个数以及目录个数
<https://zhuanlan.zhihu.com/p/296320428>
ls -l ./|grep "^d"|wc -l


### wsl创建
Invoke-WebRequest -Uri https://wsldownload.azureedge.net/Ubuntu_2004.2020.424.0_x64.appx -OutFile Ubuntu20.04.appx -UseBasicParsing

Rename-Item .\Ubuntu20.04.appx Ubuntu.zip
Expand-Archive .\Ubuntu.zip -Verbose
cd .\Ubuntu\
.\ubuntu2004.exe

sudo passwd
# 默认登录账户
sudo vim /etc/wsl.conf

[user]
default  = feiyu
```



# 2. Anaconda

[Anaconda conda](https://blog.csdn.net/chenxy_bwave/article/details/119996001)

```python
### conda的更新
conda install anaconda 

### 查看有哪些虚拟环境
conda env list

### 环境切换
conda activate Deep
conda deactivate Deep

### 增删新环境
conda create -n Deep python=3.9
conda remove -n Deep --all

### 查询包的安装情况
conda list 
conda search package_name
conda update package_name 
conda install package_name
conda uninstall package_name

### 环境打包
conda install -c conda-forge conda-pack
conda pack -n 虚拟环境名称 -o output.tar.gz
tar -xzvf output.tar.gz

### 使用conda pack进行环境迁移 
### Conda-pack 是一个命令行工具，用于打包 conda 环境，其中
### 包括该环境中安装的软件包的所有二进制文件。
conda pack -n <env_name>
mkdir new_env
tar -xzvf attnGAN.tar.gz -C /root/miniconda3/envs/new_new

# pip install(ERROR: Could not install packages due to an OSError:)
pip install pymysql -i https://pypi.tuna.tsinghua.edu.cn/simple
清华：https://pypi.tuna.tsinghua.edu.cn/simple
阿里云：http://mirrors.aliyun.com/pypi/simple


### 检查pytorch是否安装成功 
import torch # 如果pytorch安装成功即可导入
print(torch.cuda.is_available()) # 查看CUDA是否可用
print(torch.cuda.device_count()) # 查看可用的CUDA数量
print(torch.version.cuda) # 查看CUDA的版本号

### 确认CUDNN安装是否成功
import torch
print(torch.backends.cudnn.version())
# 能够正确返回8200
from torch.backends import cudnn # 若正常则静默
cudnn.is_available() 
# 若正常返回True
a=torch.tensor(1.)
cudnn.is_acceptable(a.cuda()) 
# 若正常返回True
 
```



# 3. Git

[The use of .gitignore of zhihu](https://zhuanlan.zhihu.com/p/52885189)

```python
git config --global user.name "zhoufy20"
git config --global user.email "zhoufy20@lzu.edu.cn"
ssh-keygen -t rsa -C "zhoufy20@lzu.edu.cn"
ssh -T git@github.com

git status --ignored  #查看状态，包括忽略的文件
git rm -r --cached .  #清除缓存

git branch -u origin/main main  #设置本地 main 分支跟踪远程 origin 的 main 分支
git remote set-head origin main  #设置远程 origin 的 HEAD 指向 main 分支
git branch -d <branchname>  #删除本地分支
git branch -d -r <branchname>  #删除远程分支
```



# 4. jekyll on Windows

Jekyll 是一个静态站点生成器，内置 GitHub Pages 支持和简化的构建过程。

```ruby
1.安装 ruby
https://rubyinstaller.org/downloads/ 
这里需要安装带有Devkit的，devkit是window系统下必须的。
输入cmd打开命名窗口，输入`ruby -v`如果出现了ruby的版本号则说明已经安装好了。

2.安装 Rubygems
https://rubygems.org/pages/download 
下载.zip版本直接解压缩，输入`ruby setup.rb`,
等待安装完成后输入`gem -v`如果出现gem的版本号，则说明安装完成。

3.安装 Jekyll
命令行输入 `gem install jekyll`，等待安装完成后，
输入jekyll -v ，正常出现版本号即说明安装完成。

4.安装依赖，开启本地服务
有些模板可能需要安装包的依赖，在下载好的模板的根目录下先执行`gem install bundler`命令，
来安装gem的依赖包，然后运行`bundle install`和`jekyll server`。


```



















# 4. Docker ( Linux )

## 1. linux内核版本依赖  **kernel version >= 3.8**

```dockerfile
uname -a | awk '{split($3, arr, "-"); print arr[1]}'
```



## 2. 添加Docker repository yum源

```dockerfile
# 国内源, 速度更快, 推荐
sudo yum-config-manager \
    --add-repo \
    https://mirrors.ustc.edu.cn/docker-ce/linux/centos/docker-ce.repo
```

>yum-config-manager: command not found命令找不到的解决方法:
>
>1. yum -y install yum-utils
>2. *重新加载 yum*
>3. yum clean all
>4. yum makecache

## 3. 开始安装Docker Engine

```dockerfile
sudo yum makecache fast
sudo yum install docker-ce docker-ce-cli containerd.io
```

## 4. 开启Docker

```dockerfile
### 开启Docker
sudo systemctl enable docker
sudo systemctl start docker

### 验证是否安装成功
sudo docker run hello-world
```

