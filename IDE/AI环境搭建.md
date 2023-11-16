## UNIX学习推荐：

[Unix / Linux - What is Shells? (tutorialspoint.com)](https://www.tutorialspoint.com/unix/unix-what-is-shell.htm)

## 拥抱zsh

github开源

```bash
# 安装zsh
sudo apt-get install -y zsh
# 安装oh-my-zshell
sh -c "$(curl -fsSL https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh)"
#安装auto-suggestion
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
# 编辑.zshrc
vim ~/.zshrc #把conda和cuda的一些指令粘贴过来
```

清华源：[AOSP | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/#:~:text=AOSP 使用帮助 | 镜像站使用帮助 |,清华大学开源软件镜像站，致力于为国内和校内用户提供高质量的开源软件镜像、Linux 镜像源服务，帮助用户更方便地获取开源软件。 本镜像站由清华大学 TUNA 协会负责运行维护。)

## GPU驱动安装

```bash
Method1：PPA --NO!!!!!
Method2:推荐。CUDA Toolkit => 官方文档 cd  ****
S1: Install repository meta-data
S2: Installing the CUDA public GPG key
S3: Update the Apt repository cache
S4: Install Driver
    apt-cache search nvidia-driver
    apt-get install nvidia-driver-435
S5: reboot => 验证 nvidia-smi
S6: Install CUDA => 验证
	apt-get install cuda
	不要一直Yes,有个地方需要NO，选不安装驱动
S7: 修改zsh
    export PATH=/usr/local/cuda-9.2/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda-9.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    export CUDA_HOME=/usr/local/cuda-9.2
S8: 如何卸载安装
	apt-get --purge autoremove nvidia*
	apt list --installed|grep cuda
	apt purge cuda-repo-ubuntu1804
	/usr/local/cuda-9.2/bin/cuda-uninstaller
	rm -f /usr/local/cuda-9.2
Method3: ubuntu-drivers

# 安装cudnn[Installation Guide - NVIDIA Docs](https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html#installlinux-deb)
sudo cp cuda/include/cudnn.h /home/stephan/general_software/cuda_toolkit/include
sudo cp cuda/lib64/libcudnn* /home/stephan/general_software/cuda_toolkit/lib64
sudo chmod a+r /home/stephan/general_software/cuda_toolkit/include/cudnn.h 
sudo chmod a+r /home/stephan/general_software/cuda_toolkit/lib64/libcudnn*

```

[手把手教会你在Linux服务器上安装用户级别的CUDA_长度: 2645419389 (2.5g) [application/octet-stream\] c-CSDN博客](https://blog.csdn.net/qq_35082030/article/details/110387800)

## Anacoda的安装

```bash
#创建虚拟环境
conda create -n your_env_name python=X.X（3.6、3.7等）
 
#激活虚拟环境
source activate your_env_name(虚拟环境名称)
 
#退出虚拟环境
source deactivate your_env_name(虚拟环境名称)
 
#删除虚拟环境
conda remove -n your_env_name(虚拟环境名称) --all
 
#查看安装了哪些包
conda list
 
#安装包
conda install package_name(包名)
conda install scrapy==1.3 # 安装指定版本的包
conda install -n 环境名 包名 # 在conda指定的某个环境中安装包
 
#查看当前存在哪些虚拟环境
conda env list 
#或 
conda info -e
#或
conda info --envs
 
#检查更新当前conda
conda update conda
 
#更新anaconda
conda update anaconda
 
#更新所有库
conda update --all
 
#更新python
conda update python
#关闭 base环境
conda config --set auto_activate_base false
```

为conda环境创建内核

```bash
conda create -n my-conda-env    # creates new virtual env
source activate my-conda-env     # activate environment in terminal
conda install ipykernel      # install Python kernel in new conda env
ipython kernel install --user --name=my-conda-env-kernel  # configure Jupyter to use Python kernel,--name换成conda虚拟环境的名字，--user指令不用修改
jupyter notebook      # run jupyter from system
```

## Environment的使用

```bash
conda create --name  python3.6_torch1.5_cuda10.1 python=3.6
conda activate  python3.6_torch1.5_cuda10.1 python=3.6
pip install numpy
# pip设置清华源
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
#pip取消设置源
pip config unset global.index-url
```

tensoflow要严格对应系统的GPU版本

[从源代码构建  | TensorFlow (google.cn)](https://tensorflow.google.cn/install/source?hl=zh-cn)

```bash
# 如何自由切换cuda版本,删除cuda的软链接，重新建立新版本cuda的链接
cd /usr/local
sudo rm -rf cuda
ln -s cuda-10.1 ./cuda
# cudnn的安装
# https:/developer.nvidia.com/rdp/cudnn-archive
cp include/cudnn.h /usr/local/cuda-9.0/include
cp -r lib64/libcudnn* /usr/local/cuda-9.0/lib64/
```

**Jupyter Lab**的使用

```bash
ssh framk@10.19.124.245 -p 2022
conda activate python3.6_torch1.5_cuda10.1

pip install jupyterlab
pip install ipykernel
python -m ipykernel install --user --name python3.6_torch1.5_cuda10.1 #将环境添加到jupyter,--user命令表示仅针对当前用户安装

jupyter lab --generate-config
jupyter lab password  #js96366
vim  /home/frank/.jupyter/jupyter_lab_config.py
	c.NotebookApp.ip='*' #允许任何IP连接
	c.NotebookApp.open_browser=False #不打开图形界面
	c.NotebookApp.port=8888 #初始端口，冲突自行替换
nohup jupyter lab --port=8891 --no-browser & 
cat nohup.out
ps -ef | grep jupyter-lab
kill -9 xxxx
```

## VSCode配置

- 简体中文 （Chinese simplified）
- Python 管理切换python环境
- Remote SSH 服务器连接
- Remote SSH：Editing Configuration Files

### 远程配置

step1:c](https://code.visualstudio.com/docs/remote/troubleshooting)

```bash
ssh-keygen -t rsa -b 4096 #生成mac/ubuntu的local public key
ssh-copy-id -p 2022 frank@10.19.124.245 #复制此key到远程主机
```



## Markdown 开发配置

markdown 有关插件

- Markdown All in one
- Markdownline
- Markdown Preview Mermaid Support

latex有关插件

- Latex
- LaTex workshop

## 软件安装：ssh

```bash
sudo apt-get install openssh-server
ssh user@remote -p port
scp -P port /path/to/local/file user@remote:/path/to/remote/file
```

## git学习

游戏：Learn Git Branching： https://learngitbranching.js.org/?localw=zh_CN
官方文档：https://git.scm.com/doc

## 常用Ubuntu命令

`watch -n1 nvidia-smi `# GPU实时查看GPU资源情况
`htop` # CPU