# Chap 1 - REST API 및 프로젝트 소개 

## REST API라 불리기 위한 조건
- Self-Descriptive Message
- HATEOAS(Hypermedia as the engine of application state)
  
## REST API
* API
    * **A**pplication **P**rogramming **I**nterface
* REST
    * **RE**presentational **S**tate **T**ransfer
    * REST API: REST 아키텍처 스타일을 따르는 API

## REST 아키텍쳐 스타일
* Client-Server
* Stateless
* Cache
* Uniform Interface
* Layerd System
* Code-On-Demand (optional)

## Self-Descriptive Message
메시지 스스로 메시지에 대한 설명이 가능해야 하는 것. 서버가 변해서 메시지가 변해도 클라이언트는 그 메시지를 보고 해석이 가능해야 한다. 
>  확장 가능한 커뮤니케이션


## HATEOAS
어떤 응답을 받았던 간에 애플리케이션의 다음 상태로 전이를 하려면 응답에 있는 url로 상태 전이를 해야 함. 하이퍼미디어(링크)를 통해 애플리케이션 상태 변화가 가능해야 한다. 링크 정보를 동적으로 바꿀 수 있다(Versioning 할 필요가 없어야 함)

----
### 잘못된 예시
```
{"langCode" : "ko"}
```
위의 예시에서 `langCode`가 무엇을 의미하는지, `ko` 가 무엇을 의미하는지 알 수 없으므로 self-descriptive 하지 않다. 또한 다음 상태로 전이할 url이 존재하지 않으므로 hateoas하지도 않다.


* rss 타입
  + Real Simple Syndication
  + 기사들의 제목, 헤더들을 뽑아 파일로 만들어 놓은 것

---
## Self-descriptive message 해결 방법
1. 미디어 타입 정의, Content-Type
2. **profile 링크 헤더를 추가!!**
   
## HATEOAS 해결 방법
1. **HAL을 이용해 데이터에 링크 제공!!**
2. 링크 헤더나 location 제공 