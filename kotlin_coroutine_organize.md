
### coroutine 특징
* 비동기적 실행
* 비차단이며 일시중단 함수 활용
* 비동기 코드의 순차적인 진행 가능
* light-weight thread

### coroutine 구현을 위한 3가지
* Job : coroutine의 작업 제어 가능
    * start() -> 현재 coroutine 상태 체크
    * join() -> 현재 coroutine이 끝날 때 까지 대기
    * cancel() -> 현재 coroutine 종료
    * cancelAndJoin() -> 현재 coroutine 종료 / 정상 종료 시 까지 대기 
    * cancelChildren() -> 하위 coroutine들을 종료 / 부모는 취소하지 않음

* Dispatcher : coroutine을 실행하는 Thread 타입 지정
    * Dispatcher.IO -> IO Thread //안드로이드 환경에선 Background thread
    * Dispatcher.Main -> Main Thread에서 동작
    * Dispatcher.default -> Work Thread에서 동작
    * etc

* Scope : coroutine이 실행 되는 범위 
    * runBlocking -> 호출한 위치를 Blocking / 응답이 종료되기 전까지 응답 없음 / 동기화로 작동, UI에서 활용X
    * GlobalScope -> singtone object / 어플리케이션과 동일한 생명주기 가짐/ 종료가 되지 않을 시 실행 지속
    * coroutineScope -> 필요시 선언, 종료 가능 / 안드로이드 환경에서 사용 권장

### other coroutine Builder
* withContext() -> 결과 값을 바로 반환
* async{} -> Deferred return, async블록에서 수행된 결과값 받을 수 있음
    * awit() -> coroutine 블록 완료 후 결과 반환 대기
    * awaitAll() -> 모든 async coroutine 블록이 완료 후 결과 반환 대기
* launch{} -> Job return, 생성된 coroutine의 상태를 관리, 실행 결과 값 return 불가
* channel -> 지정된 크기의 버퍼를 가진 channel 생성 / actor || produce 에서 사용
*  actor<T>{} -> 채널을 통해 전송 및 처리가 가능 / SendChannel<E> 반환 / 수신자
    * send(coroutine Scope)
    * offer(no coroutine Scope)
* produce{} -> 채널을 통한 전송 및 처리가 가능 / ReceiveChannel<E> 반환 / 송신자
* etc

### suspend keyword
* 일시 중단 함수
* suspend 함수로 시작되는 모든 coroutine은 함수가 반환 시 중지
* 현재 스레드를 차단하지 않고 현재 coroutine의 실행을 일시 중단 가능

### 풀어보면 좋을 문제

1. 출력의 순서
    * ANSWER -> 3-1-2-4
```kotlin
fun main() = runBlocking {
    launch { 
        delay(200L)
        println("Task from runBlocking") // 1
    }
    
    coroutineScope {
        launch {
            delay(500L) 
            println("Task from nested launch") // 2
        }
    
        delay(100L)
        println("Task from coroutine scope") // 3
    }
    
    println("Coroutine scope is over") // 4
}
```
2. start() 와 join() / await()의 차이점
    * start() -> non-Blocking fun  / 실행만 시키고 종료를 대기 하지 않음
    * join() / await() -> Blocking fun / 실행 후 종료까지 대기(결과 값은 서로 다름)

3. coroutine Builder 인 actor는 내부에서 일어나는 일을 공유한다 (O / X)
    * 액터 내부에서 일어나는 일은 어느 누구와도 공유 되지 않음
    * Actor는 서로간에 공유하는 자원이 없고, 서로간의 상태에 접근할 수 없음
    * 오직 Message만을 이용해서 상태(정보)를 전달하는 방식
    

4. 차단과 일시중단의 차이점
    * 다른 동작의 가능과 불가능

5. 해당 코드 실행 시 UI Blocking 여부
    * runBlocking 내부의 코드를 IO로 변경했을 뿐이기에 UI는 blocking
``` kotlin
fun load() = runBlocking(Dispatchers.IO) { 
    android.util.Log.w("WHAT", "Thread ${Thread.currentThread()}")
    count++
    delay(500)
    withContext(Dispatchers.Main) { // 2번
        updateView("count $count Thread!!!!!! ${Thread.currentThread()}")
    }
}
```
6. runBlocking()은 async()와 같은 반환 값을 가진다(O / X)
    * runBlocking() -> 실행 결과를 반환, 결과 값을 받고 싶을 경우 coroutine Builder 활용하여 job으로 받음
    * async() -> Deferred 반환 (결과 값 반환)

7. 빈칸 채우기 (구글 제공 문제 중 9번)
    * A [Deferred] is similar to a promise or future in other languages and serves as a placeholder for a return value.

### 해결되지 않은 궁금점
* viewmodel 사용 시 viewModelscope 을 활용할 수 있는데 왜 굳이 또 coroutine 블럭을 사용할까?
* coroutine은 일시 중단 기능을 통해서 연속적인 처리를 가능하게 하기 위해 쓰는 것일까 ?
* 핵심은 thread의 블록처리 or 비동기 작업 처리 ?
* retrofit과 활용이 주되지만 결국 response를 받는 것 자체가 callback인게 아닐까?


### 유익한 참고 사이트
* [Blog](https://thdev.tech/kotlin/2020/12/15/kotlin_effective_15/, "tech blog")

* [Blog](https://jaejong.tistory.com/64?category=876113, "tech Blog2")

* [official](https://www.youtube.com/watch?v=a3agLJQ6vt8, "official video")

* [developerQuiz](https://developer.android.com/courses/quizzes/android-basics-kotlin-unit-4-pathway-1/android-basics-kotlin-unit-4-pathway-1?hl=ko&skip_cache=false, "quiz")
