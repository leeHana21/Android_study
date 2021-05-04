### ViewModel
* viewModel 정의
    * Android Jetpack의 구성요소 중 하나
    * 수명 주기를 고려하여 UI 관련 데이터를 저장하고 관리할 수 있는 Class
    * 아키텍처 패턴인 MVVM에서 활용되며, 구글에서도 권장

* viewModel 장점
    * 화면 회전과 같은 동작에서 view의 인스턴스들의 초기화에도 데이터 유지
    * UI 컨트롤러 로직에서 data를 분리하여 관리 가능
    * 수명주기에 따라 생성되고 소멸 -> 메모리 누수 방지 가능
    * viewModel 공유 가능  -> 여러 activity, fragment에서 데이터 공유 가능
    * view로부터 독립적이기에 코드 유지보수가 수월

* viewModel 생성에 관여하는 Class
    * ViewModelProvider, ViewModelProvider.Factory
        * viewModel의 범위를 제공하는 유틸리티 클래스
        * viewModel 생성, 공급, 저장소에 유지하는 역할
    * ViewModelStore, ViewModelStoreOwner
        * viewModel 인스턴스를 저장하는 역할

* viewModel 생성 프로세스
    1. ViewModelProvider -> viewModel 인스턴스 요청
    2. ViewModelProvider -> ViewModelStoreOwner -> ViewModelStore 가져옴
    3. ViewModelStored에게 viewModel 인스턴스 요쳥
        * 3-1 if(viewModel Instance == null) Factory -> viewModel 생성
    4. 생성된 viewModel 인스턴스 -> ViewModelStore 저장 및 사용자에게 반환
    5. 같은 viewModel 인스턴스 요청 시 1,2,3 과정 반복

### DataBinding
* DataBinding 정의
    * Android Jetpack 라이브러리 중 하나의 기능
    * xml파일에 data 연결하여 사용할 수 있게 하는 기능
    * MVVM 패턴 구현 시 LiveData와 함께 대부분 사용

* DataBinding 장점
    * UI 연결을 위한 findViewById 사용 제거 -> 효율성 증가
    * 자동으로 viewId camelCase로 변경 -> code 상에서 바로 사용 가능
    * data를 통한 view Contents 변환을 xml 상에서 바로 가능
    * view 관련 글루 코드 감소

* DataBinding 설정 방법
    * xml 코드를 <layout> 태그로 전체를 감싼디.
    * dependency 선언
    * binding 객체 선언

* DataBinding 활용 방법
    * <data><variable></variable></data>
        * view에서 사용할 data class, viewModel 설정 가능
    * @{}
        * @{user.firstName}
        * @{age > 13 ? View.GONE : View.VISIBLE}
        * @{()-> viewModel.setText()}
        * 이 외 observer 가능한 data 활용도 가능

### LiveData
* LiveData 정의
    * 식별 가능한 data holder class
    * 수명 주기를 인식 -> 메모리 누수 등의 문제 해결
    * MVVM 패턴에서 거의 필수적으로 사용
    * coroutine과 함께 사용 지원
    * RxJava와 같은 observable 형태로 사용

* LiveData 장점
    * UI의 업데이트 되어야 하는 data 즉시 반영 가능
    * 메모리 누수 위험 방지
    * 비정상 종료 방지 -> 생명주기를 따르기 때문
    * 최신 데이터 유지 가능
    * 리소스 공유 가능 -> LiveData 싱글톤 패턴 객체 확장 사용 가능

* LiveData 사용
    * MutableLiveData / LiveData
        * MutableLiveData -> value의 get/set 모두 가능 -> 값의 변경 및 주입 시 사용
        * LiveData -> 값의 get 만 가능 -> UI에서 값의 변환만 get 할 때 주로 사용

### 참고 사이트
* [viewModel](https://charlezz.medium.com/viewmodel%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-viewmodel-%EC%B4%88%EB%B3%B4%EB%A5%BC-%EC%9C%84%ED%95%9C-%EA%B0%80%EC%9D%B4%EB%93%9C-e1be5dc1ac18)

* [DataBinding 버터나이프 비교](https://gun0912.tistory.com/71)

* [LiveData](https://thdev.tech/android/2021/02/01/LiveData-Intro/)

* [LiveData2](https://pluu.github.io/blog/android/2020/01/25/android-fragment-lifecycle/)