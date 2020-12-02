---
layout: post
title:  "Recursion"
subtitle: "Recursion"
categories: cs
tags: boostcourse
comments: true

---

재귀함수
---

함수가 본인 스스로를 호출해서 사용하는것을 재귀라고 한다.

```
#
##
###
####
```

피라미드를 만든다고 할때 for문을 2번도는 방법과

재귀적으로 푸는 방법 2가지가있다.

```
#include <cs50.h>
#include <stdio.h>

void draw(int h);

int main(void)
{
    // 사용자로부터 피라미드의 높이를 입력 받아 저장
    int height = get_int("Height: ");

    // 피라미드 그리기
    draw(height);
}

void draw(int h)
{
    // 높이가 h인 피라미드 그리기
    for (int i = 1; i <= h; i++)
    {
        for (int j = 1; j <= i; j++)
        {
            printf("#");
        }
        printf("\n");
    }
}
```

위는 반복문을 2번 쓰는방법으로 구현되었다.

```
#include <cs50.h>
#include <stdio.h>

void draw(int h);

int main(void)
{
    int height = get_int("Height: ");

    draw(height);
}

void draw(int h)
{
    // 높이가 0이라면 (그릴 필요가 없다면)
    if (h == 0)
    {
        return;
    }

    // 높이가 h-1인 피라미드 그리기
    draw(h - 1);

    // 피라미드에서 폭이 h인 한 층 그리기
    for (int i = 0; i < h; i++)
    {
        printf("#");
    }
    printf("\n");
}
```

이렇게 draw(h-1)에서 재귀적으로 자기를 계속 부르는거다.

**한번 순차적으로 적어보자**

```
h = 4

draw(4) -> draw(3) -> draw(2) -> draw(1) -> draw(0)

	                                               ↓

 ####   <-   ###   <-   ##    <-    #    <-   h == 0 return
```

재귀는 stack형식으로 쌓여있음.

그래서 종료조건인 h==0이 됬을때 draw(1) 부터 draw(4)까지

아래 for문을 실행해서 피라미드를 만들수 있게됨.

재귀는 병합정렬(merge sort)의 기본이 됨.