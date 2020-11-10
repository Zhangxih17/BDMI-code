### 1. Tensorflow


#### （1）安装


```python
import tensorflow as tf
```


```python
print(tf.__version__)
```

    2.3.1
    

#### （2）创建


```python
# This will be an int32 tensor by default; see "dtypes" below.
rank_0_tensor = tf.constant(4)
print(rank_0_tensor)
```

    tf.Tensor(4, shape=(), dtype=int32)
    


```python
# Let's make this a float tensor.
rank_1_tensor = tf.constant([2.0, 3.0, 4.0])
print(rank_1_tensor)
```

    tf.Tensor([2. 3. 4.], shape=(3,), dtype=float32)
    


```python
# If we want to be specific, we can set the dtype (see below) at creation time
rank_2_tensor = tf.constant([[1, 2],
                             [3, 4],
                             [5, 6]], dtype=tf.float16)
print(rank_2_tensor)
```

    tf.Tensor(
    [[1. 2.]
     [3. 4.]
     [5. 6.]], shape=(3, 2), dtype=float16)
    


```python
# There can be an arbitrary number of
# axes (sometimes called "dimensions")
rank_3_tensor = tf.constant([
  [[0, 1, 2, 3, 4],
   [5, 6, 7, 8, 9]],
  [[10, 11, 12, 13, 14],
   [15, 16, 17, 18, 19]],
  [[20, 21, 22, 23, 24],
   [25, 26, 27, 28, 29]],])
                    
print(rank_3_tensor)
```

    tf.Tensor(
    [[[ 0  1  2  3  4]
      [ 5  6  7  8  9]]
    
     [[10 11 12 13 14]
      [15 16 17 18 19]]
    
     [[20 21 22 23 24]
      [25 26 27 28 29]]], shape=(3, 2, 5), dtype=int32)
    

#### （3）运算


```python
a = tf.constant([[1, 2],
                 [3, 4]])
b = tf.constant([[1, 1],
                 [1, 1]]) # Could have also said `tf.ones([2,2])`

print(tf.add(a, b), "\n")
print(tf.multiply(a, b), "\n")
print(tf.matmul(a, b), "\n")
```

    tf.Tensor(
    [[2 3]
     [4 5]], shape=(2, 2), dtype=int32) 
    
    tf.Tensor(
    [[1 2]
     [3 4]], shape=(2, 2), dtype=int32) 
    
    tf.Tensor(
    [[3 3]
     [7 7]], shape=(2, 2), dtype=int32) 
    
    


```python
print(a + b, "\n") # element-wise addition
print(a * b, "\n") # element-wise multiplication
print(a @ b, "\n") # matrix multiplication
```

    tf.Tensor(
    [[2 3]
     [4 5]], shape=(2, 2), dtype=int32) 
    
    tf.Tensor(
    [[1 2]
     [3 4]], shape=(2, 2), dtype=int32) 
    
    tf.Tensor(
    [[3 3]
     [7 7]], shape=(2, 2), dtype=int32) 
    
    

### 2. 张量

#### （1）形状


```python
rank_4_tensor = tf.zeros([3, 2, 4, 5])
```


```python
print("Type of every element:", rank_4_tensor.dtype)
print("Number of dimensions:", rank_4_tensor.ndim)
print("Shape of tensor:", rank_4_tensor.shape)
print("Elements along axis 0 of tensor:", rank_4_tensor.shape[0])
print("Elements along the last axis of tensor:", rank_4_tensor.shape[-1])
print("Total number of elements (3*2*4*5): ", tf.size(rank_4_tensor).numpy())
```

    Type of every element: <dtype: 'float32'>
    Number of dimensions: 4
    Shape of tensor: (3, 2, 4, 5)
    Elements along axis 0 of tensor: 3
    Elements along the last axis of tensor: 5
    Total number of elements (3*2*4*5):  120
    

#### （2）索引


```python
rank_1_tensor = tf.constant([0, 1, 1, 2, 3, 5, 8, 13, 21, 34])
print(rank_1_tensor.numpy())
```

    [ 0  1  1  2  3  5  8 13 21 34]
    


```python
print("First:", rank_1_tensor[0].numpy())
print("Second:", rank_1_tensor[1].numpy())
print("Last:", rank_1_tensor[-1].numpy())
```

    First: 0
    Second: 1
    Last: 34
    


```python
print("Everything:", rank_1_tensor[:].numpy())
print("Before 4:", rank_1_tensor[:4].numpy())
print("From 4 to the end:", rank_1_tensor[4:].numpy())
print("From 2, before 7:", rank_1_tensor[2:7].numpy())  # 区间左闭右开
print("Every other item:", rank_1_tensor[::2].numpy())
print("Reversed:", rank_1_tensor[::-1].numpy())
```

    Everything: [ 0  1  1  2  3  5  8 13 21 34]
    Before 4: [0 1 1 2]
    From 4 to the end: [ 3  5  8 13 21 34]
    From 2, before 7: [1 2 3 5 8]
    Every other item: [ 0  1  3  8 21]
    Reversed: [34 21 13  8  5  3  2  1  1  0]
    

### 3. 变量

#### （1）创建


```python
my_tensor = tf.constant([[1.0, 2.0], [3.0, 4.0]])
my_variable = tf.Variable(my_tensor)  # 需给定初始值才能创建变量

# Variables can be all kinds of types, just like tensors，数据类型不限
bool_variable = tf.Variable([False, False, False, True])
complex_variable = tf.Variable([5 + 4j, 6 + 1j])
```


```python
print("Shape: ",my_variable.shape)
print("DType: ",my_variable.dtype)
print("As NumPy: ", my_variable.numpy)
```

    Shape:  (2, 2)
    DType:  <dtype: 'float32'>
    As NumPy:  <bound method BaseResourceVariable.numpy of <tf.Variable 'Variable:0' shape=(2, 2) dtype=float32, numpy=
    array([[1., 2.],
           [3., 4.]], dtype=float32)>>
    


```python
print("A variable:",my_variable)
print("\nViewed as a tensor:", tf.convert_to_tensor(my_variable))
print("\nIndex of highest value:", tf.argmax(my_variable))

# This creates a new tensor; it does not reshape the variable.
print("\nCopying and reshaping: ", tf.reshape(my_variable, ([1,4])))
```

    A variable: <tf.Variable 'Variable:0' shape=(2, 2) dtype=float32, numpy=
    array([[1., 2.],
           [3., 4.]], dtype=float32)>
    
    Viewed as a tensor: tf.Tensor(
    [[1. 2.]
     [3. 4.]], shape=(2, 2), dtype=float32)
    
    Index of highest value: tf.Tensor([1 1], shape=(2,), dtype=int64)
    
    Copying and reshaping:  tf.Tensor([[1. 2. 3. 4.]], shape=(1, 4), dtype=float32)
    


```python
print("\nIndex of highest value:", tf.argmax(my_variable, 0))
print("\nIndex of highest value:", tf.argmax(my_variable, 1))
```

    
    Index of highest value: tf.Tensor([1 1], shape=(2,), dtype=int64)
    
    Index of highest value: tf.Tensor([1 1], shape=(2,), dtype=int64)
    

变量一旦创建形状无法更改，因此可重新创建


```python
a = tf.Variable([2.0, 3.0])
# Create b based on the value of a
b = tf.Variable(a)
a.assign([5, 6])

# a and b are different
print(a.numpy())
print(b.numpy())

# There are other versions of assign
print(a.assign_add([2,3]).numpy())  # [7. 9.]
print(a.assign_sub([7,9]).numpy())  # [0. 0.]
```

    [5. 6.]
    [2. 3.]
    [7. 9.]
    [0. 0.]
    


```python
# 关闭梯度
step_counter = tf.Variable(1, trainable=False)
```

### 3. 自动微分

#### （1）函数求值


```python
x = tf.constant(1.0)
with tf.GradientTape(persistent=True) as t:
    t.watch(x)
    y = tf.multiply(x, x) + tf.exp(x)
dy_dx = t.gradient(y, x)  
```


```python
print(dy_dx.numpy())
```

    4.7182817
    

#### （2）即刻执行


```python
import os

import tensorflow as tf

import cProfile
```


```python
x = [[2.]]
m = tf.matmul(x, x)
print("hello, {}".format(m))
```

    hello, [[4.]]
    


```python
def fizzbuzz(max_num):
  counter = tf.constant(0)
  max_num = tf.convert_to_tensor(max_num)
  for num in range(1, max_num.numpy()+1):
    num = tf.constant(num)
    if int(num % 3) == 0 and int(num % 5) == 0:
      print('FizzBuzz')
    elif int(num % 3) == 0:
      print('Fizz')
    elif int(num % 5) == 0:
      print('Buzz')
    else:
      print(num.numpy())
    counter += 1
```


```python
fizzbuzz(15)
```

    1
    2
    Fizz
    4
    Buzz
    Fizz
    7
    8
    Fizz
    Buzz
    11
    Fizz
    13
    14
    FizzBuzz
    


```python
w = tf.Variable([[1.0]])
with tf.GradientTape() as tape:
  loss = w * w

grad = tape.gradient(loss, w)
print(grad)  # => tf.Tensor([[ 2.]], shape=(1, 1), dtype=float32)
```

    tf.Tensor([[2.]], shape=(1, 1), dtype=float32)
    


```python

```
