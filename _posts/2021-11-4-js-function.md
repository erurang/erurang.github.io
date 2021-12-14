---
layout: post
title:  "자바스크립트 함수의 다양한 선언방식에 대하여"
subtitle: "자바스크립트 함수의 다양한 선언방식에 대하여"
categories: web
tags: javascript
comments: true

---

## 자바스크립트의 함수 종류

1. function, arrow function
2. args
3. currying function
4. generator function
5. promise async await

## 자바스크립트 함수 선언방식을 알아보자

```
function (args){}
function name(args){}
const name = (args) => {}
```

위에서부터 차례대로 익명함수 함수 화살표함수 선언방식이 있다.

//화살표함수는 일반 함수 선언과 다르게 this가 bind되지 않는다// 수정

## 가변인자

```
function sum(a, b, ...args) {
  console.log(...args); 
}

sum(10,20,30,40,50)
```

위처럼 사용하면 콘솔에는 어떤값이 찍힐까? 일단 a,b에는 10,20 이라는 인자가 순서대로 들어가는데

남은 30,40,50은 ...args 라는 인자로 받아올수있는데, 이것을 가변인자라고한다.

가변인자는 정해논 변수외에 들어오는 인자들이 배열로 저장되어있다. 그래서 위의 함수를 실행한 결과는 아래와같다

```
[30,40,50]
```

정해진 인자를 제외하고 다른 추가값을 넣기위해선 가변인자를 사용하면된다.

## 함수호출하는 3가지 방식

함수 호출방식에도 3가지 방식이 있다. 실습을통해 알아보자. 

```
// 평소에 사용하던 방식
sum(10,20,30,40)

sum.call(null,10,20,30,40)

sum.apply(null,[10,20,30,40])
```

셋다 sum 함수를 호출하는 방식이다. 우리는 가장 위의 방법으로 함수를 사용한다. 아래 2가지 함수호출 방식과 무슨 차이가 있을까?

공통점으로 call과 apply는 첫번째 인자로 Context를 받는다.

Context에 대해서는 [자바스크립트 Context에 대해 알아보자]()에 정리해두었다.

차이점으로 call은 인자를 직접 적어 넘겨주고, apply는 인자를 배열에 담아 넘겨준다

call에서는 인자로 하나하나 넘겨서 직접 코드를 수정해야하기 때문에 직접 찾아서 수정해야하지만

apply는 배열을 인자로 받아서 처리하기때문에 인자 데이터를 변수에 관리하여 넘겨줄수있다. 

```
const data = [10,20,30,40]
sum.apply(null,data)
```

sum에는 a와 b 2개의 인자는 배열 맨앞의 인덱스 0 1 2가지 값으로 처리되며 나머지 값들은 가변인자로 처리되게된다.

## currying function

함수의 인자에는 변수뿐아니라 함수도 넣을수 있다.

```
function sumA(one) {
  return function sumB(two) {
    return one + two;
  };
}

function sumB(one, two) {
  return one + two;
}

sumA(10)(20)
sumB(10,20)
```

sumA와 sumB는 무슨차이일까? 값 두개를 더해서 값을 반환한다는 점에서는 같다.

sumA는 함수가 리턴되기때문에 첫번째 방식처럼 인자를 다시주어 함수를 재호출하여 값을 넘겨주는 방식이고

sumB는 애초부터 인자를 2개를 한번에 받아서 값을 넘겨준다는 차이가 있다. 그래서 sumA는 이런식으로 사용할수있다.

```
const a = sumA(10)

console.log(a) // function sumB
a(20)
```

a값을 로그해보면 function sumB 라는 내부에서 지정한 함수가 리턴되어 a변수는 함수가 된다.

그래서 a에 20을 넣어 다시 함수를 호출하여 사용하는 방식이다. 이렇게 변수를 하나 지정해서 함수를 호출하는 방식을 통해서 좀더 범용성있게 함수를 사용할수있게된다.

## generator function

생성기 함수라고 부르기도 한다. 이 함수는 기존의 함수선언방과 사용방식의 차이가 존재한다.

```
function func(){
    return ~~
}
funciton* gene(){
    yield ~~
    yield ~~
    yield ~~
}

func()
gene().next()
```

일반 함수는 return으로 한번 함수가 호출되면 값을 반환후 종료한다.  제네레이터 함수는 다르다. return 대신에 yield 가 사용된다. 

yield는 제너레이터를 멈추고 그 당시의 값을 객체로 돌려준다. (함수가 종료되지않는다.) 실습을 통해서 익혀보자.

```
// 일반함수

function makeInfiniteEnergyGenerator() {
  let energy = 0;

  return function (booster = 0) {
    if (booster) {
      energy += booster;
    } else {
      energy++;
    }

    return energy;
  };
}

const energy = makeInfiniteEnergyGenerator();

for (let i = 0; i < 5; i++) {
  console.log(energy());
}

console.log(energy(5));

// console.log 출력값
1
2
3
4
5
10
```

위같은 방법을 제네레이터로 사용해보자


```

// 제너레이터함수

function* infiniteEnergyGenerator() {
  let energy = 1;
  while (true) {
    // yield는 제네레이터를 멈춤!
    // yield ~ 를 호출자에게 값을 돌려줌
    // next() 로 호출값을 {value, done} 객체형태로 돌려줌
    const booster = yield energy;

    if (booster) {
      energy += booster;
    } else {
      energy++;
    }
  }

  // return이 되면 done이 true로 변함
  return;
}

const energyGenerator = infiniteEnergyGenerator();

for (let i = 0; i < 5; i++) {
  console.log(energyGenerator.next());
}

energyGenerator.next(5);

// console.log 출력값
{ value: 1, done: false }
{ value: 2, done: false }
{ value: 3, done: false }
{ value: 4, done: false }
{ value: 5, done: false }
{ value: 10, done: false }
```

제네레이터 함수내에서 원래라면 while(true)로 무한루프를 돌지만 yield를 통해서 내부 실현을 정지시키고 그때의 값을 객체로 반환해준다는것을 알수있다.

즉 for문 진입부터 차근차근 보면 아래와 같은 과정이다.

1. energy 1로 설정됨
2. while문 진입
3. yield 에서 energy 객체 값을 리턴하여 {value,done} 로그에 찍힌다.
4. 아래 if문이 처리가 되어 energy++ 
5. 다시 while문 최상단으로 들어온다

3번부터 5번 실행이 된다. 가장 중요한것은 함수를 멈춘다는것이다. 

함수를 멈춘후에 다음 next() 호출 또는 return값이 나오기전까지 계속해서 상태를 기억(유지)한다는 점이다.

제네레이터 함수에서 return이 일어나면 done: true로 바뀐다. 즉 제네레이터함수는 더이상 실행할 코드가 없다. 

## async await

```
{
  function delay(ms) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (Math.floor(Math.random() * 10) % 2 === 0) {
          resolve("success");
        } else {
          reject("failure");
        }
      }, ms);
    });
  }

  // promise를 사용할떄는 Then then then catch 이런식으로 콜백에 콜백을 달고옴
  delay(1000)
    .then((result) => console.log("done promise!" + result))
    .catch((error) => console.error("fail promise!" + error));

  // 위와 다르게 async await로 Try-catch로 콜백에 콜백을 하지않아도 사용하게됨
  async function main() {
    try {
      //resolve는 여기서걸림
      const result = await delay(3000);
      console.log("done promise!" + result);
    } catch (error) {
      // Reject는 여기서걸림
      console.error("fail promise!" + error);
    }
  }

  main();
}
```

## 