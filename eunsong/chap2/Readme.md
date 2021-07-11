# Chap 2

## @RunWith(SpringRunner.class)
- junit에서 service를 테스트하기 위한 어노테이션.
- junit의 기본 러너 대신 사용할 러너를 지정할 수 있음

## @WebMvcTest
- MVC를 위한 테스트
- 웹에서 테스트하기 힘든 컨트롤러를 테스트하는 데 적합
- 웹상에서 요청과 응답에 대해 테스트할 수 있음
- @SpringBootTest 어노테이션보다 가볍게 테스트할 수 있음

## @MockMvc
- 스프링 MVC 테스트 핵심 플래스
- 웹 서버를 띄우지 않고도 스프링 MVC(DispatcherServlet)가 요청을 처리하는 과정ㅇ르 확인할 수 있기 때문에 컨트롤러 테스트용으로 자주 쓰임
  

  ## 응답코드
  + 200
    + **200 OK:성공적으로 처리**
    + 201 Craeted: 요청이 성공적으로 처리돼서 리소스가 만들어짐
    + 202 Accepted: 요청이 받아들여졌지만 처리되지 않았음
  + 400
    + 400 Bad Request: 요청 자체가 잘못됨
    + 401 Unauthorized: 권한 없음
    + **403 Forbidden: 서버가 요청을 거부됨**
    + **404 Not Found: 찾는 리소스가 없음**
  + 500
    + **500 Internal Server Error: 내부 서버 에러**
    + 502 Bad Gateway: 게이트웨이가 연결된 서버로부터 잘못된 응답을 받음

## linktTo(), methodOn()
- linkTo를 통해 Controller.class를 넣으면 Controller에 적용된 RequestMapping()이 들어감
- linkTo(methodOn(Controller.class).method(argument)) 이런식으로 넣으면 해당 API의 uri가 매핑됨
  
---
## 맞닥뜨린 문제
### 1. 한글 깨짐 
```java
mockMvc.perform(post("/api/events/")
                .contentType(MediaType.APPLICATION_PROBLEM_JSON_UTF8)
```
>  APPLICATION_PROBLEM_JSON_UTF8이 지원이 중단됨. 한글이 깨져서 나오는 문제가 발생ㅠ

### 1. 해결 방법
`MockMvc`를 autowired로 빌드하지 말고 @Before에서 직접 `MockMvcBuilders`로 설정하고 `CharacterEncodingFilter`를 추가하면 해결된다.
```java
  @Autowired
    private WebApplicationContext wac;

    private MockMvc mockMvc;
    @Before
    public void setup(){
        this.mockMvc = MockMvcBuilders.webAppContextSetup(wac)
                .addFilters(new CharacterEncodingFilter("UTF-8", true))
                .build();
    }

     mockMvc.perform(post("/api/events/")
                .contentType(MediaType.APPLICATION_JSON)
                .accept(MediaTypes.HAL_JSON)
                .content(new ObjectMapper().registerModule(new Jdk8Module()).writeValueAsString(event)))
                .andDo(print())
                .andExpect(status().isCreated())
                .andExpect(jsonPath("id").exists());
```

### 2. localDateTime이 deserialize, serealize안됨
> 자바 8 이상에서 localD2 ㅌㅇ 

---
## JpaRepository
+ spring framework에서 jpa를 편리하게 사용할 수 있도록 지원하는 프로젝트
+ CRUD 처리를 위한 공통 인터페이스 제공
+ repository 개발 시 인터페이스만 작성하면 실행 시점에 스프링 데이터 jpa가 구현 객체를 동적으로 생성해서 주입
  
## MockBean
+ 테스트를 간단하게 하기 위한 mock bean 객체(빈껍데기 객체)
+ Mockito를 사용해서 mock 객체를 만들고 빈으로 등록해줌
+ (주의) 기존 빈을 테스트용 빈이 대체 한다.