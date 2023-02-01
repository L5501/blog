---
title: numpy数据分析
description: numpy数据分析笔记
data: 2022-12-4 11:14:00
updata: 2022-12-4 11:14:00
tags: 
  - python
categories:
- python笔记
---
## 我的博客部署成功了

# numpy数据分析**

## numpy模块

### 数组存在优先级：

1. 字符串>浮点型>整数

### 数组常用方法：

1. zero()

2. ones()

3. linspace()

   ```python
   np.linspace(0,100,num=20)  # 返回从0大100的20个数的一维等差数列对应的数组
   ```

   ​

4. arange()

   ```python
   np.arange(10,50,step=2)  # 返回从10到50（不包括50）,步数为2的等差数列对应的一维数组
   ```

   ​

5. random系列

   ```python
   np.random.randint(0,100,size=(5,3)) # 返回0到100间的五行三列的二维随机数
   ```

### numpy常用属性

1. shape   

   ```python
   arr.shape  # 数组的形状，几行几列
   ```

2. ndim

   ```python
   arr.ndim  # 返回的是数组的维度
   ```

3. size

   ```python
   arr.size  # 返回数组元素的个数
   ```

4. dtype

   ```python
   arr.dtype # 返回是数组元素类型题
   ```

创建数组并制定元素类型

```python
arr = np.array([1,2,3],dtype='int64')
```

修改数组元素类型

```python
arr.dtype = 'uint8' # 修改数组的元素类型
```

