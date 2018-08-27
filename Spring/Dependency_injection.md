# 4. DI(Dependency Injection)

> 대부분의 개요 및 내용은 '스프링 인 액션(크레이그 월즈 저, 제이펍)'을 참조하였으며,
>
> 부족하다고 생각되는 부분은 구글 검색 및 위키 백과를 통해 보완하였다.

* 스프링에서는 `POJO, DI, AOP` 등의 핵심 개념들이 적용되어 있다. 이를 통해 복잡하지 않으면서도 강력한 애플리케이션을 생성할 수 있다.
* 이 곳에서는 스프링의 핵심 개념 및 기능에 대해서만 정리하고 실제 스프링에 어떻게 적용되는지는 추후에 따로 다루도록 한다.

### 4-1. Spring에서의 객체 생성법

* 스프링에서는 앞서 설명한 개념들을 이용하여 기존의 자바와는 다르게 객체를 다룬다. 아래는 기존의 자바 방식이다.

  ```java
  public static void main(String[] args) {
      TV tv = new TV();
      tv.setSpeaker(new Speaker());
      
      tv.turnOn();
      tv.volumeUp();
      tv.volumeDown();
      tv.turnOff();
  }
  ```

* 그렇다면 스프링에서는 객체를 어떻게 다루는지 아래의 코드를 보자.

  ```java
  public static void main(String[] args) {
      ApplicationContext context = new AnnotationConfigApplicationContext(TVConfig.class);
      
      TV tv = context.getBean(TV.class);
      tv.turnOn();
      tv.volumeUp();
      tv.volumeDown();
      tv.turnOff();
  }
  ```

* 스프링에서는 DI, AOP, IoC등을 활용하여 서로 연관성이 없는 독립된 POJO 객체들을 결합하고 실행하여 하나의 애플리케이션으로 만든다.

* POJO가 아닌 객체 중 대표적인 것은 서블릿 객체이다. 서블릿을 구현하려면 아래와 같이 HttpServlet을 반드시 상속 받아야 하며, 자바는 단 하나의 상속만 허용하기 때문에 개발자는 추가 상속 없이 객체를 구현해야 한다. 그리고 서블릿으로 사용할 목적 이외에는 코드의 재사용 또한 불가능하다.

  ```java
  public class ServletExample extends HttpServlet {
      ...
  }
  ```

* 다음은 간단한 POJO 객체의 예제이다. setter, getter 메소드만 구현된 간단한 Value-Object 형태를 띠며 어느 곳에도 의존하지 않은 독립적인 객체이기 때문에 어느 곳에서든 재사용이 가능하다.

  ```java
  public class POJOExample {
      private Foo foo;
      
      public POJOExample() {}
      
      public void setFoo(Foo foo) {
          this.foo = foo;
      }
      
      public Foo getFoo() {
          return foo;
      }
  }
  ```

### 4-2. Spring 설정 파일
* 스프링에서 객체가 어떻게 조립되고 관리되는지 알기 위해서는 먼저 설정 파일에 대해 이해해야 한다.

* 스프링 설정 파일은 크게 두 가지 방식으로 나뉘는데 XML과 Java 클래스이다.

* 먼저 XML 방식에 대해 간략히 알아보자. XML 방식에서는 beans 네임스페이스 내에 bean 등의 태그를 선언함으로써 스프링 빈(객체)을 관리한다.

  * id : 빈의 id 지정
  * class : 빈 객체의 Classpath 지정
  * constructor-arg : 객체의 생성자를 통한 주입(index를 명시하지 않은 경우 순서 중요)
    * ref : 참조하여 주입할 빈 객체의 이름
    * value : 주입할 값
  * property : 객체의 setter 메소드를 통한 주입
    * name : 빈 객체 내에서 변수 이름 ex) setStream()에 stream으로 네이밍된 프로퍼티 주입.

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmln:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
      <bean id="tv" class="com.ch.example.SamsungTv">
          <constructor-arg ref="speaker"/>
          <constructor-arg value="#{T(System).out}"/>
      </bean>
      
      <bean id="speaker" class="com.ch.example.SonySpeaker">
          <property name="stream" value="#{T(System).out}"/>
      </bean>
  </beans>
  ```

* 다음으로는 자바 설정파일에 대해 알아보자. 자바 설정파일은 Annotation을 이용하며 각각의 클래스를 반환하는 메소드에 @Bean 애너테이션을 붙여 빈을 선언하고 다른 빈의 주입이 필요할 경우 생성자에서 해당 빈을 호출해준다.

  ```java
  @Configuration
  public class TVConfig {
      @Bean
      public TV samsungTv() {
          return new SamsungTv(outStream(), sonySpeaker());
      }
      
      @Bean
      public Speaker sonySpeaker() {
          return new SonySpeaker(outStream());
      }
      
      @Bean
      public PrintStream outStream() {
          return System.out;
      }
  }
  ```

* 위와 같은 두 가지의 방식으로 생성된 설정 파일은 런타임 시에 스프링 컨테이너로 생성되어 사용된다.

  ```java
  public static void main(String[] args) {
      ApplicationContext javaCtx = new AnnotationConfigApplicationContext(TVConfig.class);
      ApplicationContext xmlCtx = new GenericXmlApplicationContext("classpath:applicationCTX.xml");
      
      TV tv1 = javaCtx.getBean(TV.class);
      TV tv2 = xmlCtx.getBean(TV.class);
      
      javaCtx.close();
      xmlCtx.close();
  }
  ```

### 4-3. 빈 객체 생성 및 빈 와이어링

* 설정 파일에서 빈을 선언하고 조립하는 과정에 대해 알아보았다. 빈 객체에서는 어떤 작업을 해주어야 하는지 빈 객체와 설정 파일을 효율적으로 연결하는 방법은 무엇인지에 대해 알아보자.

* 빈 객체에는 클래스에 @Component 애너테이션을 붙여 빈 객체임을 표시한다. 아래는 빈 객체 코드이다.

  ```java
  @Component
  public class TV {
      @Autowired
      private Speaker speaker;
      
      public TV(Speaker speaker) {
          this.speaker = speaker;
      }
      
      public void volumeUp() {
          speaker.volumeUp();
      }
      
      public void volumeDown() {
          speaker.volumeDown();
      }
  }
  
  @Component
  public class Speaker {
      @Autowired
      private PrintStream stream;
      
      public Speaker(PrintStream stream) {
          this.stream = stream;
          stream.println("Speaker Created.");
      }
      
      public void volumeUp() {
          stream.println("Volume Up.");
      }
      
      public void volumeDown() {
          stream.println("Volume Down.");
      }
  }
  ```

* 위의 코드에서 @Autowired 애너테이션이 등장하는데 이는 자동 와이어링을 위한 애너테이션이다. 자바 설정 파일이 XML 설정 파일보다 강력한 이유이기도 하다. @Autowired 애너테이션을 빈 객체에서 인스턴스에 붙여두면 설정 파일에서 따로 지정해주지 않아도 적절한 빈을 알아서 주입해준다(적절한 빈이 없거나 여러 개의 빈이 존재하는 경우에는 알맞는 설정이 필요하다). 아래는 자동 와이어링을 사용했을 때 자바 설정 파일의 모습이다.

  ```java
  @Configuration
  @ComponentScan
  public class TVConfig {
      @Bean
      public PrintStream outStream() {
          return System.out;
      }
  }
  ```

* 이전에 자바 설정 파일을 설명할 때 개발자가 생성한 두 개의 빈을 선언하지 않고 @ComponentScan 애너테이션만 붙여두었는데 같은 패키지 안에 있는 두 개의 빈을 자동으로 찾아내서 주입한다. 패키지가 다르더라도 @ComponentScan("패키지명")으로 지정해주면 해당 패키지를 자동으로 탐색한다.

* 물론 XML 설정이 더 알맞은 상황도 존재하겠지만 고유의 기능으로만 따져봤을 때는 자바 설정 파일이 훨씬 간결하고 강력하게 코딩하기에 알맞는 것 같다.
