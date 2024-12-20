### heapq

Python的**heapq**模块提供了多种操作堆的函数，包括：

- `heappush(heap, item)`: 将值`item`推入堆`heap`中，同时保持堆的不变性。
- `heappop(heap)`: 弹出并返回堆`heap`中的最小项，同时保持堆的不变性。如果堆为空，则抛出`IndexError`。
- `heappushpop(heap, item)`: 将`item`放入堆中，然后弹出并返回堆的最小元素。这个组合操作比单独调用`heappush()`后再调用`heappop()`更高效。
- `heapify(x)`: 将列表`x`原地转换成堆，这个过程是线性时间内完成的。
- `heapreplace(heap, item)`: 弹出并返回堆中最小的项，同时推入新的`item`。如果堆为空，则引发`IndexError`。

heapq模块的高级函数

除了基本操作，``heapq``模块还提供了一些基于堆的通用函数，例如：

- `merge(\*iterables, key=None, reverse=False)`: 将多个已排序的输入合并为一个已排序的输出，返回一个迭代器。这个函数不会一次性地将所有数据放入内存，并假设每个输入流都已经排序。
- `nlargest(n, iterable, key=None)`: 返回`iterable`中最大的`n`个元素组成的列表。如果提供了`key`，则用于从每个元素中提取比较键。
- `nsmallest(n, iterable, key=None)`: 返回`iterable`中最小的`n`个元素组成的列表。同样，`key`用于提取比较键。

### deque

在Python中，*deque*（双端队列）是一个线程安全、可以快速从两端添加或者删除元素的数据类型。它是*collections*模块的一部分，提供了比列表更高效的表头插入和删除操作。

#### 基本操作

##### 创建deque

你可以使用*collections.deque*来创建一个双端队列。可以选择传入一个可迭代对象来初始化队列，也可以指定最大长度*maxlen*。

```python
from collections import deque

# 创建一个空的deque

dq = deque()

# 创建一个包含初始元素的deque

dq = deque([1, 2, 3, 4, 5])

# 创建一个有最大长度限制的deque

dq = deque([1, 2, 3], maxlen=3)
```

##### 添加元素

*deque*支持从两端添加元素，使用*append()*方法从尾部添加，使用*appendleft()*方法从头部添加。

```python
dq.append(6)	   # 添加到尾部

dq.appendleft(0)   # 添加到头部
```

##### 删除元素

同样地，*deque*也支持从两端删除元素，使用*pop()*方法从尾部删除，使用*popleft()*方法从头部删除。

```python
dq.pop()		   # 从尾部删除

dq.popleft()	   # 从头部删除
```

##### 访问和插入元素

你可以通过索引访问*deque*中的元素，但不建议这么做，因为这是一个慢速操作。*deque*也提供了插入元素的方法，名为*insert(i, x)*。

```python
# 访问第二个元素

print(dq[1])

# 在第三个位置插入字符'a'

dq.insert(2, 'a')
```

#### 高级用法

##### 限长列表

*deque*可以设置最大长度，当超出设定长度时，新加入元素会挤掉旧元素。

```python
dq = deque(maxlen=3)

for i in range(5):

dq.append(i)

print(dq)  # 输出：deque([2, 3, 4], maxlen=3)
```

##### 实现队列和栈

*deque*非常适合用于实现队列和栈数据结构。对于栈，可以使用*append()*和*pop()*方法来添加和删除元素。对于队列，可以使用*append()*和*popleft()*方法来添加和删除元素。



```python
# 栈的实现

stack = deque()

stack.append('a')

stack.append('b')

stack.append('c')

print(stack.pop())  # 输出：'c'

# 队列的实现

queue = deque()

queue.append('a')

queue.append('b')

queue.append('c')

print(queue.popleft())  # 输出：'a'
```

### Manacher

$d_1[]$：奇数回文子串臂长

```c++
std::vector<int> d1(n);
for (int i = 0, l = 0, r = -1; i < n; i++) {
  int k = (i > r) ? 1 : min(d1[l + r - i], r - i + 1);
  while (0 <= i - k && i + k < n && s[i - k] == s[i + k]) {
    k++;
  }
  d1[i] = k--;
  if (i + k > r) {
    l = i - k;
    r = i + k;
  }
}
```

```python
d1 = [0] * n
l, r = 0, -1
for i in range(0, n):
    k = 1 if i > r else min(d1[l + r - i], r - i + 1)
    while 0 <= i - k and i + k < n and s[i - k] == s[i + k]:
        k += 1
    d1[i] = k
    k -= 1
    if i + k > r:
        l = i - k
        r = i + k
```

