# Maven



## Build

빌드란 소스코드 파일을 컴퓨터에서 실행할 수 있는 독립 소프트웨어 가공물로 변환하는 과정 또는 그에 대한 결과물을 말한다. 쉽게 풀자면 개발자가 작성한 소스코드, 프로젝트에서 쓰인 각각의 파일 및 자원 등을 JVM이나 톰캣같은 WAS가 인식할 수 있는 구조로 패키징 하는 과정 및 결과물이라고 할 수 있다.



## Build Tool

-   빌드 도구란 프로젝트 생성, 테스트 빌드, 배포 등의 작업을 위한 전용 프로그램이다.

-   빠른 기간동안 계속해서 늘어나는 라이브러리 추가, 프로젝트를 진행하며 라이브러리의 버전 동기화의 어려움을 해소하고자 등장했다.

-   초기의 java 빌드도구로 Ant를 많이 사용하였으나 최근 많은 빌드도구들이 생겨나 Maven이 많이 쓰였고, 현재 Gradle이 많이 쓰인다.

    (Ant는 스크립트 작성도 많고, 라이브러리 의존관리가 되지 않아 불편하다.)



## Maven의 정의 및 특징

-   Maven은 자바용 프로젝트 관리도구로 Apache Ant의 대안으로 만들어졌다.
-   Maven은 Ant와 마찬가지로 프로젝트의 전체적인 라이프 사이클을 관리하는 도구이며, 많은 편리함과 이점이 있어 널리 사용되고 있다.
-   Maven은 필요한 라이브러리를 특정 문서(pom.xml)에 정의해 놓으면 개발자가 사용할 라이브러리 뿐만 아니라 해당 라이브러리가 작동하는데에 필요한 다른 라이브러리까지 관리하여 네트워크를 통해 자동으로 다운받아 준다.
-   간단한 설정을 통한 배포관리가 가능하다.



## Maven LifeCycle

메이븐은 프레임워크이기 때문에 동작 방식이 정해져있고, 미리 정의하고 있는 빌드 순서가 있다. 이를 라이프사이클이라 한다.

-   Default(Build): 일반적인 빌드 프로세스를 위한 모델
-   Clean: 빌드 시 생성되었던 파일들을 삭제하는 단계
-   Validate: 프로젝트가 올바른지 확인하고 필요한 모든 정보를 사용할 수 있는지 확인하는 단계
-   Compile: 프로젝트의 소스코드를 컴파일 하는 단계
-   Test: 유닛 테스트를 수행 하는 단계(테스트 실패시 빌드 실패로 처리, 스킵 가능)
-   package: 실제 컴파일된 소스 코드와 리소스들을 jar, war 등등의 파일 등의 배포를 위한 패키지로 만드는 단계
-   Verify: 통합 테스트 결과에 대한 검사를 실행하여 품질 기준을 충족하는지 확인하는 단계
-   Install: 패키지를 로컬 저장소에 설치하는 단계
-   Site: 프로젝트 문서와 사이트 작성, 생성하는 단계
-   Deploy: 만들어진 package를 원격 저장소에 release하는 단계



최종 빌드 순서는 compile => test => package 이다.

compile : src/main/java 디렉토리 아래의 모든 소스 코드가 컴파일된다.

test : src/test/java, src/test/resources 테스트 자원 복사 및 테스트 소스 코드 컴파일 (junit: 단위 테스트 프레임워크. 테스트 단계를 거치기 위해 의존 설정을 해준다.)

packaging: 컴파일과 테스트가 완료 된 후, jar, war 같은 형태로 압축하는 작업.



## Phase(단계)

Build Lifecycle의 각각의 단계를 Phase라고 한다. Phase는 의존관계를 가지고 있어 해당 Phase가 수행되려면 이전 단계의 Phase가 모두 수행되어야 한다. 즉, 모든 빌드단계는 이전 단계가 성공적으로 실행되었을 때 실행된다는 것이 Dependency이다.



## Goal

-   특정 작업, 최소한의 실행 단위

-   하나의 플러그인에서는 여러 작업을 수행할 수 있도록 지원하며, 플러그인에서 실행할 수 있는 각각의 기능(명령)을 Goal이라고 한다. (각각의 Phase에 연계된 Goal을 실행하는 과정을 Build라고 한다.)

-   플러그인의 goal을 실행하는 방법은 다음과 같다.

    ``` xml
    -mvn groupid:artifactid:version:goal(아래와 같이 생략 가능)
    -mvn plugin:goal
    ```





## Maven 설정파일

1.  settings.xml

    -   메이븐 빌드 툴과 관련한 설정파일
    -   MAVEN_HOME/conf 디렉토리에 위치 (메이븐 설치 시 기본 제공)
    -   settings.xml의 설정

    메이븐을 빌드할 때 의존 관계에 있는 라이브러리, 플러그인을 중앙 저장소에서 개발자 PC로 다운로드 하는 위치(로컬 저장소)의 기본 설정 'USER_HOME/.m/repository' 인데 settings.xml에 원하는 로컬 저장소의 경로를 지정, 변경할 수 있다.

    

2.  POM(Projecf Object Model)

    -   POM은 pom.xml파일을 말하며 pom.xml은 메이븐을 이용하는 프로젝트의 root에 존재하는 xml파일이다.

        (하나의 자바 프로젝트에 빌드 툴을 maven으로 설정하면, 프로젝트 최상위 디렉토리에 "pom.xml"이라는 파일이 생성된다.)

    -   Maven의 기능을 이용하기 위해 POM이 사용된다.

    -   파일은 프로젝트마다 1개이며, pom.xml만 보면 프로젝트의 모든 설정, 의존성 등을 알 수 있다.

    -   다른 파일이름으로 지정할 수도 있다. 하지만 pom.xml으로 사용하기를 권장한다.

    **엘리먼트**

    -   modelVersion: POM model의 버전

    -   parent: 프로젝트의 계층정보

    -   groupid: 프로젝트를 생성하는 조직의 고유 아이디를 결정한다. 일반적으로 도메인 이름을 거꾸로 적는다.

    -   artifactid: 프로젝트 빌드시 파일 대표이름이다. groupid내에서 유일해야 한다. Maven을 이용하여 빌드시 다음과 같은 규칙으로 파일이 생성된다.

        artifictid-version.packaging. 위 예의 경우 빌드할 경우 bo-0.0.1-SNAPSHOT.war 파일이 생성성된다.

    -   version: 프로젝트의 현재 버전, 프로젝트 개발 중일 때는 SNAPSHOT을 접미사로 사용.

    -   packaging: 패키징 유형(jar, war, ear등)

    -   name: 프로젝트, 프로젝트 이름

    -   description: 프로젝트에 대한 간략한 설명

    -   url: 프로젝트에 대한 참고 Reference 사이트

    -   properties: 버전관리시 용이하다.

    -   dependencies: dependencies태그 안에는 프로젝트와 의존 관계에 있는 라이브러리들을 관리한다.

    -   build: 빌드에 사용할 플러그인 목록






