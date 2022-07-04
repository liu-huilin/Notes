# PyTorch笔记

## 基础

#### 1、常用数据类型

|              dtype              |     CPU Tensor     |       GPU Tensor        |
| :-----------------------------: | :----------------: | :---------------------: |
| torch.float32  or  torch.float  | torch.FloatTensor  | torch.cuda.FloatTensor  |
| torch.float32  or  torch.double | torch.DoubleTensor | torch.cuda.DoubleTensor |
|   torch.int32  or  torch.int    |  torch.IntTensor   |  torch.cuda.IntTensor   |

#### 2、模型的保存与提取

```python
torch.save(net, 'model.pkl')  #保存模型
torch.save(net.state_dict(), 'model_params.pkl')  #仅保存模型参数，更快

net1 = torch.load('model.pkl')  #读取模型

#读取模型参数，加载到net2，net2是事先定义好的和net结构一样的神经网络
net2.load_state_dict(torch.load('model_params.pkl'))  
```

#### 3、创建Tensor

```python
#设置Tensor默认类型
torch.set_default_tensor_type(torch.DoubleTensor)
```

#### 4、Tensor的合并与拆分

```
#合并
torch.cat() #在指定维度上合并
torch.stack() #在新维度上合并
#拆分
torch.split()
torch.chunk()

```

**对于高维的Tensor（dim>2），定义其矩阵乘法仅在最后的两个维度上，要求前面的维度必须保持一致，就像矩阵的索引一样并且运算操作符只有torch.matmul()。**
$$
\large
S(y_i)=\frac{e^{y_i}}{\sum_{j}{e^{y_i}}}
$$

#### 5、Dataset与DataLoader

Dataset储存数据及标签

DataLoader将Dataset包装成可迭代的对象



## 技巧

#### 显存分析

```
torch.cuda.memory_allocated() # 返回当前进程中torch.Tensor所占用的GPU显存
torch.cuda.max_memory_allocated() # 返回到调用函数为止所达到的最大的显存占用字节数
```

##### 显存占用

- 模型：显存占用量约为参数量乘以4
- 前向传播：显存增加等于每一层模型产生的结果的显存之和，且跟batch_size成正比
- 

参考

PyTorch显存机制分析：https://zhuanlan.zhihu.com/p/424512257



#### apex加速

项目地址：https://github.com/NVIDIA/apex

> 如果linux下安装报错，用命令： python setup.py install

https://chenyue.top/2019/05/21/%E5%B7%A5%E7%A8%8B-%E4%BA%94-apex%E6%B7%B7%E5%90%88%E7%B2%BE%E5%BA%A6%E5%8A%A0%E9%80%9F/



## 三方库

### Wandb

用于自动记录模型训练过程中的超参数和输出指标，然后可视化和比较结果

```
wandb.init — 在训练脚本开头初始化一个新的运行项；

wandb.config — 跟踪超参数；

wandb.log — 在训练循环中持续记录变化的指标；

wandb.save — 保存运行项相关文件，如模型权值；

wandb.restore — 运行指定运行项时，恢复代码状态。
```

