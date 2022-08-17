---
title: "Pytorch"
date: 2022-08-16
tags: ["code"]
---

## **argparse**

[Tutorial](https://docs.python.org/3/library/argparse.html)

`argparse`module makes it easy to write user-friendly command-line interfaces. The program defines what arguments it requires, and [`argparse`](https://docs.python.org/3/library/argparse.html#module-argparse) will figure out how to parse those out of [`sys.argv`](https://docs.python.org/3/library/sys.html#sys.argv). The [`argparse`](https://docs.python.org/3/library/argparse.html#module-argparse) module also automatically generates help and usage messages and issues errors when users give the program invalid arguments.

把他看作一个字典好了

## **@property & @abstractmethod**: 

- @property: 通过这个可以把一个函数当作属性来使用

- @abstractmethod: 来自ABCMeta库，描述如下

> This module provides the infrastructure for defining [abstract base classes](https://docs.python.org/3/glossary.html#term-abstract-base-class) (ABCs) in Python

好像就是说被这个装饰的函数不能实例化，但是其子类如果实现了该抽象方法的话就可以被实例化

## np.transpose()

类似torch.permute

## torch.stack()  

torch.stack(inputs, dim=?) → Tensor

沿着一个新维度对输入张量序列进行连接。 序列中所有的张量都应该为相同形状。

```python
# 例子
# 假设是时间步T1的输出
T1 = torch.tensor([[1, 2, 3],
        		[4, 5, 6],
        		[7, 8, 9]])
# 假设是时间步T2的输出
T2 = torch.tensor([[10, 20, 30],
        		[40, 50, 60],
        		[70, 80, 90]])
print(torch.stack((T1,T2),dim=0).shape)
print(torch.stack((T1,T2),dim=1).shape)
print(torch.stack((T1,T2),dim=2).shape)
print(torch.stack((T1,T2),dim=3).shape)
# outputs:
torch.Size([2, 3, 3])
torch.Size([3, 2, 3])
torch.Size([3, 3, 2])
IndexError: Dimension out of range (expected to be in range of [-3, 2], but got 3) # 报错
```

## torch.autograd.Variable

tensor是硬币的话，那Variable就是钱包，它记录着里面的钱的多少，和钱的流向

Tensor是存在Variable中的.data里的，而cpu和gpu的数据是通过 .cpu()和.cuda()来转换的

## torch.nn.functional.interpolate

```python
torch.nn.functional.interpolate(input, size=None, scale_factor=None, mode='nearest', align_corners=None)
```

根据给定的size或者scale_factor参数来对输入进行上/下采样，参数：

- input--tensor -- 输入张量
- size --int or Tuple -- 输出大小
- scale_factor --float or Tuple--指定输出为输入的多少倍数。如果输入为tuple，其也要制定为tuple类型
- mode --str-- 可使用的上采样算法，有`'nearest'`, `'linear'`, `'bilinear'`, `'bicubic'` , `'trilinear'和'area'`. `默认使用``'nearest'`
- align_corners --bool-- 几何上，我们认为输入和输出的像素是正方形，而不是点。如果设置为True，则输入和输出张量由其角像素的中心点对齐，从而保留角像素处的值。如果设置为False，则输入和输出张量由它们的角像素的角点对齐，插值使用边界外值的边值填充;`当scale_factor保持不变时`，使该操作独立于输入大小。仅当使用的算法为`'linear'`, `'bilinear', 'bilinear'`or `'trilinear'时可以使用。`默认设置为``False`   

## torch.save

> torch.save(*obj*, *f*, *pickle_module=pickle*, *pickle_protocol=DEFAULT_PROTOCOL*, *_use_new_zipfile_serialization=True*)

[Saves an object to a disk file.](https://pytorch.org/docs/stable/notes/serialization.html#saving-loading-tensors)

```python
>>> t = torch.tensor([1., 2.])
>>> torch.save(t, 'tensor.pt')
>>> torch.load('tensor.pt')
tensor([1., 2.])
```

In PyTorch, a module’s state is frequently serialized using a ‘state dict.’ A module’s state dict contains all of its parameters and persistent buffers:

```python
>>> bn = torch.nn.BatchNorm1d(3, track_running_stats=True)
>>> list(bn.named_parameters())
[('weight', Parameter containing: tensor([1., 1., 1.], requires_grad=True)),
 ('bias', Parameter containing: tensor([0., 0., 0.], requires_grad=True))]

>>> list(bn.named_buffers())
[('running_mean', tensor([0., 0., 0.])),
 ('running_var', tensor([1., 1., 1.])),
 ('num_batches_tracked', tensor(0))]

>>> bn.state_dict()
OrderedDict([('weight', tensor([1., 1., 1.])),
             ('bias', tensor([0., 0., 0.])),
             ('running_mean', tensor([0., 0., 0.])),
             ('running_var', tensor([1., 1., 1.])),
             ('num_batches_tracked', tensor(0))])
```

In the tutorials, it is recommended to instead save only its state dict. Python modules have a function,`load_state_dict()`, to restore their states from a state dict.

```python
>>> torch.save(bn.state_dict(), 'bn.pt')
>>> bn_state_dict = torch.load('bn.pt')
>>> new_bn = torch.nn.BatchNorm1d(3, track_running_stats=True)
>>> new_bn.load_state_dict(bn_state_dict)
<All keys matched successfully>
```

## BN

Batch Normalization

**Internal Covariate Shift**

- 在深层网络训练的过程中，由于网络中参数变化而引起内部结点数据分布发生变化的这一过程被称作Internal Covariate Shift。

Batch Normalization具体做法

- 对当前层的第j个维度做规范化（有m个样本），使得每一层输入的每个特征的分布均值为0，方差为1
- 考虑到规范化后容易使得底层网络学习到的信息丢失，因此引入两个可学习的参数$\gamma$和$\beta$，对规范后的数据进行线性变换。

## tensor.contiguous

一些tensor操作（traspose，permute）和原tensor是共享内存的，不会改变底层数组的存储，但是如果要使用view方法的话，就要求对应tensor的数据占用内存是连续的。

> Tensor.contiguous(*memory_format=torch.contiguous_format*) -> Tensor

Returns a contiguous in memory tensor containing the same data as `self` tensor. If `self` tensor is already in the specified memory format, this function returns the `self` tensor.

如果想要改变形状并且直接改内存的话，就用reshape

## validation data sets

- 为了调整超参数

> 也就是说我们将数据划分训练集、验证集和测试集。在训练集上训练模型，在验证集上评估模型，一旦找到的最佳的参数，就在测试集上最后测试一次，测试集上的误差作为泛化误差的近似。关于验证集的划分可以参考测试集的划分，其实都是一样的，这里不再赘述。
>
> 吴恩达老师的视频中，如果当数据量不是很大的时候（万级别以下）的时候将训练集、验证集以及测试集划分为6：2：2；若是数据很大，可以将训练集、验证集、测试集比例调整为98：1：1；但是当可用的数据很少的情况下也可以使用一些高级的方法，比如留出方，K折交叉验证等。

引入k-fold交叉验证的模板

```python
data = load_data()
train, test = split(data)

hyper_parameters = set_hyper()
k = init_k_fold()

for hyper in hyper_parameters:
    metrics = []
    for index in range(k):
        fold_train, fold_valid = cv_split(index, k, train)
        # 用此时的超参数训练模型
        model = fit(fold_train, hyper)
        current_metric = evaluate(model, fold_valid)
        metrics.append(current_metric)
    avg_metric = np.mean(metrics)
    std_metric = np.std(metrics)
    # compare metrics among different hypers
    best_hyper = update_best()

# finally
model = fit(train, best_hyper)
metrics = evaluate(model, test)
# show your final metrics
```

## tensorboard

```python
from torch.utils.tensorboard import SummaryWriter
writer = SummaryWriter('logs/xxx')

# 打印模型
# model是你想打印的模型，inputs是forword时输入的参数，如果有多个就以元组形式输出，其中元素必须是tensor
writer.add_graph(model,inputs)

# 打印loss
for n_iter in train:
    writer.add_scalar("Loss/train", train_loss, n_iter)
```

## GPU加速

**torch.nn.DataParallel** 

> *CLASS*`torch.nn.DataParallel`(*module*, *device_ids=None*, *output_device=None*, *dim=0*)
>
> 
>
> 首先在前向过程中，你的输入数据会被划分成多个子部分（以下称为副本）送到不同的device中进行计算，而你的模型module是在每个device上进行复制一份，也就是说，输入的batch是会被平均分到每个device中去，但是你的模型module是要拷贝到每个devide中去的，每个模型module只需要处理每个副本即可，当然你要保证你的batch size大于你的gpu个数。然后在反向传播过程中，每个副本的梯度被累加到原始模块中。概括来说就是：DataParallel 会自动帮我们将数据切分 load 到相应 GPU，将模型复制到相应 GPU，进行正向传播计算梯度并汇总。

```python
device_ids = [0, 1]
net = torch.nn.DataParallel(net, device_ids=device_ids)
```

实际上optimizer也可以使用dataparallel优化，此时需要注意到下面第二行返回的是一个module，因此optimizer需要当module使用

```python
optimizer = torch.optim.SGD(net.parameters(), lr=lr)
optimizer = nn.DataParallel(optimizer, device_ids=device_ids)
# 优化器原本使用
optimizer.step()
# 修改之后优化器使用
optimizer.module.step()
```

实际上pytorch不建议用DataParallel。。。



**torch.nn.parallel.DistributedDataParallel**

相比DataParallel

- `DataParallel`是单进程多线程的，只用于单机情况，而`DistributedDataParallel`是多进程的，适用于单机和多机情况，真正实现分布式训练；
- `DistributedDataParallel`的训练更高效，因为每个进程都是独立的Python解释器，避免GIL问题，而且通信成本低其训练速度更快，基本上`DataParallel`已经被弃用；
- `DistributedDataParallel`中每个进程都有独立的优化器，执行自己的更新过程，但是梯度通过通信传递到每个进程，所有执行的内容是相同的；

https://pytorch.org/tutorials/intermediate/ddp_tutorial.html

https://zhuanlan.zhihu.com/p/113694038

https://pytorch.org/tutorials/beginner/dist_overview.html#torch-nn-parallel-distributeddataparallel

## benchmark 

benchmark in osad

- IoU: 交并比。  
  - $IoU = \frac{traget \cap prediction}{target \cup prediction}$
- CC: Pearson product-moment correlation coefficient,皮尔逊相关系数。
  - $\rho_{X,Y}=\frac{cov(X，Y)}{\sigma_X\sigma_Y}$
- MAE：Mean Absolute Error，平均绝对误差

## torch.cuda.synchronize()

因为torch里面执行都是异步的，这行代码的意思是等待所有进程运行完。

```python
# 例子
torch.cuda.synchronize()
start = time.time()
result = model(input)
torch.cuda.synchronize()
end = time.time()
```

## torch.bmm

> torch.bmm(*input*, *mat2*, *, *out=None*) → Tensor

If `input` is a $(b \times n \times m)$ tensor, `mat2` is a $(b \times m \times p)$ tensor, `out` will be a$(b \times n \times p)$ tensor.

input and mat2 must be 3-D tensors each containing the same number of matrics.

```python
>>> input = torch.randn(10, 3, 4)
>>> mat2 = torch.randn(10, 4, 5)
>>> res = torch.bmm(input, mat2)
>>> res.size()
torch.Size([10, 3, 5])
```

