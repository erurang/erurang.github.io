---
layout: post
title:  "중복 단어 제거(사전순)(remove-duplicate-letters)"
subtitle: "중복 단어 제거(사전순)(remove-duplicate-letters)"
categories: boj
tags: queuestack
comments: true

---
![중복 단어 제거(사전순)(remove-duplicate-letters)](https://leetcode.com/problems/remove-duplicate-letters/)


이 문제에서 배운점. Counter모듈을 사용하면 자동적으로 키:밸류 로 갯수를 세어줌

그리고 ord()로 비교할 필요없이 파이썬 자체에서 a < b 를 하면 알아서 True로 리턴됨..

물론 소문자는 소문자끼리 대문자는 대문자끼리 비교할때임.

접근방법을 머리에 넣어두자.

1. 총단어들의 갯수를 카운터로셈 
2. 단어가 나올때 카운터에서 뺌
3. 내가 본 단어 리스트에 이미존재하면 다음단어를 받아옴
4. 내가 본 단어도 아닌데 내가 여태 뽑아온 단어의 끝보다 작고 단어의끝이 하나더있으면? 그럼 끝문자를 삭제함
5. 본단어에서도 삭제... 반복


```
class Solution:
    def removeDuplicateLetters(self, s: str) -> str:
        counter , seen , stack = Counter(s), set(), []
        
        for i in s:
            # 현재까지 문자에서 하나 뺴고
            counter[i] -= 1
            
            # 이미 본 단어면은 넘김
            if i in seen:
                continue
            
            # 스택이 존재하고 뽑아온 단어가 스택의 끝단어보다 순번이 작고 스택의 끝단어가 아직 1개이상존재하면
            while stack and i < stack[-1] and counter[stack[-1]] > 0:
                # 스택끝단어를 팝하고 본단어에서도 삭제함
                seen.remove(stack.pop())
                
            # 단어를 추가해감
            stack.append(i)
            seen.add(i)


        return ''.join(stack)
```

위는 책의코드

내가 처음짠코드는 s를 포문돌려서 이 단어가 유일한 단어인지부터 판단했다.

그다음에 s를 하나하나 받아올때마다 유일한단어의 리스트와 다포함한 단어의 리스트를 따로 저장했다.

그래서 매번 문자를 뽑아올때마다 중복된 단어일때 현재까지의 단어와 순서가 같은지 체크했고 아니라면 브레이크로 넘겼다

그렇게 다음루프로 넘길때 문제가생겼다. abcd가 있으면 첫루프에는 abcd 다음루프에는 bcd 이렇게 루프를 줄여가면서 했기때문에..

여튼 내 방법은 틀렸다는걸 알게됬다.
