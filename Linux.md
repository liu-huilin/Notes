### 挂起当前任务

##### 挂起

step1：screen

step2：运行任务

step3：ctrl+a+d    挂起任务，返回到shell

##### 恢复

step1：screen  -ls

step2：screen  -r  id

##### 杀死

screen  -S id -X quit





后台挂起程序，并输出到log.txt

```
nohup python -u main.py > ./log.txt 2>&1 &
```



##### 同时打印到终端和文件

```
python test.py | tee output.txt
```

主要是使用tee工具（https://zhuanlan.zhihu.com/p/34510815）

追加

```
python test.py | tee -a output.txt
```



### 文件操作

##### 查看文件数量

```
ls -l |grep -c "^-"  
```

##### 查看文件夹数量

```
ls -l |grep -c "^d"
```

##### 解压文件

```
tar -zxvf filename.tar.gz
```

##### 压缩文件

```
将/home/wwwroot/xahot/ 这个目录下所有文件和文件夹打包为当前目录下的xahot.zip

zip –q –r xahot.zip /home/wwwroot/xahot
```



##### 指定GPU运行程序

```
CUDA_VISIBLE_DEVICES=1 python main.py
```



### vim

```
y  复制
yy 复制一整行
p(小写) 在光标位置之后粘贴
P(大写) 在光标位置之前粘贴
0 快速至当前行的行头
$ 快速至当前行的行尾
dd 删除行
u 撤销
```

### GDB

```
# 1、编译时需要加入-g选项
g++ test.cpp -g

```

### g++

```
g++ test.cpp -o test.out
./test.out
```

### shell

#### 1、关于Shell

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

Linux 的 Shell 种类众多，常见的有：

- Bourne Shell（/usr/bin/sh或/bin/sh）
- **Bourne Again Shell（/bin/bash）**：大多数Linux 系统默认的 Shell，bash
- C Shell（/usr/bin/csh）
- K Shell（/usr/bin/ksh）
- Shell for Root（/sbin/sh）

**#!** 告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序，如#!/bin/bash.

#### 2、Shell基础

Shell脚本文件拓展名为**.sh**，脚本第一行可指定解释器

##### 运行 Shell 脚本有两种方法：

**1、作为可执行程序**

将上面的代码保存为 test.sh，并 cd 到相应目录：

```
chmod +x ./test.sh  #使脚本具有执行权限
./test.sh  #执行脚本
```

注意，一定要写成 **./test.sh**，而不是 **test.sh**，运行其它二进制的程序也一样，直接写 test.sh，linux 系统会去 PATH 里寻找有没有叫 test.sh 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 test.sh 是会找不到命令的，要用 ./test.sh 告诉系统说，就在当前目录找。

**2、作为解释器参数**

这种运行方式是，直接运行解释器，其参数就是 shell 脚本的文件名，如：

```
/bin/sh test.sh
/bin/php test.php
```

这种方式运行的脚本，不需要在第一行指定解释器信息，写了也没用。

**3、多行注释**

简单来说就是用一个空命令接收要注释的命令行，不做任何事情，以此来达到注释的目的。例如：

```
<< EOF
Cmd line 1
Cmd line 2
Cmd line 3
Cmd line 4
EOF
```

命令行在起始 << EOF 和随后临近的 第一个 EOF 之间的内容都会被注释掉。
