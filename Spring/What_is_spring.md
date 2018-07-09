# 1. Spring이란?

### 1-1. 프레임워크의 개념

* 프레임워크란 프로그램을 구성하는 가장 기초적인 기반 코드와 이 기반 코드를 확장시켜 나갈 수 있는 라이브러리가 통합되어 개발자에게 제공되는 것이다.
* 개발자는 프레임워크의 규칙을 준수하고 프레임워크에 의존하여 개발함으로써 프레임워크를 활용하여 빠르게 원하는 기능을 개발할 수 있다.

### 1-2. Spring

* 자바 플랫폼을 기반으로 한 오픈소스 프레임워크이다.
* [`POJO(Plain Old Java Object)`](https://ko.wikipedia.org/wiki/Plain_Old_Java_Object) 방식의 프레임워크이고 기존의 [J2EE](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%ED%94%8C%EB%9E%AB%ED%8F%BC,_%EC%97%94%ED%84%B0%ED%94%84%EB%9D%BC%EC%9D%B4%EC%A6%88_%EC%97%90%EB%94%94%EC%85%98) 기반 프레임워크에 비해 코드의 경량화 및 기존 라이브러리 지원에 용이한 것이 특징이다.
* [IoC](https://ko.wikipedia.org/wiki/%EC%A0%9C%EC%96%B4_%EB%B0%98%EC%A0%84), [DI](https://ko.wikipedia.org/wiki/%EC%9D%98%EC%A1%B4%EC%84%B1_%EC%A3%BC%EC%9E%85), [AOP](https://ko.wikipedia.org/wiki/%EA%B4%80%EC%A0%90_%EC%A7%80%ED%96%A5_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)를 지원하여 효율적인 프로그래밍을 가능하게 한다.
* 국내에서는 자바 개발자들의 표준 프레임워크로 통용되고 있으며 한국 전자정부 표준프레임워크의 기반 기술이기도 하다.
* 스프링의 학습을 위해서는 Java, JSP & Servlet 등에 대한 기반 지식을 전제로 한다.

### 1-3. Spring 개발환경 구축

* Java JDK 설치
* IntelliJ 혹은 Eclipse 설치(Eclipse의 경우 STS 플러그인 필요)
* tomcat 설치
* 설치과정은 생략하며 IDE 환경 학습도 병행하기 위해 IntelliJ와 Eclipse를 번갈아가며 사용한다.