---
layout: post
title:  "let,const 선언과 메모리모델"
subtitle: "let,const 선언과 메모리모델"
categories: web
tags: javascript
comments: true

---

### 원시타입

```
Number : 숫자
String : 문자
boolean : true or false
null : 값은 값이지만 의미없는 값이 선언되있음 , gc에 의해서 메모리에서 삭제가능함
undefined : 변수가 선언은 되었으나 값이없음
symbol : 
```

원시타입에 대해선 이곳 [mutable immutable](https://erurang.github.io/web/2021/05/11/js-mutable/) 에 정리해두었다.

### 자바스크립트 변수 선언에는 3가지가 있다

```
var let const 
```

var는 앞으로도 절대 쓰면 안된다. 있다고만 알아두자 (옛날방식임)

```
var a = 1
var a = 2
```

그 이유는 무엇인가? var는 자동으로 변수를 선언했을때 호이스팅이 된다.

호이스팅이 된다는건 변수가 끌어올려져서 선언된다 라고 이해하면 좋다.

var는 이미 선언되있어도 재선언이 가능하다. 오류없이 실행이 가능하다.

그리고 호이스팅이 되었을때 `a = undefined`를 초기값으로 메모리가 할당되어서 오류가 일어나지않는다

```
let a = 1
let a = 2 (x)
a = 2 (o)
```

호이스팅이 되지않아서 값을 할당하지않고 사용하려하면 에러가 발생한다. 기존 할당값을 변경시킬수있다.

```
const a = 1
const a = 2 (x)
a = 2 (x)

const a = { name : 'erurang' }
a.name = 'hi' (o)
```

호이스팅이 되지않아서 값을 할당하지않고 사용하려하면 에러가 발생한다. 기존 할당값을 변경시킬수없다.

하지만 객체안의 프로퍼티의 추가 변경 삭제는 문제가없다. 만약에 객체도 변경되지않게 하려면 frezze() 메소드를 사용할수있다.


### 변수를 선언하고, 초기화하고, 값을 할당했을때 우리 메모리에선 어떤 일이 일어날까?

```
1===
let num = 12 (0x1234)

2===
let newNum = num

3===
num += 1

4===
num = 13?? (0x2345)

5===
const num = { age : 10 }
num.age = 15 (o)
```

1번이 실행되면 num은 0x1234(임시주소)에 value 12가 저장되어진다. 즉 num은 값 12가 저장된 메모리주소를 가르킨다로 이해하면된다.

2번이 실행되면 newNum은 num이 가르키는 메모리주소 0x1234를 같이 가르키게된다. 0x1234는 12의 값을 가지고있으니 nuewNum과 num은 같은 주소를 참조하고있다라고 표기할수있다.

3번이 실행되면 이제 num이 +1이 되었으니 newNum도 +1이 되어서 0x1234의 값이 13으로 변하게될까?

답은 틀렸다. 이다. 3번이 실행된후 4번의 메모리주소는 다른 새로운 주소를 할당받는다. 그래서 newNum은 0x1234라는 값을 그대로 가르킨 상태이고 num의 메모리 주소만 0x2345로 변하게된다.

5번의 경우를 보자. 5번의 const는 값을 변경할수없다. 라고 배웠다. 하지만 객체가 `num.age`는 변경이 가능하다. 이유가 뭘까?

일단 우리 콘솔을 키고 `{} === {}`를 해보자. 이 값은 true일까? false일까? 정답은 false다. 왜냐하면 오브젝트가 생성될때 각각 새로운 주소로 할당되기때문이다.

그래서 이게 무슨관련인가? 5번을 다시보면 num은 { age : 10 } 의 주소값을 가르키고 있을뿐이다. 그안의 age값까지 가지고있지않다. 즉 num은 { } 의 주소만 가르키고있을뿐

객체안의 내용까지는 const로 가르키고 있지 않기 때문에, 즉 객체안의 요소들의 주소는 { }가 가지고있다. 객체안의 변수들은 수정이 가능한것이다.

간단히.. 객체내의 값을 바꾸는것은 가르키는 메모리 주소의 변경을 의미한다. 변수에 할당된 값을 바꾸는 것이 아니다. 라고 이해함이 편하다..

