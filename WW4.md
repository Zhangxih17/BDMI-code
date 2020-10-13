## 1. Quick Sort


```python
import random

def QuickSort(A):
    if len(A) <= 1:
        return A
    L = []
    R = []
    p = random.choice(range(len(A)))
    E = [A[p]]  #pivot
    for i in range(len(A)):
        if i==p:
            continue
        if A[i]<A[p]:
            L.append(A[i])
        else:
            R.append(A[i])
    return QuickSort(L)+E+QuickSort(R)

temp = [6,4,2,9,3,8,5,1,7]
QuickSort(temp)
```




    [1, 2, 3, 4, 5, 6, 7, 8, 9]



## 2. Bucket and Radix Sort  
- Bucket: need more assumptions, faster but more space  


```python
def bucket_sort(A, min_value, max_value):
    buckets = [[] for _ in range(min_value, max_value+1)]
    for x in A:
        # implement it by yourself (将数放到对应的桶里)
        buckets[x].append(x)
    sorted_arr = []
    for bucket in buckets:
        # implement it by yourself (按顺序合并所有的桶)
        for k in bucket:
            sorted_arr.append(k)
    return sorted_arr


arr = [5,1,2,7,3,9,4,0,6,8]
sorted_arr = bucket_sort(arr, 0, 9)
print(sorted_arr)
```

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    

- Radix: digit d, base r=n


```python
def getDigits(x, base):
    digits = []
    while x > 0:
        digits.append( x % base )
        x = (x / base).__trunc__()
    return digits
        

class myInt:
    def __init__(self,x, base=10, keyDigit=0):
        self.digits = getDigits(x, base)
        self.keyDigit = keyDigit
        self.value = x
        
    def key(self):
        if len(self.digits) > self.keyDigit:
            return self.digits[self.keyDigit]
        return 0
    
    def updateKeyDigit(self,p):
        self.keyDigit = p
        
    def getValue(self):
        return self.value


def bucketSortBase(A, base):
    buckets = [[] for _ in range(base)]
    for x in A:
        # implement it by yourself (根据指定位置放进对应的桶里)
        buckets[x.key()].append(x)
    sorted_arr = []
    for bucket in buckets:
        # implement it by yourself (按照顺序合并所有的桶)
        for k in bucket:
            sorted_arr.append(k)
    return sorted_arr

def radix_sort(A, n_digits, base):
    B = [ myInt(x, base=base) for x in A ]
    for j in range(n_digits): #迭代位数
        for x in B:
            # implement it by yourself (指定比较的位置)
            x.updateKeyDigit(j)
        # implement it by yourself (桶排序)
        B = bucketSortBase(B, base)
    B = [x.getValue() for x in B]
    return B


arr = [523,123,4,33,12]
sorted_arr = radix_sort(arr, 3, 10)
print(sorted_arr)
```

    [4, 12, 33, 123, 523]
    

## 3. Linked list


```python
class node:
    def __init__(self,key=None):
        self.key=key
        self.next=None
```


```python
class linked_list:
    def __init__(self):
        self.head=node()

    # Adds new node containing 'key' to the end of the linked list.
    def append(self,key):
        new_node=node(key)
        cur=self.head
        while cur.next!=None:
            cur=cur.next
        cur.next=new_node

    # Returns the length (integer) of the linked list.
    def length(self):
        cur=self.head
        total=0
        while cur.next!=None:
            total+=1
            cur=cur.next
        return total 
    
    # Prints out the linked list in traditional Python list format. 
    def display(self):
        elems=[]
        cur_node=self.head
        while cur_node.next!=None:
            cur_node=cur_node.next
            elems.append(cur_node.key)
        print(elems)

    # Returns the value of the node at 'index'. 
    def get(self,index):
        if index>=self.length() or index<0: # added 'index<0' post-video
            print("ERROR: 'Get' Index out of range!")
            return None
        cur_idx=0
        cur_node=self.head
        while True:
            cur_node=cur_node.next
            if cur_idx==index: return cur_node.key
            cur_idx+=1
    
    # Deletes the node at index 'index'.
    def erase(self,index):
        if index>=self.length() or index<0: 
            print("ERROR: 'Erase' Index out of range!")
            return 
        cur_idx=0
        cur_node=self.head
        while True:
            last_node=cur_node
            cur_node=cur_node.next
            if cur_idx==index:
                last_node.next=cur_node.next
                return
            cur_idx+=1

    # Allows for bracket operator syntax (i.e. a[0] to return first item).
    def __getitem__(self,index):
        return self.get(index)
    
    
    #######################################################

    # Inserts a new node at index 'index' containing key 'key'.
    # Indices begin at 0. If the provided index is greater than or 
    # equal to the length of the linked list the 'key' will be appended.
    def insert(self,index,key):
        if index>=self.length() or index<0:
            return self.append(key)
        cur_node=self.head
        prior_node=self.head
        cur_idx=0
        while True:
            cur_node=cur_node.next
            if cur_idx==index: 
                new_node=node(key)
                prior_node.next=new_node
                new_node.next=cur_node
                return
            prior_node=cur_node
            cur_idx+=1
    
    # Inserts the node 'node' at index 'index'. Indices begin at 0.
    # If the 'index' is greater than or equal to the length of the linked 
    # list the 'node' will be appended.
    def insert_node(self,index,node):
        if index<0:
            print("ERROR: 'Erase' Index cannot be negative!")
            return
        if index>=self.length(): # append the node
            cur_node=self.head
            while cur_node.next!=None:
                cur_node=cur_node.next
            cur_node.next=node
            return
        cur_node=self.head
        prior_node=self.head
        cur_idx=0
        while True:
            cur_node=cur_node.next
            if cur_idx==index: 
                prior_node.next=node
                return
            prior_node=cur_node
            cur_idx+=1
    
    # Sets the key at index 'index' equal to 'key'.
    # Indices begin at 0. If the 'index' is greater than or equal 
    # to the length of the linked list a warning will be printed 
    # to the user.
    def set(self,index,key):
        if index>=self.length() or index<0:
            print("ERROR: 'Set' Index out of range!")
            return
        cur_node=self.head
        cur_idx=0
        while True:
            cur_node=cur_node.next
            if cur_idx==index: 
                cur_node.key=key
                return
            cur_idx+=1
```


```python
class Node:
    def __init__(self,value,node = 0):
        self.value = value
        self.next = node


class LinkedList:
    def __init__(self,value=0, *args):
        self.lenth = 0
        # 创建表头head
        self.head = 0 if value == 0 else Node(value)
        # 如果初始化实例时传入多个参数，循环加入链表
        p = self.head
        for i in [*args]:
            node = Node(i)
            p.next = node
            p = p.next

    def printLinkedList(self):
        self.p = self.head
        while self.p:
            print(self.p.value)
            if not self.p.next:
                return self.p
            self.p = self.p.next

#在此添加代码append,insert
    def append(self,value):
        new_node=Node(value)
        cur=self.head
        while cur.next!=0:
            cur=cur.next
        cur.next=new_node

    def insert(self,value):
        #请在此添加代码，写代码时删除pass
        self.head = Node(value,self.head)
        
    
    def insert_anywhere(self,index,value):
        if self.head is None or index <= 0:
            self.head = Node(value,self.head)
        else:
            temp = self.head
            while index > 1 and temp.next != None:
                temp = temp.next
                index = index-1
            temp.next = Node(value,temp.next)
        self.lenth = self.lenth+1
    
    def searchIndex(self,value):
        temp = self.head
        index = 0
        while temp != None and value != temp.value:
            index = index+1
            temp = temp.next
        return index


    def delete(self,value):
        index = self.searchIndex(value)
        if index <=0 or self.head.next is None:
            removeItem = self.head.value
            self.head = self.head.next
        else:
            temp = self.head
            while index>1 and temp.next.next != None:
                temp = temp.next
                index = index-1
            removeItem = temp.next.value
            temp.next = temp.next.next
        self.lenth = self.lenth -1
```


```python
l = LinkedList(7,5,3,4,1,2,8)
l.printLinkedList()
```

    7
    5
    3
    4
    1
    2
    8
    




    <__main__.Node at 0x9454f0>




```python
l.insert(9)
l.printLinkedList()
```

    9
    7
    5
    3
    4
    1
    2
    8
    




    <__main__.Node at 0x9454f0>




```python
l.delete(4)
l.printLinkedList()
```

    9
    7
    5
    3
    1
    2
    8
    




    <__main__.Node at 0x9454f0>



## 4. 二元查找树BST


```python
# 节点类
class Node:
    # 用类成员函数进行节点初始化
    def __init__(self, value):
        self.value = value
        self.lchild = None
        self.rchild = None

# BST树类
class BST:
    # 用类成员函数进行BST初始化
    def __init__(self, node_list):
        self.root = Node(node_list[0])
        for value in node_list[1:]:
            self.insert(value)
    # 搜索拥有某值的节点操作
    def search(self, node, parent, value):
        if node is None:
            return False, node, parent
        if node.value == value:
            return True, node, parent
        # 小的在左孩子，大于等于的在右孩子
        if node.value > value:
            return self.search(node.lchild, node, value)
        else:
            return self.search(node.rchild, node, value)
    
    # 插入某值的节点操作
    def insert(self, value):
        flag, n, p = self.search(self.root, self.root, value)
        if not flag:
            new_node = Node(value)
            if value > p.value:
                p.rchild = new_node
            else:
                p.lchild = new_node
    
    # 删除某值的节点
    def delete(self, value):
        flag, n, p = self.search(self.root, self.root, value)
        if flag is False:
            print("Can't find the key! Delete failed!")
        else:
            #当左子树为空时
            if n.lchild is None:
                if n == p.lchild:
                    p.lchild = n.rchild
                else:
                    p.rchild = n.rchild
                    
            #当右子树为空时
            elif n.rchild is None:
                if n == p.lchild:
                    p.lchild = n.lchild
                else:
                    p.rchild = n.lchild
            else:
                #当左右都不为空时，选择右子树
                pre = n.rchild
                if pre.lchild is None:
                    #如果左子树为空，直接将右子树上移
                    n.value = pre.value
                    n.rchild = pre.rchild
                else:
                    #如果左子树不为空，直接迭代到左子树根节点
                    next = pre.lchild
                    while next.lchild is not None:
                        #迭代，在这里写代码，写代码时候删除pass
                        pass
                    n.value = next.value
                    pre.lchild = next.rchild
    
    # 先序遍历
    def pre_order_traverse(self, node):
        if node is not None:
            print(node.value)
            self.pre_order_traverse(node.lchild)
            self.pre_order_traverse(node.rchild)

    # 中序遍历
    def in_order_traverse(self, node):
        if node is not None:
            self.in_order_traverse(node.lchild)
            print(node.value)
            self.in_order_traverse(node.rchild)

    # 后序遍历
    def post_order_traverse(self, node):
        if node is not None:
            self.post_order_traverse(node.lchild)
            self.post_order_traverse(node.rchild)
            print(node.value)
```


```python
array = [7,5,3,4,1,2,8]
bst = BST(array)

bst.pre_order_traverse(bst.root)
```

    7
    5
    3
    1
    2
    4
    8
    


```python
 bst.in_order_traverse(bst.root)
```

    1
    2
    3
    4
    5
    7
    8
    


```python
bst.post_order_traverse(bst.root)
```

    2
    1
    4
    3
    5
    8
    7
    


```python

```
