# 本地部署 SD 并提供局域网内计算机访问
## 1. 前提
学校项目要求绘制故事海报和角色设计，但我板绘画的跟小猫一样 QAQ

包括 SD 在线版，以及基于 SD 封装的网站比如 6pen art，免费版的速度感人，自由度也相对小于 SD

加上自己有 RTX4080 的算力，果断选择使用 Stable diffusion 

如果已经配置完 SD 环境，需要将 SD 开放给局域网计算机，直接 [跳转第 4 步](#4-将服务器开放到局域网)
## 2. 前期环境配置
根据版本在 [官网下载](https://docs.conda.io/projects/miniconda/en/latest/)
然后全部按照默认安装就行

安装完成后，打开 Anaconda Prompt (Miniconda3)。
低版本 Miniconda 名称可能为 Anaconda Prompt (Miniconda)

**1\. 然后输入并执行，用于输出版本号确认 conda 环境是否安装成功**

    conda -V

返回结果为：conda (版本号)

**2\.分别输入并执行:**

    conda config --set show_channel_urls yes
    conda clean -i
    
**3\. 配置 git**

参考我专门写的另一篇教程 [UseGitInCnUltimate](https://github.com/Sacilave/UseGitInCnUltimate/)

**4\. 配置 Python 环境**

    conda create --name sdwebui python=3.10.6

此步骤中如遇网络问题也可根据 第3步 配置

命令行中会提醒是否继续进行操作，输入 y 继续进行操作

**5\. 激活 Conda 环境**

    conda activate sdwebui

**6\. pip 升级并更改镜像为清华镜像，分别输入**

    python -m pip install --upgrade pip
    pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/

**7\. 安装 CUDA**

命令行输入

    nvidia-smi

在表格第一行第三个 CUDA Version: 后面便为需要下载的 CUDA 版本号

进入 [官网](https://developer.nvidia.com/cuda-toolkit-archive) ，点击 CUDA Toolkit 你的版本号，官网很慢需要魔法

可能出现网页刷新完了却什么都没有出现的情况，等待一段时间会出现 选择 operating system 选项

然后一路根据自己的系统选下去，直接 download 就行

然后找个空余容量大点的磁盘安装就行

## 3. 配置 SD 环境

**1. 选择 SD 所在位置**

在 Miniconda 中输入 盘符: 即可跳到对应磁盘，如下方举例使用 D 盘

    D:

使用 cd 进入想要 SD 所在的位置，比如 cd ./sdProj 为进入 当前文件夹下的 sdProj 文件夹内

输入以下指令从 github 上 clone SD 项目

    git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git

如果没报错的话，接下来应该会在当前文件目录下生成一个新文件夹 stable-diffusion-webui

**2. 下载SD模型**

[官网基础模型下载](https://huggingface.co/stabilityai/stable-diffusion-2-1/blob/main/v2-1_768-ema-pruned.ckpt)

将下载模型放入新文件夹中的 models\Stable-diffusion 文件夹内

更多模型访问 [Civitai](https://civitai.com/) 也放在上述文件夹内

**3. 部署的最后一步！**

接下来，进入项目文件夹，在 Miniconda 中输入

    cd stable-diffusion-webui

然后启动！

    webui-user.bat

接下来，命令行中应该会出现以下提示，那就说明成功部署啦！
**注意！！！千万不要关掉 Miniconda 窗口**
    
    running on local URL: http://127.0.0.1:7860

(不小心关掉了就重新按照步骤启动)

    conda activate sdwebui
    webui-user.bat

如果没有成功部署，可以先试下重新运行启动命令

还是不行先确认是否为网络问题，是否在下载时出现问题，报错信息看不懂可先通过翻译软件检查大概

若非网络问题，重试将 Miniconda 和 Python 环境 都安装在默认位置，卸载本机内 Python 版本高于 3.10 的版本

我自己遇到的报错 ↓↓↓  重新将环境安装在了默认位置解决

    stderr: ERROR: Ignored the following versions that require a different python version: 1.6.2 Requires-Python >=3.7,<3.10; 1.6.3 Requires-Python >=3.7,<3.10; 1.7.0 Requires-Python >=3.7,<3.10; 1.7.1 Requires-Python >=3.7,<3.10

    ERROR: Could not find a version that satisfies the requirement tb-nightly (from versions: none)

    ERROR: No matching distribution found for tb-nightly

    WARNING: There was an error checking the latest version of pip.

**4. 开始使用**

浏览器地址栏输入 http://127.0.0.1:7860 即可访问本地部署的 SD 了

左上角 Stable Diffusion checkpoint 下方的下拉框更改模型 ，更多具体 SD 使用教程可以上网搜咯

## 4. 将服务器开放到局域网

**1. 设置启动参数**

此时已经启动了本地 SD，在 Miniconda 窗口中 按下 Ctrl + C 停止运行

接下来需要把 SD 设置为可被同局域网环境下访问

找到项目所在文件夹，找到 webui.bat 文件，没错就是你启动的那个文件

右键，编辑，用记事本打开就行

在 第一行 @echo off 下方添加一行

    set COMMANDLINE_ARGS=--listen

Ctrl + S 保存文件，重新启动项目，直接输入

    webui-user.bat

**2. 获取本地IP**

按住 Windows徽标键 + R，输入 cmd，打开命令行窗口后，输入

    ipconfig

此时会出现很长一串，根据你局域网连接类型找到对应 IP

    如果连接网线，找到 **以太网适配器 以太网** 下方的 IPv4 地址，后面的一串数字就是 IP

    如果连接 WIFI，找到 **无线局域网适配器 WLAN** 下方的 IPv4 地址，后面的一串数字就是 IP

**3. 关闭防火墙**

打开 Windows安全中心，进入 防火墙和网络保护，将 Microsoft Defender 防火墙 关闭

**4. 访问 SD**

在局域网中另一电脑浏览器中输入 步骤2 中获取的 IP，IP后加上端口号

比如 IP地址 是 192.0.0.1，那就输入

    192.0.0.1:7860

此时就能访问啦

如果无法访问问题可能是，不在同一局域网下，防火墙阻止了连接

**5. 建立两台电脑远程连接访问 SD**

下载 Radmin VPN ，配置一个虚拟局域网环境即可

也可以考虑 公网IP 映射端口，没有公网 IP 也可以通过 Frp 实现，教程可以网上搜哦

