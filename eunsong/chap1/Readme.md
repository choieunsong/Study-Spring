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

* HAL(Hypertext Application Language)
  * HAL은 API 리소스 사이를 하이퍼링크 위한 쉽고 지속적인 방법 제공
  

-----
# "Event" REST API
## GET/api/events
* 이벤트 목록 조회 REST API
  * 로그인 안 한 상태와 로그인 한 상태 구분해야 함
    * 이벤트 목록
    * 링크
      * self
      * profile
      * get-an-event
      * next
      * prev
  * 로그인 했을 땐 create-new-event 추가: 이벤트 생성할 수 있는 API 링크

## POST/api/events
* 이벤트 생성

## GET/api/events/{id}
* 이벤트 하나 조회

## PUT/api/events/{id}
* 이벤트 수정
  
---
## 롬복이란(lombok)?
  * getter, setter를 annotation 설정으로 적용할 수 있는 라이브러리
```java
@Data
public class Student {
    private int id;
    private String name;
    private int grade;
    private String department;
}
 ```

### Event Test 만들기
+ Event 클래스에서 command + shift + T (맥기준) 하면 test 폴더 밑에 자동으로 EventTest 파일 생성
+ EventTest에 lombok의 @Test 어노테이션 붙이기, Event에 @Builder 어노테이션 붙이기 => 컴파일 시 자동으로 getter, setter, constructor 만들어줌

```java
class EventTest {
    @Test
    public void builder(){
        Event event = Event.builder()
                .name("Inflearn Spring Rest Api")
                .description("REST API development with Spring")
                .build();
        assertThat(event).isNotNull();
    }
}
  ```


### @Builder @AllArgsConstructor를 쓰는 이유?
+ builder로 빌드하면 모든 인자가 포함된 생성자 만듬. 기본 생성자 만들어주기 위해

### Q. 너무 긴데 애노테이션 못줄이나?
A. 롬복은 메타 애노테이션을 지원 안함

### AssertThat
+ junit 테스트에서 제공하는 assertions 작성에 도움되는 자바 라이브러리
+ `assertThat(actual).isEqualTo(expect)`