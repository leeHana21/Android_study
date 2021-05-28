### flavor(projectFlavor)
* 정의
    * 빌드 변형 구성 -> 하나의 소스로 여러 버전의 앱 제작을 가능하게 하기 위함(ex. 상용 배포시의 firebase를 변경하고 싶을 때)
* 사용 방법
    * build.gradle(Module:app)에 명시
* 사용 이유
    * 버전 별 관리가 쉽고 각 flavor의 realese/debug 생성으로 유지보수 수월
* flavor 속성 정리
    * flavorDimensions -> 빌드의 구분을 나타냄
    * applicationIdSuffix -> defalutConfig에 명시된 applicationId 뒤에 붙음 -> 이걸로 각 앱 구분 가능
    * buildConfigField -> BuildConfig라는 클래스에서 호출할 수 있는 값
    * manifestPlaceholders -> AndroidManifest.xml 파일에서 ${appLabel}처럼 사용 가능 (android:label="${appLabel}")
    * resValue -> 문자열 정의
* code
    ``` xml
    flavorDimensions "version"
    productFlavors {
        lee {
            dimension "version"
            manifestPlaceholders = [
                    appLabel: "lee-Flavor"
            ]
            applicationIdSuffix ".lee"
            versionCode 10000 // 쓰지 않을 경우 default config 정보 따라감
            versionName "1.0.0" // 쓰지 않을 경우 default config 정보 따라감
            buildConfigField "String", "ConfigField", "\"This is lee-FLAVOR application\""
            resValue "string", "appName", "lee-Flavor"
        }
        paid {
            dimension "version"
            manifestPlaceholders = [
                    appLabel: "hana-Flavor"
            ]
            applicationIdSuffix ".hana"
            versionCode 20000
            versionName "2.0.0"
            buildConfigField "String", "ConfigField", "\"This is hana-FLAVOR application\""
            resValue "string", "appName", "hana-Flavor"
        }
    }
    ```
* 상단의 code를 이용할 경우
    * build Variants 에서 총 4개의 카테고리 확인 가능(leeRelease / leeDebug / hanaRelease / hanaDebug)
    * 기존 defalut 값은 생략됨
    * 직접 해보았을 때 각 flavor 안에 dimension 작성은 생략해도 무방