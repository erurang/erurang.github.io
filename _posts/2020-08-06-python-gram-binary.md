---
layout: post
title:  "Binary Search"
subtitle: "Binary Search"
categories: python
tags: algorithm
comments: true

---

Binary Search
---

재귀적구현
---
```
def binary_search(data,search):
    if len(data) == 1 and search == data[0]:
        return True
    if len(data) == 1 and search != data[0]:
        return False

    medium = len(data)//2

    if search == data[medium]:
        return True
    else:
        if search > data[medium]:
            return binary_search(data[medium:],search)
        elif search < data[medium]:
            return  binary_search(data[:medium],search)


a = [1,2,3,4,5,6,7,8]

print(binary_search(a,9))
```

재귀구현 x
---
```
def binary_search(arr,search):
    start = 0
    end = len(arr) -1
    
    while start <= end:
        mid = (start+end) //2
        if arr[mid] == search:
            return mid
        elif arr[mid] < search:
            start = mid +1
        else:
            end = mid -1
    return None
```

시간복잡도 분석
---

<-------------------------------->
<-------------->
<------>
<--->

매 탐색마다 반씩 줄어들기때문에

n x ½ x ½ x ½ ... = 1

1이될때까지 계속 반씩 탐색하기때문에

시간복잡도는 O(logn)이 됨
