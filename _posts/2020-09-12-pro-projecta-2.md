---
layout: post
title:  "웹UI 프로젝트A-2"
subtitle: "웹UI 프로젝트A-2"
categories: project
tags: uiux
comments: true

---


![table](https://user-images.githubusercontent.com/56789064/92996771-0e539d80-f549-11ea-8491-ecb98fa9e9e1.png)
![form](https://user-images.githubusercontent.com/56789064/92996767-0c89da00-f549-11ea-9bcc-56514f7f7cfe.png)
![list1](https://user-images.githubusercontent.com/56789064/92996769-0dbb0700-f549-11ea-9867-1be361547d2f.png)
![list2](https://user-images.githubusercontent.com/56789064/92996770-0e539d80-f549-11ea-91f2-013f111c56c8.png)


https://www.edwith.org/boostcourse-ui/project/156/content/128#summary

어제 다 만들어 놓았다.

프로젝트 A-1 에서 PASS가 나와야 제출이 되는것 같다.

만들면서 배운건 표만드는게 진짜 생각보다 어려웠다.

그리고 list안에선 a div strong을 쓸수있다 나머지는 html 검사에서 오류가 떳다.

나머지는 크게 시간보낸점이 없었던것 같다.

아마 제출하면 문제로 지적할 곳이 실시간검색어 list-style부분이랑

웹툰쪽 a태그 영역문제 정도일것같다..

제출후에 피드백은 다시 적겠다.
---

9월16일 피드백이 왔다.

웹툰영역이랑 form쪽은 pass를 할줄알았는데 3부분 다 fail이 나왔다. ㅠ_ㅠ..

div로 막 갈길게아니라 **일반적으로 문서에서 목차에 해당하는 것은 헤딩 태그로 적용**하기

리스트타입에서 **용어 설명은 dl/dt/dd 태그**를 사용하기

테이블 표에서 **제목이 될만한 내용은 th**로 쓰기

form부분은 label을 하나하나 다 연결을 시켜줘야하는거였다. (하나만연결하면 되는줄..)

**HTML에서 img를 쓸때 width와 height를 정해주라는점.**

```
html이 브라우저에 랜더링될 때에는 리플로와 리페인트가 발생합니다. 
이미지가 많고 전송 회선이 느린 페이지를 열 경우에 이미지가 로딩되면서 페이지 길이가 계속 늘어나는 경우를 볼 수 있습니다.
하지만, HTML 문서에 직접 이미지의 width와 height 속성을 적용했다면 얘기는 달라집니다.
브라우저가 HTML을 로딩하는 순간 이미 각각의 이미지가 차지하는 영역을 알고 있으니까요.
즉, 처음부터 문서의 길이를 모든 이미지를 수용할 수 있을 만큼 늘려서 렌더링한다는 얘기입니다.
고정된 사이즈를 랜더링 한다면 성능 향상에 도움이 됩니다.
```

다시 다 수정후 제출했다. 이번엔 다 pass라고 확신한다!
