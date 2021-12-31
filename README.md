# nomadcoders-nestJs


NestJS로 API 만들기

https://nomadcoders.co/nestjs-fundamentals



Nest는 Node.js 프레임워크인데 

Angular에서 많은 영감을 받았다고 한다.

 

 

장점 

Nest는 Express를 사용하지만 

Fastify와 같은 다양한 라이브러리와 호환성을 제공하므로 사용 가능한 외부 플러그인을 쉽게 사용 할 수 있다.

 

제공

TypeScript(바닐라 JavaScript와의 호환성 유지 )
객체지향 프로그램  [ OPP (Object Oriented Programming) ] 
기능적 프로그램 [ FP (Functional Programming) ]
FRP (Functional Reactive Programming)
 

Fastify는

 속도와 오버헤드를 가능한 줄이는데 초점을 둔 Node.js 웹 프레임워크이다.

Connect/Express와 Restify와의 호환 가능한 미들웨어를 지원하여 확장할 수 있게 디자인 돼있다.

또한 JSON schema를 사용하여 Validation과 Serialize를 정의할 수 있는 구조를 가지고 있다.

(fast-json-stringify는 이 JSON Schema를 사용해 직렬화를 고속으로 처리 하고 있다.)

출처 : https://jser.info/ko/2017/09/11/yarn-1.0workspace-node.jsfastify/

 

 

 

 

 

주목할만한 부분은 ES7(혹은 타입 스크립트)에 새로 추가된  @ 데코레이터(Decorator)를 주로 이용한다는 것 이다.

 

 

데코레이더(@) 이용방법
 

@Controller 

 

Nestjs는 기본적으로 컨트롤러(Controller)를 통해 라우팅(Routing)을 구현한다.

/cats 경로로 들어오는 리퀘스트를 처리하기 위해선

@Controller 데코레이터를 이용해서 다음과 같이 컨트롤러를 정의한다.


import {Controller} from '@nestjs/common';
 
@Controller('cats')
export class CatsController {
   // todo
}
Colored by Color Scripter
cs
 

 

 

@Get()

 

그렇다면 컨트롤러를 통해 받은 Method는 어떻게 정의할까

@Get() 데코레이터를 이용한다. 또 코드를 보면 알겠지만

Nesjs는 타입 스크립트를 기본으로 사용한다. 타입의 정의에 따른 이점을 극대화 하고 있으며,

프레임워크에 잘 녹여내고 있다.


import {Controller, Get} from '@nestjs/common';
 
@Controller('cats')
export class CatsController {
  @Get()
  findAll(): string {
    return 'This action returns all cats';
  }
}
Colored by Color Scripter
cs
 

 

 

 

@Post()

 

post메서드나 동적 매칭, 헤더, 스테이터스 코드에 대한 로직처리는 어떻게 할까

마찬가지로 데코레이터를 이용한다.

 
import {Controller, HttpCode, Get, Post, Header, Param, Body} from '@nestjs/common';
import {CreateCatDto} from "./create-cat.dto";
 
@Controller('cats')
export class CatsController {
  @Post('dto')
  async createWithDto(@Body() createCatDto: CreateCatDto) {
    return 'This action adds a new cat: ' + JSON.stringify(createCatDto);
  }
 
  @Post()
  @Header('Cache-Control', 'none')
  create(): string {
    return 'This action adds a new cat';
  }
 
  @Get()
  findAll(): string {
    return 'This action returns all cats';
  }
 
  @Get('ab*cd')
  wildcards() {
    return 'This route uses a wildcard';
  }
 
  @Get('status')
  @HttpCode(500)
  withStatusCode() {
    return 'This route status code 500';
  }
 
  @Get(':id')
  findOne(@Param() params): string {
    console.log(params.id);
    return `This action returns a #${params.id} cat`;
  }
}
 
// create-cat.dto.ts
export class CreateCatDto {
    readonly name: string;
    readonly age: number;
}
Colored by Color Scripter
cs
 

지금까지 구조를 보면 Spring 프레임워크와 매우 유사하다는걸 알수있다.

 

 

 

 

@Module()

 

개발한 컨트롤러를 모듈에 붙여주기만 하면 완성이다.


import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
 
@Module({
  controllers: [CatsController],
})
export class AppModule {}
cs
 

애플리케이션 서버를 만드는 과정도 Express만큼이나 심플하다.

 

import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
 
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
Colored by Color Scripter
cs
 

이 밖에도 Provider, Module, Middleware, Pipe, Guard, Interceptor 등 서버 개발에 필요한 다양한 개념이있다.

 

 

참고 : https://dev-momo.tistory.com/m/entry/Hello-Nestjs

 → git hub https://github.com/hg-pyun/hello-nestjs

 

 

공식 사이트

https://nestjs.com/

