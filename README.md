# 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술

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
  - build & run
    - ./gradlew build
    - java -jar ./build/libs/~~~.jar