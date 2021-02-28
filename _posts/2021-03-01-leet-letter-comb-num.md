---
layout: post
title:  "전화 번호 문자 조합(letter-combinations-of-a-phone-number)"
subtitle: "전화 번호 문자 조합(letter-combinations-of-a-phone-number)"
categories: boj
tags: backtracking
comments: true

---
[전화 번호 문자 조합(letter-combinations-of-a-phone-number)](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

이걸 일단 내가 풀었다는거에 난 나한테 감사하다. 고민하고 고민한끝에 풀긴풀었다.

근데 좀 답을 끼워맞춘 느낌이 나긴했다. 일단 재귀를 하기위한 과정을 고민한끝에 나온결론은.

1. 단어 선택할때 다음 digits가 있나 확인함 있으면 다음루프로들어감
2. 없으면 집어온 단어에 있는 단어를 하나하나 추가함
3. 다 추가했으면 돌아옴 
4. 반복

이렇게 무작정 주석달고 어케든 해봤다..

```
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        
        table = {
            "2" : ["a","b","c"],
            "3" : ["d","e","f"],
            "4" : ["g","h","i"],
            "5" : ["j","k","l"],
            "6" : ["m","n","o"],
            "7" : ["p","q","r","s"],
            "8" : ["t","u","v"],
            "9" : ["w","x","y","z"]
        }
        
        ans = []
        
        if len(digits) == 1:
            return table[digits]
        elif len(digits) == 0:
            return ans

        def comb_n(digit,w):

            if len(digit) == 0:
                if len(digits) == len(w):
                    ans.append(''.join(w[:]))
                return

            for i in range(len(digit)):
                for j in table[digit[i]]:
                    w.append(j)
                    comb_n(digit[i+1:],w)
                    w.pop()
    
        
        comb_n(digits,[])
        
        return ans
```

solution
```
def back(comb, next_digits):
    # 종료조건
    if len(next_digits) == 0:
        output.append(comb)
    else:
        # 포문을 두번 안쓰고 
        # 애초부터 처음숫자를 받고 테이블에있는 리스트를 가져옴
        # 예시에선 [a,b,c]
        for letter in phone[next_digits[0]]:
            # comb(현재까지) + a , 다음칸으로 넘김
            back(comb + letter, next_digits[1:])

back("",digits)
```

이게 훨 깔끔한 코드고 보기도좋다.