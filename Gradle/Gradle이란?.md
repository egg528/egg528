### Gradle이란?

- Gradle은 거의 모든 SW에 적용할 수 있는 `빌드 자동화` 도구이다 (오픈 소스).
- Gradle은 당신이 무엇을 빌드 하는지, 어떻게 빌드하는지에 거의 관여하지 않는다.
  - 이런 점 때문에 Gradle은 유연하게 적용될 수 있다.



### Design Fundamentals (설계 원칙)

- High Performance
  - Input, Output이 바뀌는. 즉, 꼭 필요한 부분만 처리하려고 한다.
  - 이를 위해 다양한 캐시를 사용함.
- JVM Foundation
  - gradle은 JVM 위에서 실행된다.
  - 즉, build 로직이 Java 표준 API를 이용하기에 Java유저에게 친숙하다.
- Conventions
  - 정형화된 사용법으로 build를 쉽게 한다.
  - 하지만 원한다면 custom이 얼마든지 가능하다.
- Extensibility
  - 원한다면 custom task나 plugin을 추가할 수 있다.
- Insight
  - Build Scan이 실행된 build에 대한 정보들을 제공해준다.
  - 이를 바탕으로 디버깅이나 성능 개선이 가능하다

