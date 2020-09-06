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
```

#### 2.stack

List  简单实现

```python
stack = []
stack.append(c)
stack.pop() # list.pop(index=-1)
```

3.