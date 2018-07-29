# 3. Spring 프로젝트 생성

* IntelliJ와 이클립스 두 개의 IDE 환경에서의 스프링 조작법을 익히기 위해 두 가지 방법 모두 알아보고자 한다.
* 모든 내용은 해당 IDE 설치 및 자바 환경설정이 되어 있다는 가정하에 간단한 프로젝트 생성만을 알아본다.

### 3-1. IntelliJ

- IntelliJ에서는 추가적인 작업 없이 기본적으로 스프링을 지원한다.

1. IntelliJ에서 새 maven 프로젝트를 생성

   ![](https://github.com/ChanHoYe/TIL/blob/master/Spring/img/intellij_spring_1.PNG?raw=true)

2. 적당한 GroupId와 ArtifactId를 설정 후 Next

   ![](https://github.com/ChanHoYe/TIL/blob/master/Spring/img/intellij_spring_2.PNG?raw=true)

3. 프로젝트명과 경로 확인 후 Finish

   ![](https://github.com/ChanHoYe/TIL/blob/master/Spring/img/intellij_spring_3.PNG?raw=true)

4. 우측 하단 Import 관련 창에 [Enable Auto-Import] 선택

   ![](https://github.com/ChanHoYe/TIL/blob/master/Spring/img/intellij_spring_4.PNG?raw=true)

5. pom.xml에 스프링 관련 dependency 설정

   ![](https://github.com/ChanHoYe/TIL/blob/master/Spring/img/intellij_spring_5.PNG?raw=true)

6. 간단한 예제 코드 실행

### 3-2. Eclipse

> 이클립스는 Java 개발자용 이클립스가 아닌 JavaEE 개발자용 이클립스가 필요

1. `STS`(Spring Tool Suite) 설치

   1. [Help - Eclipse Marketplace]로 진입한다.

   ![](https://github.com/ChanHoYe/TIL/blob/master/Spring/img/sts_1.png?raw=true)

   2. STS 플러그인을 검색

   ![](https://github.com/ChanHoYe/TIL/blob/master/Spring/img/sts_2.PNG?raw=true)

   3. 기능을 모두 선택한 후 Confirm

   ![](https://github.com/ChanHoYe/TIL/blob/master/Spring/img/sts_3.PNG?raw=true)

   4. 라이센스 동의 후 Confirm

   ![](https://github.com/ChanHoYe/TIL/blob/master/Spring/img/sts_4.PNG?raw=true)

   5. 설치가 완료되면 이클립스 재시작

   ![](https://github.com/ChanHoYe/TIL/blob/master/Spring/img/sts_5.PNG?raw=true)

2. 프로젝트 생성

   1. [File - New - Other]로 진입한 뒤 [Spring - Spring Legacy Project] 선택

   ![](https://github.com/ChanHoYe/TIL/blob/master/Spring/img/eclipse_spring_1.png?raw=true)

   ![](https://github.com/ChanHoYe/TIL/blob/master/Spring/img/eclipse_spring_2.PNG?raw=true)

   2. 프로젝트 명을 기입하고 Simple Spring Maven 템플릿 선택 후 Finish

   ![](https://github.com/ChanHoYe/TIL/blob/master/Spring/img/eclipse_spring_3.PNG?raw=true)

   3. 간단한 예제 코드 실행

#### - 두 가지의 IDE 모두 사용해 본 결과 널리 쓰이는 이클립스보단 개인적으로 IntelliJ가 훨씬 간편하게 스프링을 지원하는 느낌을 받았다. 이클립스에 익숙해져 있지만 IntelliJ 기반으로 코딩하는 방법을 익힌다면 훨씬 더 간편하고 강력한 지원이 가능할 것 같다.
