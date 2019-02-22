### 直观事例
```python
#在迭代过程中修改迭代序列不安全，所以要修改序列时，
#要对它的副本做迭代，而不是原序列本身
#对原迭代直接使用insert会进入卡死状态！！（待验证)。原意是迭代过程中修改迭代序列不安全，
#可以使用切割标识修改其副本，但是这里直接就BAN了。

arr = [1, 2, 3, 4]
for elem in arr[:]: # 想要修改arr，则通过arr的副本arr[:]做迭代，没有[:]会直接卡死
    if elem == 3:
        arr.insert(0, elem)
print(arr)
 
''' 若要对原序列做修改，则可以通过下标的for循环实现，不要用迭代 '''
for idx in range(len(arr)):
    if arr[idx] == 3:
        arr[idx] = 5
print_elem(arr)
 
''' 想要同时得到索引和对应的值，可以使用enumerate '''
for idx, elem in enumerate(arr):
    print('(', idx, ',', elem, ')', sep="", end=" ")
print()
 
 
''' 想要同时迭代两个序列，用zip,迭代的次数是较短序列的长度  '''
arr1 = ["cx", "wb"]
arr2 = ["山东", "河北", "江西"]
for person, hometown in zip(arr1, arr2):
    print("{0}的家乡是{1}".format(person, hometown), end=",")
print()
 
'''循环一个序列按排序顺序，使用sorted()函数，set返回一个新的排序的列表(去除所有重复元素)，保留原列表不变。'''
for elem in sorted(set(arr2)):
    print(elem, end=",")
print()
# 原列表未改变
print_elem(arr2)
 
''' 要反向遍历一个序列，首先正向生成这个序列，然后调用 reversed() 函数。'''
for i in reversed(range(1, 10, 2)):
    print(i, end=',')
```
文字总结循环迭代技巧：
* 字典中循环使用`items()`, 可以同时读取出来。
* 序列`list`中循环时，可以使用`enumerate()`同时得到index和value
* 循环两个以上变量时，可以使用`zip()`整体打包，以较短的变量为循环长度
* 逆向循环可以正向定义`range()`然后使用`reversed()`函数
* `set`可以将序列中的每个不重复元素提取出来

### Range()
`Range()`函数会创建一个**可迭代对象**，其可以借助其他迭代器(for,list,enumerate)来表达出一个有序列表，但是如果直接返回/打印`Range()`对象，则看不到理想的生成的有序列表。
`range()`对象有两个内建函数：
* count(value) 返回value在range对象里面出现的次数，大部分都是1
* index(value) 返回value在range对象里面的位置

### 关键字 in
测定序列中是否包含某个确定的值
```python
if 'y' in ('y', 'ye', 'yes'):
    return True
```

### 

