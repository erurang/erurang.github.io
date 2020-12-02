---
layout: post
title:  "Linear Search & typedef"
subtitle: "Linear Search & typedef"
categories: cs
tags: boostcourse
comments: true

---

Linear Search
---

![linear](https://user-images.githubusercontent.com/56789064/89124108-54135400-d50f-11ea-81aa-e08c740b1776.jpg)

왼쪽에서 오른쪽으로 차례대로 검색하는 방법으로

50을 찾는다고 했을때 우리는 상자안의 숫자가

정렬이 되어있지 않은 상태에서 왼쪽부터 순서대로

열어보는 수 밖에 없다. 7번만에 찾아낼수있다.

C에서 구현해보자

```
#include <stdio.h>

int main(void)
{
    int numbers[6] = { 4,
                       8,
                       15,
                       16,
                       23,
                       42 }

    for (int i = 0 ; i<6; i++)
    {
        if (numbers[i] == 50)
        {
            printf("Found\n");
            return 0;
        }
    }
    printf("Not fount\n");
    return 1;
}

```

숫자일경우 우리는 for문에서 연사자 ==를 이용해 찾을수있다.

string일 경우는 어떨까?

python이나 java에서는 == 로 비교할수있지만

C에서는 문자 하나하나를 == 로 비교해서 같은지 

구분하는 방법밖에 없다. 불편함을 해소하기 위해

**C에는 strcmp**라는 함수가 구현되어 있다.

```
#include <stdio.h>
#include <string.h>

int main(void)
{
    string names[4] = {"EMMA","RODRIGO","BRIAN","DAVID"};

    for (int i = 0 ; i < 4; i++)
    {
        if (strcmp(names[i] , "EMMA") == 0)
        {
            printf("Found\n");
			return 0;
        }
    }
    printf("Not fount\n");
    return 1;
}
```

strcmp 함수는 비교될 string과 같을경우 0를 반환함.

그럼 우리만의 전화번호부 프로그램을 만들어보자.
```
#include <stdio.h>
#include <string.h>

int main(void)
{
    string names[4] = {"EMMA","RODRIGO","BRIAN","DAVID"};
	string numbers[4] = {"123-456","456-789","101-112",131-415"};
	
    for (int i = 0 ; i < 4; i++)
    {
        if (strcmp(names[i] , "EMMA") == 0)
        {
            printf("%s\n",numbers[i]);
			return 0;
        }
    }
    printf("Not fount\n");
    return 1;
}
```

이제 우리는 EMMA가 존재하면 같은 index에 있는 숫자를 반환하는 식으로 짯다.

그런데 여기서 하나 문제가있다.

EMMA의 번호가 123-456 이라고 장담할수없다.

456-789일수도있고 101-112일수도 있기 때문이다.

그래서 **C에는 typedef 라는 구조체**가 존재한다.

```
#include <stdio.h>
#include <string.h>

typedef struct
{
    string name;
    string number;
}person;

int main(void)
{
    person people[4];


    people[0].name = "EMMA";
    people[0].number = "123-456";
    people[1].name = "RODRIGO";
    people[1].number = "456-789";
    people[2].name = "BRIAN";
    people[2].number = "101-112";
    people[3].name = "DAVID";
    people[3].number = 131-415";

    // EMMA 검색
    for (int i = 0; i < 4; i++)
    {
        if (strcmp(people[i].name, "EMMA") == 0)
        {
            printf("Found %s\n", people[i].number);
            return 0;
        }
    }
    printf("Not found\n");
    return 1;
}
```

int float string.. 처럼 우리는 person 이라는 자료형을 만든것이다.

person 이라는 자료형으로 people[4]라는 배열을 만들고

person의 0번째 인덱스의 name과 number를 설정해준다.

이렇게 우리는 **typedef를 통해서 새로운 구조체**를 만들어서

더욱 확장성 있는 전화번호부 검색 프로그램을 만들수 있게 되었다.


