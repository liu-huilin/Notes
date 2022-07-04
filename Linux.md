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

