---
layout: post
title:  "env 환경설정 + 유효성검사"
subtitle: "env 환경설정 + 유효성검사"
categories: project
tags: ubereats
comments: true

---

## env를 왜 사용할까? 

데이터베이스의 아이디 비밀번호, 포트번호, API키... 등등 우리가 외부에 보여선 안되는 키들을 숨기는 방법은 없을까?

이 문제를 해결하기위해 `dotenv`가 등장했다.

```
npm i dotenv
```

현재 디렉토리에 위치한 `.env` 파일로 부터 환경 변수를 읽어낸다. `.env`파일을 만든후 파일에 저장해놓은 환경변수를 

`dotenv`를 통해서 `process.env`에 설정할수있다. 예를들어 `.env`파일이 아래처럼 설정되있다면 

```
API_KEY = abcdefgh
```
사용하고자하는 파일에서 

```
import dotenv from "dotenv"
dotenv.config()
```

자동적으로 `process.env`에 등록이 되고, `process.env.API_KEY`로 접근이 가능해진다.


## nest에서 env사용

```
npm i --save @nestjs/config
```

프로젝트 최상단 루트에 개발용/배포용/테스트용 환경변수를 각각 다르게 적용하기 위해서 `.env.dev .env.prod .env.test` 를 만들어 주자.

## 그런데 왜 개발모드와 배포모드를 다르게 둘까?

개발 모드는 버그로 이어질만한 많은 부분을 미리 경고해주는 검증 코드가 포함되어 있다. 하지만, 이런 코드는 번들 크기를 증가시키고 앱 속도를 느리게 할 수 있다.

개발모드에서의 느린 앱 속도는 문제가 되지 않지만, 배포 모드에선 유익한 것이 없으니 이런 검증들은 생략해야 한다.

## app.module.ts

```
import { ConfigModule } from "@nestjs/config"

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true, // 모든 모듈이 env에 접근할수있도록 global 선언
      envFilePath: process.env.NODE_ENV === "dev" ? '.env.dev' : '.env.test'
      ignoreEnvFile: process.env.NODE_ENV === 'prod',
    }),
  ],
  controllers: [],
  providers: [],
})

export class AppModule { }
```

## joi

```
npm i joi
```

환경변수의 타입을 체크하고싶을때(유효성검사)는 어떻게하면 좋을까? `joi`를 설치후 `app.module.ts`에 `validationSchema`를 추가해주자

```
import * as Joi from "joi"

@Module({
    imports: [
    
    ConfigModule.forRoot({
        ...

      validationSchema: Joi.object({
        
        NODE_ENV: Joi.string().valid('dev', 'prod'),
        
        // env에 선언된 환경변수 이름
        API_KEY: Joi.string().required(),
        
        ... 환경변수 파일에 선언된 변수들..

      })
    }),
  ],
  ....
})
```

이제 우리는 env에대한 유효성 검사도 하여 조금 더 안정적으로 개발을 하게 되었다.

## cross-env

프로젝트 참여자 각각이 MacOS, Windows, Linux 등 다양한 OS를 사용한다면 운영체제마다 환경변수 설정법이 다르기 떄문에 문제가 생긴다.

간편한 설정을위해 `cross-env`를 설치해주자.

```
npm install --save-dev cross-env
```

`package.json` 스크립트를 지정 env로 실행할수있도록 수정해준다.

```
"start": "cross-env NODE_ENV=prod nest start",
"start:dev": "cross-env NODE_ENV=dev nest start --watch",
"start:prod": "node dist/main",
"test": "cross-env NODE_ENV=test jest",
```

`env` 세팅은 모두 끝낫다!

깃허브 링크 : [nest env설정](https://github.com/erurang/js/commit/a6390141d3f67fcf309cb9f479622a4c03017420)