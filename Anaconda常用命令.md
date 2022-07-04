# Anaconda常用命令

### 0— Anaconda

```shell
conda  create  -n  TF2.1  python==3.7      #  创建一个名为TF2.1的python3.7环境

conda  remove  -n  TF2.1  --all   # 删除虚拟环境TF2.1

conda  activate  TF2.1       #   进入TF2.1环境

conda  install  cudatoolkit**=**x.x      #   安装英伟达SDK（有英伟达GPU才可以）

conda  install  cudnn              #    安装英伟达深度学习软件包（有英伟达GPU才可以）

conda info -e           #   查看所有conda环境

conda  deactivate   # 退出环境

conda create -n 新环境名 --clone 旧环境名  # 克隆已有环境
```



##### 镜像源的设置

conda config --show  channels 显示所有源

conda config --add https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/  添加源

conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/   移除源

conda config --set show_channel_urls yes的意思是从channel中安装包时显示channel的url，这样就可以知道包的安装来源了。



换回默认源

conda config --remove-key channels  

conda clean -i

#### 将conda创建的环境添加到Jupyter中

1、首先在该环境安装**ipykernel**，也可以在创建虚拟环境时直接安装，在命令最后添加 ipykernel即可

2、执行命令：python -m ipykernel install --user --name 环境名称 --display-name "要展示的环境名称"

#### 删除创建的环境

conda env remove -n 环境名

#### Jupyter

jupyter kernelspec list  查看环境

jupyter kernelspec uninstall myenv   删除环境myenv

jupyter notebook  mypath   在路径mypath打开jupyter





####  取消linux下每次启动自动激活base环境

conda config --set auto_activate_base false








