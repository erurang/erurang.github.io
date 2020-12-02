---
layout: post
title:  "컴파일링"
subtitle: "컴파일링"
categories: cs
tags: boostcourse
comments: true

---

Complie의 4단계
---

1. Preprocessing (전처리)
2. Compiling (컴파일)
3. Assembling (기계어로 변경)
4. Linking (병합)

```
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string name = get_string("What's your name?\n");
    printf("hello, %s\n", name);
}
```

이렇게 C로된 "hello.c" 파일이 있다고 하자.

맨위의 #include는 라이브러리 파일을 추가하라고 말하고 있다.

그러니 우리는 이 C코드에서 이렇게 표현이 가능할것이다.

Preprocessing
---
```
#include <cs50.h> == string get_string(string prompt);
#include <stdio.h> == int printf(string fromant, .. );
```

cs.50.h 파일에 들어가서 해당되는 코드를 가져와서 hello.c 해당되는 함수를 붙여넣는다.

#안에 포함된 다른 파일들도 많겟지만 hello.c에서 필요한 함수들만 가져와서 적용된 예다.

Compiling
---

![assemb](https://user-images.githubusercontent.com/56789064/88609081-33d23800-d0be-11ea-942c-89328aadd16c.jpg)


엄청나게 무섭게생긴 lowlevel 언어이며 노란색은 명령어임.

소스코드를 머신코드와 일대일 대응이 되는 단계로 바꿔준다.

Assembling
---

![zeroone](https://user-images.githubusercontent.com/56789064/88615129-45bad780-d0cc-11ea-98ac-4ce1d15a6cc9.jpg)


이제 Compiling된 언어를 가지고 0과 1로 이루어진 머신코드로 바꿔야함

Linking
---

![link](https://user-images.githubusercontent.com/56789064/88609729-d50dbe00-d0bf-11ea-8303-ebc20aa945f1.jpg)

우리는 위의 hello.c 파일에서 여러 다른 파일과 연관되어있는것을

머신코드로 바꾸고 그 기계어로 종합한걸 합치면

hello.c가 작동이 된다!
