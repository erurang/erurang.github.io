---
layout: post
title:  "운영체제란? System Call"
subtitle: "운영체제란? System Call"
categories: cs
tags: os
comments: true

---


## 운영체제란?

운영체제는 시스템 자원을 관리하고 효율적으로 분배하며, 사용자와 컴퓨터간의 컴퓨터 커뮤니케이션을 지원한다.

즉 프로그램이 요청하는 자원을 효율적으로 관리하는 소프트웨어 라고 할수있다.

## 운영체제는 어디 존재하는가?

운영체제는 SSD/HD 같은 저장소에 있으며, 실행시에 메모리에 올라가서 실행이 된다. (모든 코드는 메모리에 올라가서 실행이 된다)

## 사용자가 운영체제를 통해 자원을 요청할때 시스템 콜을 사용한다.

CPU는 Protection Ring으로 이루어져있다.

![스크린샷 2022-01-25 오전 11 25 51](https://user-images.githubusercontent.com/56789064/150899342-6e79b113-5283-436e-af3e-ad9ce662ea8f.png)

커널모드와 사용자모드로 나뉘어져 있는데, OS가 사용자 <---> 커널 사이에서 시스템콜으로 요청한 자원을 사용자에게 준다.

왜 나뉘어있을까? 사용자모드에서 함부로 응용프로그램이 전체 시스템을 해치지 못하도록 막기위함이다.

## 요청한 자원을 주는 방식

```
    USER
------------------------
    APP/SHELL
------------------------
    API
------------------------
    Stystem Call
------------------------
    OS
------------------------
    SSD/HD .. 저장장치
```

1. 사용자는 필요한 자원을 얻기위해서 시스템콜을 사용한다.
2. App이나 shell에서 사용요청을 하면
3. 응용프로그램은 시스템 콜을 부르는 API를 요청한다
4. 사용자모드 -> 커널모드로 변경이 되고
5. 시스템 콜이 요청되면 OS는 적절한 자원을 user에게 할당하여 돌려주며 커널모드 -> 사용자모드 로 변경된다.

즉, 시스템콜은 운영체제가 운영체제의 기능을 사용할수 있도록 명령함수(API)를 통해서 쉽게사용하도록 만든 것이다.

<!-- ![systemcall1](https://user-images.githubusercontent.com/56789064/92602522-35139900-f2e9-11ea-96c1-38066c76c237.jpg)
![systemcall2](https://user-images.githubusercontent.com/56789064/92602527-3644c600-f2e9-11ea-9928-0c287f7368ff.jpg)
![systemcall3](https://user-images.githubusercontent.com/56789064/92602528-3644c600-f2e9-11ea-8ced-5caf259baf4b.jpg)
![systemcall4](https://user-images.githubusercontent.com/56789064/92602529-36dd5c80-f2e9-11ea-9d05-821d898f00e7.jpg) -->
