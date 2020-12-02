---
layout: post
title:  "Queue , PriorityQueue"
subtitle: "Queue , PriorityQueue"
categories: python
tags: algorithm
comments: true

---

Queue(first in first out)
---
```
import queue

data_queue = queue.Queue()

data_queue.put("test")
data_queue.put("1")
-> data_queue에 데이터를 넣음

data_queue.qsize()
->2

data_queue.get()
-> "test" 
-> pop과 같이 get을하면 먼저 들어온 값이 나오게됨
```

Queue(last in first out)
---
```
import queue

data_queue = queue.LifoQueue()

data_queue.put("test")
data_queue.put("1")
-> data_queue에 데이터를 넣음

data_queue.qsize()
->2

data_queue.get()
-> 1
-> stack 같이 나중에 들어온 값이 먼저 나오게됨
```

PriorityQueue()
---
```
import queue

data_queue = queue.PriorityQueue()

data_queue.put((10,"korea"))
data_queue.put((5,1))
data_queue.put((15,"japan"))

data_queue.size()
-> 3

data_queue.get()
-> (5,1)
```

우선순위 큐에서는 (우선순위 , 값) 으로 처리되어서

(5,1) -> (10,"korea") -> (15,"japan") 순으로 출력이됨