# 4. DI(Dependency Injection)

> 대부분의 개요 및 내용은 '스프링 인 액션(크레이그 월즈 저, 제이펍)'을 참조하였으며,
>
> 부족하다고 생각되는 부분은 구글 검색 및 위키 백과를 통해 보완하였다.

* 스프링에서는 `POJO, DI, AOP` 등의 핵심 개념들이 적용되어 있다. 이를 통해 복잡하지 않으면서도 강력한 애플리케이션을 생성할 수 있다.
* 이 곳에서는 스프링의 핵심 개념 및 기능에 대해서만 정리하고 실제 스프링에 어떻게 적용되는지는 추후에 따로 다루도록 한다.

### 2-1. POJO

* [`POJO(Plain Old Java Object)`](https://ko.wikipedia.org/wiki/Plain_Old_Java_Object)란 '보통의(평범한) 자바 객체'를 뜻하는 말로 자바의 기본 사양 이외에 다른 것을 사용(상속, 구현)하지 않는 깨끗한 객체를 뜻한다.

* 스프링에서는 EJB 등의 프레임워크와 달리 프레임워크에서 사용될 bean 객체에 상속, 구현, 애너테이션을 강제하지 않는다. 다만, 종종 bean의 사용 목적에 따라 애너테이션이 요구되기도 한다.

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

### 2-2. IoC(Inversion of Control)
* [`IoC(Inversion of Control)`](https://ko.wikipedia.org/wiki/%EC%A0%9C%EC%96%B4_%EB%B0%98%EC%A0%84)는 스프링에서 적용되는 개념들을 가능하게 해주는 가장 핵심적인 개념이다. 애플리케이션 흐름의 제어권을 애플리케이션이 아닌 애플리케이션 외부의 프레임워크가 가짐으로써 프레임워크에서 객체들을 제어하고 DI, AOP 등을 이용한 프로그래밍을 가능하게 한다.
* 스프링에서는 컴포넌트를 관리하는 컨테이너를 통해 각 컴포넌트에 의존성을 주입하고 생명 주기를 관리한다. 이용자가 컨테이너를 실행시키면 애플리케이션의 제어권은 이용자가 아닌 컨테이너로 위임되며 컨테이너가 컴포넌트들을 실행하고 관리한다.

### 2-3. DI(Dependency Injection)

* 스프링에서 가장 핵심적인 개념인 [`DI(Dependency Injection)`](https://ko.wikipedia.org/wiki/%EC%9D%98%EC%A1%B4%EC%84%B1_%EC%A3%BC%EC%9E%85)는 객체의 내부에서 객체 간 의존성을 정의하지 않고 외부의 프레임워크에서 객체에 의존성을 주입한다. 때문에 객체 상호간의 결합도가 낮으며 코드의 수정, 재사용, 테스트에 상당히 용이하다.

* 일반적으로 어떤 객체에서 다른 객체를 사용하고자 할 때 객체 내에서 생성하여 사용한다. 이런 방식은 상당히 간단하지만 객체 간 높은 결합도를 갖기 때문에 재사용 및 테스트가 어렵고 코드의 수정 시에 많은 양의 코드를 수정해야 하는 번거로움 또한 존재한다.

  ```java
  public class TV {
      private Speaker speaker;
      
      public TV() {
          System.out.println("TV Created.");
          this.speaker = new Speaker();
      }
      
      public void volumeUp() {
          speaker.volumeUp();
      }
      
      public void volumeDown() {
          speaker.volumeDown();
      }
  }
  
  public class Speaker {
      public Speaker() {
          System.out.println("Speaker Created.");
      }
      
      public void volumeUp() {
          System.out.println("Volume Up.");
      }
      
      public void volumeDown() {
          System.out.println("Volume Down.");
      }
  }
  ```

* 그러나 객체 간 의존도를 없애고자 독립적인 객체를 만든다면 객체 간의 상호작용이 불가능한 쓸모없는 코드가 되어버린다.

  ```java
  public static void main(String[] args) {
      TV tv = new TV();
      Speaker speaker = new Speaker();
  }
  
  public class TV {
      public TV() {
          System.out.println("TV Created.");
      }
      
      public void volumeUp() {
          ...
      }
      
      public void volumeDown() {
          ...
      }
  }
  
  public class Speaker {
      public Speaker() {
          System.out.println("Speaker Created.");
      }
      
      public void volumeUp() {
          System.out.println("Volume Up.");
      }
      
      public void volumeDown() {
          System.out.println("Volume Down.");
      }
  }
  ```

* 스프링에서는 DI를 활용하여 생성자 혹은 Setter 메소드를 통해 객체 간 의존성을 외부에서 주입함으로써 객체 간의 결합도를 낮추면서도 상호작용이 가능하게 한다.

* 스프링에서는 일반적으로 의존성을 주입 할 메소드가 포함된 인터페이스를 객체에 구현하고 의존성이 주입될 객체에서도 해당 인터페이스를 통해 주입함으로써 객체 간 결합도를 낮춘다.

* 아래는 의존성이 있는 메소드가 포함된 인터페이스, 인터페이스를 구현한 객체, 그리고 생성자 혹은 Setter 메소드를 통해서 의존성이 주입될 객체의 간단한 예제이다.

  ```java
  public class SpeakerImpl implements Speaker {
      public SpeakerImpl() {
          System.out.println("SoundBar Created.");
      }
   
      @Override
      public void volumeUp() {
          System.out.println("Volume Up.");
      }
      
      @Override
      public void volumeDown() {
          System.out.println("Volume Down.");
      }
  }
  
  public interface Speaker {
      void volumeUp();
      void volumeDown();
  }
  ```
  ```java
  // 생성자(Constructor)를 통한 의존성 주입
  public class TV {
      private Speaker speaker;
      
      public TV(Speaker speaker) {
          this.speaker = speaker;
      }
      
      public void volumeUp() {
          speaker.volumeUp;
      }
      
      public void volumeDown() {
          speaker.volumeDown;
      }   
  }
  ```

  ```java
  // Setter 메소드를 통한 의존성 주입
  public class TV {
      private Speaker speaker;
      
      public TV() {
      }
      
      public void setSpeaker(Speaker speaker) {
          this.speaker = speaker;
      }
      
      public void volumeUp() {
          speaker.volumeUp;
      }
      
      public void volumeDown() {
          speaker.volumeDown;
      }   
  }
  ```

* 위와 같은 코드는 Speaker 인터페이스를 통해 객체를 구현하고 주입됨으로써 객체 간 결합도를 낮추어 코드의 재사용이 용이하게 한다. TV 객체에는 인터페이스 구현체가 아닌 Speaker 인터페이스가 주입되기 때문에 코드의 수정 없이도 스프링 설정 수정을 통해 Speaker가 구현된 모든 객체를 주입할 수 있다.

### 2-4. Aspect(AOP)

* [`AOP(Aspect-Oriented Programming)`](https://ko.wikipedia.org/wiki/%EA%B4%80%EC%A0%90_%EC%A7%80%ED%96%A5_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)는 핵심 기능이 구현된 컴포넌트와 공통된 기능이 구현된 컴포넌트를 분리하여 독립적인 컴포넌트로 만들고 핵심 컴포넌트 실행 시에 필요한 공통 컴포넌트를 스프링 설정을 통해 실행 함으로써 중복 코드를 최소화하고 컴포넌트 간 결합도를 낮춘다.

* 다음은 현금 인출을 위한 간단한 예제이다. 핵심 기능인 인출이외에 트랜잭션을 위한 전처리, 후처리까지 한 클래스에 모두 구현되어있다. 만약 개발자가 입금 클래스를 새로 만든다고 가정한다면 개발자는 트랜잭션을 위한 메소드를 또 다른 클래스에 중복으로 구현해야 한다.

  ```java
  public class WithdrawEx {
      private Account account;
      public WithdrawEx(Account account) {
          this.account = account;
      }
      public void beforeWithdraw() {
          account.blockAccount();
      }
      public void withdraw() {
          account.withdraw();
      }
      public void afterWithdraw() {
          account.unblockAccount();
      }
  }
  ```

* 아래와 같이 트랜잭션을 분리된 컴포넌트로 구현하면 트랜잭션이 필요한 다른 핵심 기능에서도 중복된 코드 없이 해당 컴포넌트를 재사용하여 트랜잭션 기능을 사용할 수 있을 것이다.

  ```java
  public void main(String[] args) {
      Account account = new Account();
      Transaction transaction = new Transaction(account);
      
      transaction.before();
      account.withdraw();
      transaction.after();
  }
  
  public class Transaction {
      private Account account;
      public Transaction(Account account) {
          this.account = account;
      }
      public void before() {
          account.blockAccount();
      }
      public void after() {
          account.unblockAccount();
      }
  }
  ```

### 2-5. Template

* 자바를 이용할 때 필요한 기능을 사용하기 위해 불필요한 공통 코드(Bolilerplate Code)를 빈번하게 사용해야 하는 경우가 있다. 대표적으로 JDBC가 있는데 스프링에서는 템플릿을 통해 이러한 공통 코드들은 템플릿 내부에서 실행되고 실제 소스코드에는 핵심 코드만 남아 코드의 가독성을 높여준다. 또한, 기능의 관리 측면에서도 더 효율적이다.

* 다음은 mysql을 사용하는 JDBC의 일반적인 코드이다. 각각의 단계별로 메소드로 분리시켜도 코드의 양이 상당히 많아보인다. 핵심 부분인 쿼리를 실행하는 부분은 getData 메소드 내의 단 두줄에 불과하지만 그것을 하기 위해 많은 코드를 거쳐야 한다. 이는 코드의 가독성을 떨어뜨리고 데이터베이스 접근이 빈번해질수록 중복된 코드 또한 늘어나게 할 것이다.

  ```java
  public JDBCEx {
      Connection conn;
      Statement stmt;
      ResultSet rs;
      
      public JDBCEx() {
          try {
              Class.forName("com.mysql.jdbc.Driver");
          } catch (Exception e) {
              e.printStackTrace();
          }
      }
      
      public void open() {
          try {
              conn = DriverManager.getConnection(url, id, pw);
          } catch(Exception e) {
              e.printStackTrace();
          }
      }
      
      public void getData() {
          try {
              stmt = conn.createStatement();
              rs = stmt.executeQuery("select * from db");
          } catch(Exception e) {
              e.printStackTrace();
          }
      }
      
      public void close() {
          try {
              rs.close();
              stmt.close();
              conn.close();
          } catch(Exception e) {
              e.printStackTrace();
          }
      }
      
      public static void main(String[] args) {
          JDBCEx ex = new JDBCEx();
          ex.open();
          ex.getData();
          ex.close();
      }
  }
  ```

* 다음은 스프링에서 지원하는 JdbcTemplate을 이용하여 데이터베이스에 접근한 객체의 코드이다. 이전의 코드와는 다르게 스프링 설정 파일 내에서 미리 데이터베이스에 대한 세팅을 끝내두기 때문에 실제 코드에는 쿼리를 실행하여 가져오는 부분밖에 없다. 이는 코드의 가독성을 높여줄 뿐만 아니라 동일한 데이터베이스에 대한 연결 및 해제를 템플릿 내부에서 자체적으로 관리하기 때문에 효율성 또한 높아진다.

  ```java
  public class JDBCEx {
      private JdbcTemplate template;
      private Collection result;
      
      public JDBCEx(JdbcTemplate template) {
          this.template = template;
      }
      
      public void getQuery() {
          this.result = this.template.query(
              "select * from db",
              new RowMapper() {
                  public Object mapRow(ResultSet rs, int rowCount) throws SQLException {
                      DB db = new DB();
                      db.setData(rs.getString("data"));
                      return db;
                  }
              });
      }
  }
  ```