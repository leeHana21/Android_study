### Dependency Injection(DI)
* DI 정의
    * 제어 역전(Inverse Of Control)을 통해 객체끼리의 의존 관계를 분리시키는 디자인 패턴
    * 내부가 아닌 외부에서 객체를 생성해서 넣어준다

* 활용 방법
    * 생성자 주입(Constructor Injection) : 생성자를 통해 의존하는 객체를 전달
    * 필드 주입 혹은 세터 주입(field or setter injection)

* 구현 방법
    * 직접 Provider를 구성해 코드로 작성
    * 라이브러리를 통해 처리 하는 방법

* DI 장점
    * 의존성 파라미터를 생성자에 작성하지 않음 -> 보일러 플레이트 코드 감소
    * 인터페이스 구현체를 쉽게 교체 -> 상황에 따라 적절한 코드 정의 가능
    * 코드 재사용 가능
    * 코드 리팩토링 편의성 상승
    * 테스트 편의성 상승

* 상용 프레임 워크
    * Dagger / Dagger2
    * Hilt
    * Koin
### Dagger
* 특징
    * 러닝 커브 높음
    * Inject
        * 어노테이션을 활용해 주입 요청
    * Component
        * 연결된 module을 이용해 의존성 객체 생성, 요청받은 인스턴스에 객체 주입
    * Subcomponent
        * 계층 관계를 생성할 수 있음
        * 주입 요청을 받을 경우 본인에서 먼저 검색, 없을 경우 부모로 올라가 검색 실행
    * Module
        * Component에 연결되어 의존성 객체 생성, scope에 따른 관리 실행
    * Scope
        * 생성된 객체의 lifecycle 범위
    * 에러 검출 시점 : 컴파일 타임
    * Java / Kotlin 혼용 가능

### Hilt
* 특징
    * 러닝커브 낮음
    * Dagger를 기반으로 만들어진 프레임 워크
    * Dagger2와 비슷한 동작 방식
    * 에러 검출 시점 : 컴파일 타임
    * 아직은 beta 버전
    * Java / Kotlin 혼용 가능
    * Jetpack DI 권장 라이브러리

### Koin
* 특징
    * 러닝커브 낮음
    * 코틀린 개발 환경에 쉽게 적용 가능한 경량화된 프레임워크
    * 에러 검출 시점 : 런타임
    * DSL KeyWord 
        * module
            * Koin모듈을 정의할때 사용
        * factory
            * inject하는 시점에 해당 객체 생성이 이용
        * single
            * 앱이 살아있는 동안 전역적으로 사용 가능한 객체를 생성
        * bind
            * 생성할 객체를 다른 타입으로 바인딩하고 싶을 때 이용
        * get
            * 주입할 각 component끼리의 의존성을 해결하기 위해 사용

### 의문점
* MVVM 패턴으로 구현 시 viewmodel을 의존성 주입할 경우 viewModel은 라이프 사이클을 따르지 않는건가 ?

### 참고 사이트
[DI 내용 정리](https://salix97.tistory.com/264)
[DI 프레임 워크 비교](https://velog.io/@sysout-achieve/Android-DI-Framework-%EC%84%A0%ED%83%9D%EC%A7%80Dagger2-Koin-Hilt)