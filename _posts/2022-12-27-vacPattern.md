---
layout: post
title: "VAC 패턴으로 컴포넌트 구성하기"
subtitle: "VAC 패턴으로 컴포넌트 구성하기"
categories: web
tags: react
comments: true
---

## VAC 패턴으로 컴포넌트를 구성해보자.

리액트에서 컴포넌트를 구성하는 다양한 패턴 방식이 있다. 난 이번 프로젝트에 적용했던 `VAC` 패턴에 대해 다루어보고자 한다.

리액트는 `UI` 개발과 프론트(로직..) 개발 크게 2가지로 나눌수 있다고 생각한다.

## 협업시에 생기는 문제

만약 코드를 위와같은 틀로 나누지않고 한 파일에서 모든걸 다 담당하게 되면 아래와 같은 경우가 발생한다.

프론트 개발자 `A`씨와 마크업 개발자 `B`씨가 있다. 둘은 서로 `Home.tsx` 라는 파일에서 작업을 하고있다고 하자.

### A와 B는 처음에 같은 파일에서 작업을 시작한다.

```
Home.tsx

function Home() {
	return <div>안녕하세요. 저는 마크업 개발자입니다. 우리 팀은 1명 있습니다</div>
}
```

### `A`는 숫자를 동적으로 표현하고자 한다.

```
Home.tsx

function Home() {
	const [cnt,setCnt] = useState(0)

	return <div>안녕하세요. 저는 마크업 개발자입니다. 저 말고 {cnt}명이 더있습니다.</div>
}
```

### `B`는 `CSS`를 적용하고자 한다

```
Home.tsx

function Home() {
	return <div className=".title">안녕하세요. 저는 마크업 개발자입니다. 우리 팀은 1명 있습니다</div>
}
```

이렇게 `A`와 `B`모두 한 파일을 함께 수정함으로써 저장소에 `push`할때 충돌이 발생하게 된다.

## 충돌을 피하기 위해 컴포넌트를 나누어보자.

`A`는 `Home.tsx` `B`는 `VHome.tsx` 에서 작업하도록 나누자. `V`는 `View`의 약자로 데이터를 다루지않는다는 암묵적 약속이다.

```
Home.tsx

function Home() {
	const [cnt,setCnt] = useState(0)

	const props = {
		cnt,
		setCnt : () => setCnt(prev => prev+1)
	}

	return <VHome {...props}>
}
```

`A`는 `Home` 컴포넌트에서 데이터 로직만을 다루고 `VHome` 컴포넌트로 `props`를 넘겨주고있다. 단순히 `A`의 역할은 이것으로 마무리이다.

```
VHome.tsx

interface IProps {
	cnt : number;
	setCnt : () => void;
}

function VHome({cnt, setCnt} : IProps) {
	return (
		<>
			<div className=".title">안녕하세요. 저는 마크업 개발자입니다. 우리 팀은 {cnt}명 있습니다</div>
			<button className=".button" onClick={setCnt}>팀원 숫자를 늘려봅시다</button>
		</>

	)
}
```

`B`는 `props` 속성값들을 연결(ref)만 해줄뿐 `setCnt`가 어떻게 작동하는지에 대해서는 알필요가 없다. 그리고 `VHome` 에서 마크업이 추가되어도

`A`와는 아무런 충돌이 일어나지않는다. `A`는 `A`의 일을 `B`는 `B`의 일을 하면 된다. 서로의 파일에서 서로의 코드가 어떻게 동작하는지 신경쓰지않아도 된다는것이 이 패턴의 장점이다

이 패턴의 중요성은 `B`는 데이터 로직에 직접 관여하지 않는다는 것이다.

## 단점

만약 데이터를 다루는 상위 컴포넌트에서 `View` 컴포넌트로 많은 `Props를` 보내게 된다면 흔히 말하는 `Props drilling`이 문제가 생긴다.

이 패턴을 적용해보았는데, 단점을 감안하고도 `Controller`와 `View`을 분리하여 개발하는것이 코드를 파악하고 유지보수 하는데 더욱 편리했다.
