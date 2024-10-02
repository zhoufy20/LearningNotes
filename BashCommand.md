# **Some significant commands** 

## *1. Linux*

> [超算入门课程1 致敬"银河•天河"40年超级计算奋进征途 | 超算小站 (mrzhenggang.com)](https://nscc.mrzhenggang.com/supercomputer-courses/history-40-years/)
>
> [Shell笔记_doordiey的博客-CSDN博客](https://blog.csdn.net/doordiev/article/details/120761851)
>
> [一篇教会你写90%的shell脚本 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/264346586#:~:text=shell脚本就,用编译即可运行。)

```bash
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
# 显示深度为3级，只显示目录
tree -d -L 3

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


# 默认登录账户
sudo passwd
sudo vim /etc/wsl.conf
[user]
default  = feiyu
```



## *2. Anaconda*

[Anaconda conda](https://blog.csdn.net/chenxy_bwave/article/details/119996001)

```bash
### conda的更新
conda install anaconda 

### 查看有哪些虚拟环境
conda env list

### 环境切换
conda activate Deep
conda deactivate Deep

### 增删新环境
conda create -n envname python=3.9
conda remove -n envname --all

### 查询包的安装情况
conda list 
conda search package_name
conda update package_name 
conda install package_name
conda uninstall package_name

### conda pack 环境打包
conda install -c conda-forge conda-pack
conda pack -n 虚拟环境名称 -o output.tar.gz
tar -xzvf output.tar.gz -C xxx/miniconda/envs/xxx

# pip install(ERROR: Could not install packages due to an OSError:)
pip install pymysql -i https://pypi.tuna.tsinghua.edu.cn/simple
清华：https://pypi.tuna.tsinghua.edu.cn/simple
阿里云：http://mirrors.aliyun.com/pypi/simple
# 恢复为默认源
conda config --remove-key channels
# 查看源的相关信息
conda config --show
# 切换国内清华源
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
# 切换国内中科大源
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/
conda config --set show_channel_urls yes

### 检查pytorch是否安装成功 
import torch # 如果pytorch安装成功即可导入
print(torch.cuda.is_available()) # 查看CUDA是否可用
print(torch.cuda.device_count()) # 查看可用的CUDA数量
print(torch.version.cuda) # 查看CUDA的版本号
### 确认CUDNN安装是否成功
import torch
print(torch.backends.cudnn.version()) # 能够正确返回8200
from torch.backends import cudnn # 若正常则静默
cudnn.is_available() # 若正常返回True
a=torch.tensor(1.)
cudnn.is_acceptable(a.cuda()) # 若正常返回True

```



## *3. Git with github*

[The use of .gitignore of zhihu](https://zhuanlan.zhihu.com/p/52885189)

```bash
git config --global user.name "zhoufy.xjtu@gmail.cn"
git config --global user.email "zhoufy.xjtu@gmail.cn"
ssh-keygen -t rsa -b 4096 -C "zhoufy.xjtu@gmail.cn"
ssh -T git@github.com

git status --ignored  #查看状态，包括忽略的文件
git rm -r --cached .  #清除缓存

git branch -u origin/main main  #设置本地 main 分支跟踪远程 origin 的 main 分支
git remote set-head origin main  #设置远程 origin 的 HEAD 指向 main 分支
git branch -d <branchname>  #删除本地分支
git branch -d -r <branchname>  #删除远程分支

# 强制拉取以覆盖本地更改
# 执行后本地分支将指向远程仓库的最新版本，并且本地的更改将被覆盖
git fetch --all
git reset --hard origin/main
```







## *4. Website*

**搭建个人网站**

> 【云服务器】请在安全组放行 8888 端口
> 外网面板地址: https://39.105.13.69:8888/f949e5e2
> 内网面板地址: https://172.19.0.20:8888/f949e5e2
> username: gjtas6yn
> password: e0f5c5f6



**jekyll on Windows**

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





## *5. Docker*

**Docker** 是一个开源的应用容器引擎，可以轻松的为任何应用创建一个轻量级的、可移植的、自给自足的容器。与虚拟机通过操作系统实现隔离不同，容器技术只隔离应用程序的运行时环境但容器之间可以共享同一个操作系统，这里的运行时环境指的是程序运行依赖的各种库以及配置。

docker中有这样几个概念: `dockerfile`, `image`, `container`, 实际上你可以简单的把`image`理解为可执行程序, `container`就是运行起来的进程。那么写程序需要源代码，那么“写”image就需要dockerfile，dockerfile就是image的源代码，docker就是"编译器"。

```do
# 列出本机的所有 image 文件：
docker images
# 列出本机的所有 docker容器：
docker ps -a
# 列出本机的运行中的 docker容器：
docker ps

# 搜索我们想要的anaconda镜像
docker search anaconda

# 拉取原始镜像命令
docker pull whereyour/what:latest
# 在原命令前缀加入加速镜像地址 
docker pull dockerpull.com/whereyour/what:latest

# 用continuumio/anaconda3镜像创建一个名为test的容器
docker run --name testname -idt continuumio/anaconda3
# 进入test容器
docker exec -it testname /bin/bash

# 在本地环境中将本地环境复制到docker中
docker cp /home/deroot/miniconda3/envs/cpfnetgpu test:/opt/conda/envs
docker cp /home/deroot/cbpfnet test:/root/

# 将容器保存为镜像
docker commit -a 'author' -m 'instruction' testname imagename
# 将镜像存为压缩包
docker save -o testname.tar imagename
# 读取镜像
docker load -i testname.tar
# 用testname镜像创建一个名为create_test的容器
docker run --name creat_test -idt image_test

# 启动容器 停止容器 删除容器 删除镜像-需先删除关联的容器
docker start 'CONTAINER ID'
docker stop 'CONTAINER ID'
docker rm 'CONTAINER ID'
docker rmi 'IMAGE ID'
```







## *6. Web Scraping*

爬虫（又被称为网页蜘蛛，网络机器人）就是模拟浏览器发送网络请求，接收请求响应，可以按照一定的规则，自动地抓取互联网信息的程序。爬虫技术，虽说有个诡异的名字，第一反应是那种软软的蠕动的生物，但它却是一个可以在虚拟世界里，无往不前的利器。

```python
from bs4 import BeautifulSoup	# 网页解析，获取数据
import re	# 正则化表达式，进行文字匹配
import urllib.request, urllib.error	# 制定URL，获取网页数据	
import xlwt	# 进行excel操作
import sqlite3 	# 进行SQLite数据库操作
```





## *7. GPUs Parallel*

> [Distributed communication package - torch.distributed — PyTorch 2.4 documentation](https://pytorch.org/docs/stable/distributed.html)
>
> [科研第五步：如何使用DDP分布式多GPU并行跑pytorch深度学习训练_ddp 如何使用-CSDN博客](https://blog.csdn.net/fs1341825137/article/details/123233295#:~:text=这里基于pytorc)

在深度学习的炼丹之路上，多GPU的使用如同助燃剂，能够极大地加速模型的训练和测试。根据不同的GPU数量和内存配置，我们可以选择多种策略来充分利用这些资源。

在PyTorch中，有多种方式可以实现多GPU的并行计算，包括`DataParallel`、`DistributedDataParallel`以及手动模型拆分等。

- DataParallel（DP）：Parameter Server模式，一张卡为reducer，实现也超级简单，一行代码。

- DistributedDataParallel（DDP）：All-Reduce模式，本意是用来分布式训练，但是也可用于单机多卡。

每种方式都有其适用的场景和优缺点，我们需要根据具体的任务和数据集来选择合适的策略，主要分为**数据并行和模型并行二种策略。**

- 模型并行：模型大到单个显卡放不下的地步，就把模型分为几个部分分别放于不同的显卡上单独运行，各显卡输入相同的数据

- 数据并行：不同的显卡输入不同的数据，运行完全相同的完整的模型（更为常用）。





## 8.  Visual Studio Code (vscode) 配置LaTeX

[Visual Studio Code (vscode)配置LaTeX - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/166523064)

```latex
{
 //------------------------------LaTeX 配置----------------------------------
    // 设置是否自动编译
    "latex-workshop.latex.autoBuild.run":"never",
    //右键菜单
    "latex-workshop.showContextMenu":true,
    //从使用的包中自动补全命令和环境
    "latex-workshop.intellisense.package.enabled": true,
    //编译出错时设置是否弹出气泡设置
    "latex-workshop.message.error.show": false,
    "latex-workshop.message.warning.show": false,
    // 编译工具和命令
    "latex-workshop.latex.tools": [
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "-outdir=%OUTDIR%",
                "%DOCFILE%"
            ]
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        }
    ],
    // 用于配置编译链
    "latex-workshop.latex.recipes": [
        {
            "name": "XeLaTeX",
            "tools": [
                "xelatex"
            ]
        },
        {
            "name": "PDFLaTeX",
            "tools": [
                "pdflatex"
            ]
        },
        {
            "name": "BibTeX",
            "tools": [
                "bibtex"
            ]
        },
        {
            "name": "LaTeXmk",
            "tools": [
                "latexmk"
            ]
        },
        {
            "name": "xelatex -> bibtex -> xelatex*2",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        },
        {
            "name": "pdflatex -> bibtex -> pdflatex*2",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        }
    ],
    //文件清理。此属性必须是字符串数组
    "latex-workshop.latex.clean.fileTypes": [
        "*.aux",
        "*.bbl",
        "*.blg",
        "*.idx",
        "*.ind",
        "*.lof",
        "*.lot",
        "*.out",
        "*.toc",
        "*.acn",
        "*.acr",
        "*.alg",
        "*.glg",
        "*.glo",
        "*.gls",
        "*.ist",
        "*.fls",
        "*.log",
        "*.fdb_latexmk"
    ],
    //设置为onFaild 在构建失败后清除辅助文件
    "latex-workshop.latex.autoClean.run": "onFailed",
    // 使用上次的recipe编译组合
    "latex-workshop.latex.recipe.default": "lastUsed",
    // 用于反向同步的内部查看器的键绑定。ctrl/cmd +点击(默认)或双击
    "latex-workshop.view.pdf.internal.synctex.keybinding": "double-click",



    //使用 SumatraPDF 预览编译好的PDF文件
    // 设置VScode内部查看生成的pdf文件
    "latex-workshop.view.pdf.viewer": "external",
    // PDF查看器用于在\ref上的[View on PDF]链接
    "latex-workshop.view.pdf.ref.viewer":"auto",
    // 使用外部查看器时要执行的命令。此功能不受官方支持。
    "latex-workshop.view.pdf.external.viewer.command": "F:/SumatraPDF/SumatraPDF.exe", // 注意修改路径
    // 使用外部查看器时，latex-workshop.view.pdf.external.view .command的参数。此功能不受官方支持。%PDF%是用于生成PDF文件的绝对路径的占位符。
    "latex-workshop.view.pdf.external.viewer.args": [
        "%PDF%"
    ],
    // 将synctex转发到外部查看器时要执行的命令。此功能不受官方支持。
    "latex-workshop.view.pdf.external.synctex.command": "F:/SumatraPDF/SumatraPDF.exe", // 注意修改路径
    // latex-workshop.view.pdf.external.synctex的参数。当同步到外部查看器时。%LINE%是行号，%PDF%是生成PDF文件的绝对路径的占位符，%TEX%是触发syncTeX的扩展名为.tex的LaTeX文件路径。
    "latex-workshop.view.pdf.external.synctex.args": [
        "-forward-search",
        "%TEX%",
        "%LINE%",
        "-reuse-instance",
        "-inverse-search",
        "\"F:/Microsoft VS Code/Code.exe\" \"F:/Microsoft VS Code/resources/app/out/cli.js\" -r -g \"%f:%l\"", // 注意修改路径
        "%PDF%"
    ]
}
```

