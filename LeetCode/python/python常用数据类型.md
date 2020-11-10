# Python STL

#### 1.Queue PriorityQueue heap

```python
from Queue import Queue, PriorityQueue
q = Queue()#创建一个空队列，队列大小没有指定 
q1 = Queue(3) #在创建队列时，指定队列大小（表示该队列最多能存多少个元素）
q.put(1)
q.empty()
q.full()
value = q.get()#遵循的原则是：先进先出
q.qsize()

q = PriorityQueue()
# 特点：val数字越小，优先级越高
q.put((val, node))
val, node = q.get()

import heapq #实现的是小顶堆，如果要实现大顶堆，push进list的数取相反数
min_heap = []
heapq.heappush(min_heap, num)
heapq.heappop(min_heap)
min_heap[0] #小顶堆


from collections import deque
d = deque()
d.append(x) #将x添加到deque的右侧；
d.appendleft(x) #将x添加到deque的左侧；
d.pop() #移除和返回deque中最右侧的元素，如果没有元素，将会报出IndexError；
d.popleft() #移除和返回deque中最左侧的元素，如果没有元素，将会报出IndexError；
d.clear() #将deque中的元素全部删除，最后长度为0；
d.count(x) #返回deque中元素等于x的个数；
d.extend(iterable) #将可迭代变量iterable中的元素添加至deque的右侧；
d.extendleft(iterable) #将变量iterable中的元素添加至deque的左侧，往左侧添加序列的顺序与可迭代变量iterable中的元素相反；
d.remove(value) #移除第一次出现的value，如果没有找到，报出ValueError；
d.reverse() #反转deque中的元素，并返回None；
```

#### 2.stack

List  简单实现

```python
stack = []
stack.append(c)
stack.pop() # list.pop(index=-1)
```
