> 跑代码需要
>
> train文件+datasets类+配置文件+一个网络
>
> 数据格式：
>
> datasetroot/Testl/image
>
> datasetroot/Tes/label
>
> datasetroot/Train/image
>
> datasetroot/Train/label
>
> datasetroot/Val/image
>
> datasetroot/Val/label
>
> <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/51149297/1767870594008-53c18d89-f642-42f0-859b-993e1acedfd7.png)
>

# 代码架构
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/51149297/1767870934944-e03837ed-1cdd-4db0-ba4f-0c9ed982c4c7.png)

终端输入

```python
python train.py -c ./config/mass/MGCCNet/MGCCUNet.py 
```

./config/mass/MGCCNet/MGCCUNet.py ---> 对应的配置文件

## config文件夹
命名规范： config/数据集名字/网络名字/xxxx.py

## geoseg文件夹
### geoseg/datasets 文件夹


对应数据集怎么加载，写一个datasets类，这个类继承Dataset： 

```python
from torch.utils.data import Dataset
```

构造函数一般传入数据集的根目录，和image目录名字，label目录名字，以及文件名后缀等等, 比如datasetroot/test/image/           根目录就是datasetroot/， 



然后重写__getitem__方法，这个方法，就是在对应根据索引index取出datasetroot/test/image/下的第index张图片和datasetroot/test/label/下的第index张图片, 一般需要单独写个函数把对应文件的列表加载成一个数组，数组的元素值就是一个个文件的路径： [datasetroot/test/label/xxx2.png, datasetroot/test/label/xxx2.png]

```python
def __getitem__(self, index)
```

返回经过处理后的图片的数据，也就是通过index得到对应的image和label路径，通过路径读取数据，处理image进行归一化等，处理label把颜色映射到0~numclass-1

## geoseg/losses
损失函数，参考jz_useful_loss.py写法

## geoseg/models
网络模型



