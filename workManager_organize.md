### WorkManager
* 정의
    * Android JetPack 아키텍처의 구성요소 중 하나
    * 안드로이드의 백그라운드 작업을 처리하는 방법 중 하나
* 장점
    * 알람 매니저를 대체해 안정적으로 백그라운드 태스크 처리 가능
    * 하나의 코드로 다른 API Level마다 비슷한 동작을 보장
* 주요 기능
    * 실행해야하는 작업에 대한 제약조건을 걸 수 있다(ex. 네트워크 가용성, 배터리 충전 중 인지)
    * 일회성, 주기성 작업 예약 가능
    * 작업 체이닝이 가능
    * 앱이나 기기의 리부팅시에도 작업 실행을 보장
* 특징
    * 지연이 가능한 작업 및 안정적으로 실행이 되어야 하는 작업을 대상으로 해야한다
* WorkManager 주요 class
    * WorkManager
        * 처리가 필요한 작업을 자신의 큐에 넣어 관리
        * 싱글톤으로 구현되어있기에 getInstance()를 통해 인스턴스를 받아서 상속
    * Worker
        * 추상 클래스 / 처리해야하는 백그라운드 작업의 처리 코드를 작성하여 사용
        * doWork() 메서드를 오버라이드 하여 처리해야하는 코드 작업 작성
        * SUCCESS, FAILURE, RETRY 의 리턴 값 가짐 / 값에 따라 다른 로직으로의 정의 가능
    * WorkRequest
        * workManager를 통해 실제 요청하게 될 개별 작업
        * 실행 조건, 제약 사항 등 작업 처리에 대한 정보를 담음
        * 반복 여부에 따라 oneTimeWorkRequest / PeriodicWorkRequest
    * WorkState
        * workRequest의 id와 현재 작업의 진행 상태를 담는 클래스
        * ENQUEUED, RUNNING, SUCCEEDED, FAILED, BLOCKED, CANCLLED의 6개 상태를 가짐
        * LiveData 형태로 작업 상태 파악 후 추가 처리 요청을 가능하게 함

* 특정 시간에 계속 반복되는 죽지 않는 work 만들기
    ``` kotlin
    // 백그라운드 작업이 필요한 내용(doWork()에 정의)
    override fun doWork(): Result {
            val initWorkRequest: WorkRequest = OneTimeWorkRequest.Builder(TestTimeNotificationWorker::class.java)
                .setInitialDelay(getSettingTime(), TimeUnit.MILLISECONDS).addTag("noti_day_by_day").build()
            WorkManager.getInstance(applicationContext).enqueue(initWorkRequest)

        return Result.success()
    }
    // workManager에 작업을 요청()
    private void initTimeToTestWorker(){
            WorkRequest initWorkRequest = new OneTimeWorkRequest.Builder(TestTimeNotificationWorker.class).setInitialDelay(TimeCountUtil.Companion.getSettingTime(), TimeUnit.MILLISECONDS).addTag("noti_day_by_day").build();
            WorkManager.getInstance(this).enqueue(initWorkRequest);
    }
    // workState 파악하기
    private fun workState(){
        var resetDayByDayWorkerRequest : OneTimeWorkRequest // 설명을 위해 함수 안에 추가 원래는 상단에 전역으로 선언
            WorkManager.getInstance(applicationContext).getWorkInfoByIdLiveData(initWorkRequest.id)
                .observe(this, Observer { happen -> happen?.let {
                    if (App.prefs.createdSVisitor || App.prefs.createdVisitor){
                        startService()
                    }else {
                        when(it.state){
                            WorkInfo.State.SUCCEEDED ->{
                                Toast.makeText(applicationContext,getString(R.string.noti_daily),Toast.LENGTH_LONG).show()
                                onBackPressed()
                            }
                            else -> {
                                Toast.makeText(applicationContext,getString(R.string.init_your_info),Toast.LENGTH_LONG).show()
                            }
                        }
                    }
                }
            })
        }
    // 시간 계산 함수
    fun getSettingTime() : Long {
            val currentDate = Calendar.getInstance()
            val dueDate = Calendar.getInstance()

            dueDate.set(Calendar.HOUR_OF_DAY, 21)
            dueDate.set(Calendar.MINUTE, 0)
            dueDate.set(Calendar.SECOND, 0)

            if (dueDate.before(currentDate)){
                dueDate.add(Calendar.HOUR_OF_DAY, 24)
            }
            return dueDate.timeInMillis - currentDate.timeInMillis
    }
    ```

* 특정시간 반복에 왜 PeriodicWorkRequest를 사용하지 않나
    * 10분 / 20분 과 같은 시간으로만 지정이 가능하고, 매일 매일 반복은 안되기 때문

* 참고 사이트
    [WorkManager 내용 정리](https://dongsik93.github.io/til/2020/05/15/til-jetpack-workmanager/)
    [WorkManager 매일 반복 작업](https://zladnrms.tistory.com/157)