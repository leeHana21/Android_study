* Android Test
* 정의
    * 앱 내 구성요소(화면 / 비즈니스 로직)들의 동작을 점검하기 위함
    * make high-quality and errorless code

* 테스트 하기 좋은 코드
    * 모듈 측면으로 개발한 코드 -> 각 기능 별로 테스트 할 수 있기 때문
    * 모듈끼리의 경계를 잘 정의하고, 모듈 간 통신 하는 api가 일관된 코드
* androidTest / test
    * androidTest (계측 테스트)
        * 실제 또는 가상 기기에서 실행되는 테스트가 포함
        * jvm 만으로 앱 기능의 유효성을 검사할 수 없는 기타 테스트
        * 대부분 ui 테스트

    * test (단위 테스트)
        * 로컬 시스템에서 실행되는 테스트
        * 비즈니스 로직 테스트에 사용

    * Test의 중요도 (Google 권장) 및 종류
        * 단위 테스트 > 통합 테스트 > 엔드 투 엔드 테스트 
        
        * 소형 테스트 -> 한번에 한 클래스 씩 앱 동작의 유효성 검사
        * 중형 테스트 -> 관련 모듈 간 상호작용의 유효성을 검사
        * 대형 테스트 -> 앱의 여러 모듈에 걸친 실 사용자 여정의 유효성을 검사

        * AndroidX 테스트 -> Android frameWkork와 상호 작용하는 코드를 테스트 하는 새로운 방법 Robolectric으로 실기기 or 에뮬, jvm에서 실행
    * 프레임 워크
        * Roblectric -> 안드로이드 api를 이용하면서 데스크톱 jvm에서 실행할 수 있는 프레임 워크
            * espresso만 가능하던 액티비티 / view 접근 지원
            * performClick(), assertThat() 액션 및 검증 지원
            * src/test/java/패키지명에 테스트 작성
        * Espresso -> 사용자 인터페이스 테스트 지원하는 Android 용 테스트 프레임 워크
        * jUnit -> 안드로이드 스튜디오에 이미 기본적으로 포함되어있는 단위 테스트 프레임 워크
        * Mockito -> 객체를 Mocking하는데 사용되는 Java 기반의 라이브러리 Junit과 함께  unitTest에 사용 됨
    * 궁금한 점
        * 실제 기기의 블루투스 scan, advertising 와 같은 부분도 가능한 방법이 있는지
        * 모듈화를 못하는 코드 (viewHolder 코드 등)는 어떤 식으로 테스트가 가능한지
    
* 참고 사이트
   * [androidTest / test 차이](https://stackoverflow.com/questions/34397524/whats-the-difference-between-src-androidtest-and-src-test-folders)
   * [AndroidX Test](https://medium.com/google-developer-experts/pushing-the-limits-of-androidx-test-3776ff249c71)
   * [Android Test- Robolectric](https://readystory.tistory.com/category/Android/Test)
   * [Test Code의 범위 - 뱅크샐러드 블로그](https://blog.banksalad.com/tech/test-in-banksalad-android/)
   * [Android 내장 test가 아닌 Android test - 배민 블로그](https://woowabros.github.io/experience/2020/01/06/mobile-app-test-with-appium.html)
