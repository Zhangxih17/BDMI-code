### 1. Pandas


```python
import numpy as np
import pandas as pd
```


```python
s = pd.Series(np.random.randn(5),index=['a','b','c','d','e'])
s
```




    a   -0.066821
    b    0.341982
    c    1.952593
    d   -0.638402
    e   -1.580250
    dtype: float64




```python
s+s
```




    a   -0.133642
    b    0.683964
    c    3.905185
    d   -1.276804
    e   -3.160501
    dtype: float64




```python
s[1:]+s[:-1]   # 错位相加减
```




    a         NaN
    b    0.683964
    c    3.905185
    d   -1.276804
    e         NaN
    dtype: float64




```python
d = {'ID': pd.Series([2017010308, 2017020308], index = ['zxh','hhh']),
    'Gender': pd.Series(['female', 'male'], index = ['zxh','hhh'])}
```


```python
df = pd.DataFrame(d)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>Gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>zxh</th>
      <td>2017010308</td>
      <td>female</td>
    </tr>
    <tr>
      <th>hhh</th>
      <td>2017020308</td>
      <td>male</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.loc['zxh']
```




    ID        2017010308
    Gender        female
    Name: zxh, dtype: object



### 2. 神经网络

(1) 损失函数


```python
def mse(x):
    return np.sum((x-np.mean(x))**2)/len(x)
```


```python
a = [72, 49, 92, 87]
mse(a)
```




    279.5



(2) 交叉熵


```python
def CE(x1, x2):
    return -np.sum(x1*np.log(x2))
```


```python
CE([1,0], [0.9,0.1])
```




    0.10536051565782628




```python
CE([0,1], [0.2,0.8])
```




    0.2231435513142097



(3) Logistic回归


```python
def log_likelihood(features, target, weights):
    scores = np.dot(features, weights)
    ll = np.sum( target*scores - np.log(1 + np.exp(scores)) )
    return ll
```

### 3. Tensorflow


```python

```
