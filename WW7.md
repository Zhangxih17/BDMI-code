### 1. Numpy

#### （1）基本使用


```python
import numpy as np

c = np.array([[1,2,3],[4,5,6]])
print(c)

d = np.array([(1,2,3),(4,5,6)])
print(d)

g = np.array([['a','b'],['c','d']])
print(g)

e = np.array([(1,2,3),(4,5,6),(7,8,9)])
print(e)

```

    [[1 2 3]
     [4 5 6]]
    [[1 2 3]
     [4 5 6]]
    [['a' 'b']
     ['c' 'd']]
    




    array([[1, 2, 3],
           [4, 5, 6],
           [7, 8, 9]])



#### （2） 运算符


```python
A = np.arange(0,9).reshape(3,3)
A
```




    array([[0, 1, 2],
           [3, 4, 5],
           [6, 7, 8]])




```python
B = np.ones((3,3))
B
```




    array([[1., 1., 1.],
           [1., 1., 1.],
           [1., 1., 1.]])




```python
A*B   # 对应位置乘积
```




    array([[0., 1., 2.],
           [3., 4., 5.],
           [6., 7., 8.]])




```python
np.dot(A,B)   # 矩阵运算
```




    array([[ 3.,  3.,  3.],
           [12., 12., 12.],
           [21., 21., 21.]])



#### （3）增减算符


```python
a = np.arange(4)
a += 1
a
```




    array([1, 2, 3, 4])



#### （4）数组变形：reshape, ravel, transpose


```python
a = np.random.random(12)
a
```




    array([0.76460504, 0.38877321, 0.33526273, 0.24902417, 0.15008739,
           0.64305217, 0.41459186, 0.45056764, 0.39703228, 0.03973318,
           0.25702235, 0.91431916])




```python
A = a.reshape(3,4)
A
```




    array([[0.76460504, 0.38877321, 0.33526273, 0.24902417],
           [0.15008739, 0.64305217, 0.41459186, 0.45056764],
           [0.39703228, 0.03973318, 0.25702235, 0.91431916]])




```python
a.shape = (3,4)
a
```




    array([[0.76460504, 0.38877321, 0.33526273, 0.24902417],
           [0.15008739, 0.64305217, 0.41459186, 0.45056764],
           [0.39703228, 0.03973318, 0.25702235, 0.91431916]])




```python
a = a.ravel()   # 恢复长串数组
a
```




    array([0.76460504, 0.38877321, 0.33526273, 0.24902417, 0.15008739,
           0.64305217, 0.41459186, 0.45056764, 0.39703228, 0.03973318,
           0.25702235, 0.91431916])




```python
a.shape = (12)
a
```




    array([0.76460504, 0.38877321, 0.33526273, 0.24902417, 0.15008739,
           0.64305217, 0.41459186, 0.45056764, 0.39703228, 0.03973318,
           0.25702235, 0.91431916])




```python
A.transpose()
```




    array([[0.76460504, 0.15008739, 0.39703228],
           [0.38877321, 0.64305217, 0.03973318],
           [0.33526273, 0.41459186, 0.25702235],
           [0.24902417, 0.45056764, 0.91431916]])




```python
# 练习 sigmoid函数
def sigmoid(x):
    return 1/(1+np.exp(-x))

# tanh函数
def tanh(x):
    return (np.exp(x)-np.exp(-x))/(np.exp(x)+np.exp(-x))

print(sigmoid(0))
print(tanh(0))
```

    0.5
    0.0
    


```python
# ReLU函数
def ReLU(x):
    return np.maximum(x,0)
```

### 2. SciPy

### 3. 神经网络  

#### （1）ReLU单元


```python
def ReLU_unit(x):
    w = np.array([0.21,0.7,0.3])
    return np.dot(w,ReLU(x))
```

#### （2）logistic单元


```python
def logistic_unit(x):
    w = np.array([0.21,0.7,0.3])
    return np.dot(w,sigmoid(x))
```

#### （3）与门


```python
def AND_door(x):
    w = np.array([20,20])
    y = np.dot(w,x)-30
    return round(sigmoid(y))
```


```python
a = np.array([1,1])
print(AND_door(a))
```

    1.0
    


```python
def OR_door(x):
    w = np.array([20,20])
    y = np.dot(w,x)-10
    return round(sigmoid(y))
```


```python
def NAND_door(x):
    w = np.array([20,20])
    y = 30-np.dot(w,x)
    return round(sigmoid(y))
```


```python

```

#### （4）softmax


```python
def softmax(x):
    s = sum(np.exp(x))
    return [np.exp(i)/s for i in x]
```


```python
print(softmax(np.array([1,2,3])))
```

    [0.09003057317038046, 0.24472847105479767, 0.6652409557748219]
    


```python
def logit(x):
    return np.log(x/(1-x))
```


```python

```
