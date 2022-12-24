### 프로젝트 폴더 생성

- `gradle init`: gradle 프로젝터 생성 명령어

``` java
$ gradle init

Select type of project to generate:
  1: basic
  2: application
  3: library
  4: Gradle plugin
Enter selection (default: basic) [1..4] 2

Select implementation language:
  1: C++
  2: Groovy
  3: Java
  4: Kotlin
  5: Scala
  6: Swift
Enter selection (default: Java) [1..6] 3

Select build script DSL:
  1: Groovy
  2: Kotlin
Enter selection (default: Groovy) [1..2] 1

Select test framework:
  1: JUnit 4
  2: TestNG
  3: Spock
  4: JUnit Jupiter
Enter selection (default: JUnit 4) [1..4]

Project name (default: demo):
Source package (default: demo):


BUILD SUCCESSFUL
2 actionable tasks: 2 executed
```



- 초기 구조

``` java
├── gradle // wrapper file 폴더, wrapper파일은 gradle 실행 파일을 미리 다운 받아둔 것 
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew // gradle wrapper 실행 스크립트 (1)
├── gradlew.bat // gradle wrapper 실행 스크립트 (2)
├── settings.gradle // 빌드명, 서브프로젝트 정의 파일
└── app
    ├── build.gradle // app 프로젝트 빌드 스크립트
    └── src
        ├── main
        │   └── java 
        │       └── demo
        │           └── App.java
        └── test
            └── java 
                └── demo
                    └── AppTest.java
```



### 프로젝트 파일 살펴보기



- settings.gradle

```groovy
// 실제 사용 파일
pluginManagement {
    repositories {
        maven { url ('https://repository주소')}
        gradlePluginPortal()
    }
}

rootProject.name = '빌드명'

// 원래는 include('app명')
includeProject(":lib-core", "src/library/lib-core")
includeProject(":lib-infrastructure", "src/library/lib-infrastructure")
includeProject(":bootapp-api", "src/bootapp-api")



def includeProject(String name, String projectPath) {
    include(name)
    project(name).setProjectDir(file(projectPath))
}
```



- app/build.gradle

```groovy
ext {
    변수명 = 값 // 스크립트에서 사용할 변수 만들기
}

// Java CLI application 빌드를 돕는 plugin을 추가한 것
plugins {
    id 'application' 
}

// maven Central 레포 추가 (의존성을 가져오는 곳)
repositories {
    mavenCentral() 
}

// 의존성 정의
dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter:5.9.1' // 테스트용 

    implementation 'com.google.guava:guava:31.1-jre' // app용
}

// application의 main 클래스 정의
application {
    mainClass = 'demo.App' 
}


tasks.named('test') {
    useJUnitPlatform() 
}

// jar 파일 생성을 막는 것
jar {
    enabled = false
}
```



### Build Scan

- `./gradlew build --scan`
  - https://scans.gradle.com/?sdf.1674134935.1671856616-1853008599.1671163459