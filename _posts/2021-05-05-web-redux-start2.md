---
layout: post
title:  "Redux를 사용하지않았을때"
subtitle: "Redux를 사용하지않았을때"
categories: web
tags: redux
comments: true

---

### 리덕스를 사용하지 않았을때

우리는 버튼을 클릭했을때 나머지 박스의 색깔도 변하도록 하는 코드를 짜고싶다.

![image](https://user-images.githubusercontent.com/56789064/117151480-17ed8680-adf4-11eb-8824-b7f486c8e684.png)

만약 우리가 파란색 버튼을 클릭하였을때 일어나는 경우를 생각해보자. 자기자신의 색깔변화 한가지, 다른 버튼의 색깔변화 한가지,

총 2가지의 로직이 필요하다. 각 버튼마다 2개의 로직으로 총 4번의 로직이 필요하다.

만약의 3가지 버튼이라면? 1번 버튼을 누를때의 자기자신변화, 2번,3번 변화.. 한 버튼마다 3가지의 로직이 생겨 총 9번의 로직이 필요해진다

![스크린샷 2021-05-05 오후 10 51 53](https://user-images.githubusercontent.com/56789064/117151930-7f0b3b00-adf4-11eb-87e7-d308e76ab918.png)

만약 4가지라면?? 총 16번의 로직이 필요할것이다. 만약에 100개라고 가정해보면.. 우린 총 1만번의 로직을 적어줘야한다. 이거 완전 미친 노가다가아닌가.

만약에 한 버튼을 삭제하는 경우라도 생긴다고 해보자. 그러면 우리는 100가지의 버튼들의 로직중에 삭제되는 버튼의 로직을 일일이 찾아서 하나하나 삭제해야한다.

이렇게 모든 요소마다 서로서로가 연결되있는 이 상태를 볼수있고 코드의 복잡성이 엄청나다는것을 알수있다.

### 리덕스를 사용하였을때

![스크린샷 2021-05-05 오후 11 00 36](https://user-images.githubusercontent.com/56789064/117153208-b62e1c00-adf5-11eb-84de-0f554edbed11.png)

우리는 4개의 버튼을 가지고있고 검은색이 모든 정보를 모은 store라고 생각해보자. (1번)

![스크린샷 2021-05-05 오후 11 01 27](https://user-images.githubusercontent.com/56789064/117153348-d52cae00-adf5-11eb-8dbf-aca375c199f6.png)

파란버튼을 눌렀을때 파란버튼과 다른 버튼들의 색깔이 바뀌어야 된다. 우리는 이전에는 각 버튼마다 연결해서 서로서로 상태를 주고받았다면, 

이번에는 스토어에 파란색의 상태변화가 일어났다! 하고 알려주는 로직 하나를 짜준다. (3번)

![스크린샷 2021-05-05 오후 11 04 20](https://user-images.githubusercontent.com/56789064/117153760-3c4a6280-adf6-11eb-8118-e401d8bde46c.png)

그럼 스토어는 스토어를 구독중인 여러 버튼들에게 파란색버튼의 상태변화가 일어났으니 니네도 상태를 바꿔! 하고 알려준다. 

![스크린샷 2021-05-05 오후 11 08 01](https://user-images.githubusercontent.com/56789064/117154380-bed32200-adf6-11eb-88fb-811ee0eca1d1.png)

그럼 이번에 로직을 생각해보자. 내 상태가 바꼇다는것을 스토어에 알리는 로직하나, 상태가 변경됫을때의 로직하나로 다른 버튼들이 몇개가 있던간에 각 버튼마다 딱 2개의 로직만 필요하게된다.

### 리덕스를 사용하지않았을때 코딩
```
<html>
    <body>
        <style>
            .container {
                border:5px solid black;
                padding: 10px;
            }
        </style>
        <div id="red"></div>
        <div id="green"></div>
        <div id="blue"></div>
            <script>
                function red() {
                    document.querySelector('#red').innerHTML= `
                    <div class="container" id="component_red">
                        <h1>red</h1>
                        <input type="button" value="fire" onclick="
                        document.querySelector('#component_red').style.backgroundColor = 'red'
                        document.querySelector('#component_green').style.backgroundColor = 'red'
                        document.querySelector('#component_blue').style.backgroundColor = 'red'
                        "
                    </div>
                    `
                }
                red();

                function green() {
                    document.querySelector('#green').innerHTML= `
                    <div class="container" id="component_green">
                        <h1>green</h1>
                        <input type="button" value="fire" onclick="
                        document.querySelector('#component_green').style.backgroundColor = 'green'
                        document.querySelector('#component_red').style.backgroundColor = 'green'
                        document.querySelector('#component_blue').style.backgroundColor = 'green'
                        "
                    </div>
                    `
                }
                green();

                function blue() {
                    document.querySelector('#blue').innerHTML= `
                    <div class="container" id="component_blue">
                        <h1>blue</h1>
                        <input type="button" value="fire" onclick="
                        document.querySelector('#component_blue').style.backgroundColor = 'blue'
                        document.querySelector('#component_red').style.backgroundColor = 'blue'
                        document.querySelector('#component_green').style.backgroundColor = 'blue'
                        "
                    </div>
                    `
                }
                blue();

            </script>
    </body>
</html>
```

### 결과
![a](https://user-images.githubusercontent.com/56789064/117159327-268b6c00-adfb-11eb-8a80-ca3b4bdcd212.gif)

물론 작동은 원하는대로 작동은 된다. 하지만 요소마다 로직이 3번, 총 9번의 로직이 필요해졌으며, 
각각의 요소마다 아래와같은 코드를 일일이 적어서 코딩해줘야하며 요소간의 독립성도 보장이 되지 않게되었다.

```
document.querySelector('#component_blue').style.backgroundColor = 'blue'
document.querySelector('#component_red').style.backgroundColor = 'blue'
document.querySelector('#component_green').style.backgroundColor = 'blue'
```

다음엔 리덕스를 사용하였을때와 리덕스의 사용법에 대해 알아보자.