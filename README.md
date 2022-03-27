# [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)

## 프로젝트 환경설정

- [스프링 이니셜라이저](https://start.spring.io/) 에서 각종 설정을 하면 프로젝트 세팅을 해준다.
    - 설정하는 것
        - maven, gradle 디펜던시 관리, 빌드, 번들링 등 전체적인 걸 봐주는 툴인듯. 요즘은 gradle
        - 스프링부트 버전
        - 프로젝트 메타데이터들
        - 당겨 쓸 디펜던시
    - build.gradle
        - plugins, versions, 등등 그레이들이 관리하는 프로젝트 컨피그?
        - repositories = mavenCentral 에서 디펜던시들 내려받아라
        - 뭐 대충 ~
    - 뷰 환경 설정
        - `@Controller` 어노테이션을 붙이면 컨트롤러 클래스가 생성되고 스프링에 등록됨
        - 컨트롤러 클래스의 퍼블릭 메소드에 엔드포인트와 메소드를 매핑해서 각 요청을 핸들링 할 수 있다.
        - 각 메소드는 Model 의존성을 주입받아서 attribute를 추가할 수 있다.
        - 컨트롤러가 문자열을 리턴하면 viewResolver 가 이름에 맞는 html을 찾고 마찬가지로 Model 주입해주는 것 같다.
        - 근데 이 일련의 과정을 하는게 스프링 '부트' 라고 하시는디 그냥 오토 컨피그 말고 다른게 있는건가? 모르겠다.
        - ㅇㅎ... 앞에 내장 톰캣 서버가 붙고 뒤에 스프링 컨테이너가 있는 이 전체 그림을 스프링 부트가 제공해주는듯...!
        - 뷰 리졸버는 스프링에 있음
    - build & run
        - ./gradlew build
        - java -jar ./build/libs/~~~.jar

## 스프링 웹 개발 기초

- 정적 컨텐츠
    - 요청이 들어온다.
    - 내장 톰캣 서버가 스프링 컨테이너한테 컨트롤러 있는지 물어본다 ( 컨트롤러가 우선순위를 가진다는 뜻 )
    - 없으면 톰캣서버가 resource: static/hello-static.html 을 찾아서 내려준다
- MVC와 템플릿 엔진
    - 뷰 환경설정에서 한것과 똑같음.
    - 장고가 새록새록...
- API
    - 컨트롤러 메소드에 `@ResponseBody` 를 붙이면 http body에 내용을 직접 반환함.
    - 이럴 경우에는 viewResolver 대신 HttpMessageConverter가 동작.

## 회원 관리 예제

- 일반적인 아키텍쳐
    - ![arichtecture](./arichtecture.png)
    - 나중에 좀 더 보자
- controller
    - 는 안했다.
- domain
    - 회원 정보와 이름을 가지고, 게터/세터만 있는 Member 클래스 생성
- repository
    - Member 를 save, find 하는 MemberRepository 인터페이스 생성. (DB를 정하지 않았다는 가정이 있어서.)
    - 일단 사용할 메모리에 저장하는 형태로 MemeoryRespository를 구현한 MemoryMemberRepository 생성
- service
    - 비즈니스 로직이 담겨있음.
    - 예제 기준) 내부적으로 MemberRepository를 사용? 하는데 이떄 service 에서 인스턴스를 생성하는 형태가 되면 ( 왜안좋지...? 까먹음 아무튼 ) 안되니 외부에서 생성자를 통해 주입받는
      형태로 작성. DI 의존성주입
    - MemberRepository를 MemberService에서 뿐만 아니라 다른 곳에서도 사용할 경우 그때마다 새 인스턴스를 생성하기때문에 문제가 발생할 수 있다.

## 스프링 빈과 의존관계

- 스프링 어플리케이션이 뜰때 `@Component`가 달린 클래스는 싱글톤으로 생성해서 빈 에서 의존성 관리를 해준다.
- `@Controller`, `@Repository` 등의 어노테이션은 `@Component`를 포함하기 떄문에 모두 등록된다.
- DI를 받는 클래스의 생성자에 `@Autowired` 어노테이션을 달면 스프링이 주입해준다.
- 만약 이를 수동으로 하고싶다면 Configuration 하는 클래스를 만들어줘야한다.
- `@Configuration` 어노테이션을 달고, 관리해야하는 디펜던시를 리턴하는 퍼블릭 메소드를 만들고 `@Bean` 어노테이션을 달면된다.
- 자동으로 하는게 편하긴 한데, 구현체를 바꾸고 싶은 경우에는 매뉴얼로 하는게 낫다


## 회원관리 예제 MVC 개발

- 게터/세터 설정된 클래스를, 컨트롤러의 메소드의 인자로 설정해두면 스프링이 요청 값을 매핑해서 넣어준다

## 스프링 DB 접근 기술

- 스프링 통합 테스트
  - 테스트에 `SpringBootTest`
  - `@Transactional` 붙이면 커밋 안하니까 테스트하기 편함
  - DI 받아야함.
- jdbc
  - 직접 커넥션 만들어서 관리하는 형태
- jdbc template
  - template method 패턴 이용한 jdbc 인터페이스
- jpa
  - orm
  - entitiy manager 사용함
  - 도메인 모델에 `@Entity`
  - `@Transactional` 붙은 스코프? 에서만 돌아야 함
  - `@Configuration` 클래스도 스프링 컨테이너가 관리하니까 DI 받을 수 있음. EntityManager 주입받아서 repository에 주입.
- spring data jpa
  - jpa를 또 래핑한 인터페이스
  - 기본적인 findById, findAll 등 공통화 가능한 녀석들 다 만들어둠
  - 인터페이스만 만들어도 알아서 뚝딱 해줌
  
## AOP

- cross cutting concern과 core concern의 분리
- 스프링에서는 `@Aspect` 로 지원
- `@Component`로 컴포넌트 스캔으로 등록해도 되고
- `@Configuration`에서 직접 등록해도 되는데, 이때에는 `@Around`에서 `@Configuration` 클래스를 대상으로 하지 않는지 확인하고, 만약 대상으로 한다면 제거해줘야 순환참조가 발생하지 않는다.

## 자바 볼것

- final
- package?
- MemberRepository 를 주입받는다고 해뒀는데 어떻게 MemoryMemberRepository 를 주입받은겨..?
- lambda, hashmap, map. list, optional, stream
