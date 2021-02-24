---
layout: post
title:  "그룹 애너그램"
subtitle: "그룹 애너그램"
categories: boj
tags: string
comments: true

---

[그룹 애너그램](https://leetcode.com/problems/group-anagrams/)

제출코드
```
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
                
        word_dict = {}

        for i in strs:
            if ''.join(sorted(i)) in word_dict:
                word_dict[''.join(sorted(i))] += [i]
            else:
                word_dict[''.join(sorted(i))] = [i]

        result = []

        for k,v in word_dict.items():
            result.append(v)
        
        return result
```

vscode에서 실험해봄

```
strs = ["eat","tea","tan","ate","nat","bat"]

# 어떻게.. 처리할까??..
# 만약에 sort를 하더라도..? 기본형을 저장해야하나? 딕셔너리로??..

# 그럼 sort를 했을때의 딕셔너리에 벨류를 기본형으로 추가함.
# sort됫을떄 안됫을때 2가지를 선택하자
# dict는 in 처리할때 key가 있는지 확인함
# a = {'a':1,'b':2}
# print('a' in a) => true


word_dict = {}

for i in strs:
    # "abc"를 sorted() 하면 리스트형태로 정렬되서나옴
    # 그래서 ''.join()을 통해서 str으로 다시 되돌려줌

    if ''.join(sorted(i)) in word_dict:
        word_dict[''.join(sorted(i))] += [i]
    else:
        word_dict[''.join(sorted(i))] = [i]

result = []

for k,v in word_dict.items():
    result.append(v)

print(result)
```
