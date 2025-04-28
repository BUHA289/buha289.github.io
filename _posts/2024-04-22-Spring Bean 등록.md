## Spring Bean 등록

### @ComponentScan

<aside> 📚 Spring이 특정 패키지 내에서 `@Component`, `@Service`, `@Repository`, `@Controller` 같은 Annotation이 붙은 클래스를 자동으로 검색하고, 이를 Bean으로 등록하는 기능이다. 개발자가 Bean을 직접 등록하지 않고도 Spring이 자동으로 관리할 객체들을 찾는다.

</aside>

- **ComponentScan의 역할**

  - Chef가 요리할 재료를 자동으로 식료품 저장고에서 찾아오는 과정, Chef는 스스로 필요한 재료를 찾아 요리에 사용한다.

    ![img](https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F958e044b-3d8c-4344-afb7-a143522908eb%2Fimage.png?table=block&id=1342dc3e-f514-81b8-aa31-ea0947527775&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=860&userId=&cache=v2)

    - 요리사(개발자)가 직접 재료(Bean)를 찾아서 가져올 필요가 없다.

- **@ComponentScan**

  1. 특정 패키지 내에 

     `@Component`

      Annotation이 붙은 클래스를 자동으로 찾아서 

     Spring Bean

     으로 등록한다.

     - Annotation을 이용해 Bean을 등록할 수 있어 코드가 간결해지고 유지보수가 쉬워진다.

  2. 스캐닝 범위는 주로 애플리케이션의 루트(최상위) 패키지에서 시작된다.

  3. @SpringBootApplication

     - SpringBoot로 프로젝트를 생성하면 `main()` 메서드가 있는 클래스 상단에 `@SpringBootApplication` Annotation 이 존재한다.

       ![img](https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F4cca534c-9ba8-4071-9aaa-421b11145d50%2Fimage.png?table=block&id=1342dc3e-f514-8186-b803-e7bff4476694&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=860&userId=&cache=v2)

       ![img](https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F979d8e43-38a4-4237-91d3-73483874330f%2Fimage.png?table=block&id=1342dc3e-f514-8196-82e6-f888c5712798&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=860&userId=&cache=v2)

       - @ComponentScan의 속성

         - basePackages

           : 특정 패키지를 스캔할 때 사용, 배열로 여러개를 선언할 수 있다.

           - 예시: `@ComponentScan(basePackages = {"com.example", "com.another"})`

         - basePackageClasses

           : 특정 클래스가 속한 패키지를 기준으로 스캔할 수 있다.

           - 예시: `@ComponentScan(basePackageClasses = MyApp.class)`

         - excludeFilters

           : 스캔에서 제외할 클래스를 필터링할 수 있다.

           - 예시: `@ComponentScan(excludeFilters = @ComponentScan.Filter(SomeClass.class))`

         - includeFilters

           : 특정 조건에 맞는 클래스만 스캔하여 포함할 수 있다.

           - 예시: `@ComponentScan(includeFilters = @ComponentScan.Filter(Service.class))`

- **@ComponentScan의 동작 순서**

  1. Spring Application이 실행되면 `@ComponentScan`이 지정된 패키지를 탐색한다.
  2. 해당 패키지에서 `@Component` 또는 Annotation이 붙은 클래스를 찾습니다.
  3. 찾은 클래스를 Spring 컨테이너에 빈으로 등록합니다.
  4. 등록된 빈은 **의존성 주입(DI)**과 같은 방식으로 다른 빈과 연결됩니다.

### @Configuration, @Bean

<aside> 📚 Spring Bean을 등록하는 방법에는 수동, 자동 두가지가 존재한다.

</aside>

- Spring Bean 등록 방법

  - Spring Bean은 Bean의 이름으로 등록된다.

    1. **자동 Bean 등록(@ComponentScan, @Component)**

    - `@Component` 이 있는 클래스의 앞글자만 소문자로 변경하여 Bean 이름으로 등록한다.

      ```java
      // myService 라는 이름의 Spring Bean
      @Component
      public class MyService {
      
          public void doSomething() {
              System.out.println("Spring Bean 으로 동작");
          }
          
      }
      ```

    - `@ComponentScan` 을 통해 `@Component`로 설정된 클래스를 찾는다.

    1. **수동 Bean 등록(@Configuration, @Bean)**

    - `@Configuration` 이 있는 클래스를 Bean으로 등록하고 해당 클래스를 파싱해서  `@Bean` 이 있는 메서드를 찾아 Bean을 생성한다. 이때 해당 메서드의 이름으로 Bean의 이름이 설정된다.

      ```java
      // 인터페이스
      public interface TestService {
          void doSomething();
      }
      
      // 인터페이스 구현체
      public class TestServiceImpl implements TestService {
          @Override
          public void doSomething() {
              System.out.println("Test Service 메서드 호출");
          }
      }
      
      // 수동으로 빈 등록
      @Configuration
      public class AppConfig {
          
          // TestService 타입의 Spring Bean 등록
          @Bean
          public TestService testService() {
              // TestServiceImpl을 Bean으로 등록
              return new TestServiceImpl();
          }
          
      }
      
      // Spring Bean으로 등록이 되었는지 확인
      public class MainApp {
          public static void main(String[] args) {
              // Spring ApplicationContext 생성 및 설정 클래스(AppConfig) 등록
              ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
      
              // 등록된 TestService 빈 가져오기
              TestService service = context.getBean(TestService.class);
      
              // 빈 메서드 호출
              service.doSomething();
          }
      }
      ```

<aside> 💡 수동으로 Bean을 등록할 때는 항상 `@Configuration`과 함께 사용해야 Bean이 싱글톤으로 관리된다. CGLIB 라이브러리와 연관이 있다.

</aside>

### Bean 충돌

<aside> 📚 Bean 등록 방법에는 수동, 자동 두가지가 존재하고 Bean은 각각의 이름으로 생성된다. 이때 이름이 같은 Bean이 설정되고자 한다면 충돌이 발생한다.

</aside>

- 같은 이름의 Bean 등록

  - 자동 Bean 등록 VS 자동 Bean 등록

    ```java
    public interface ConflictService {
        void test();
    }
    
    // Bean의 이름을 service로 설정
    @Component("service")
    public class ConflictServiceV1 implements ConflictService {
        @Override
        public void test() {
            System.out.println("Conflict V1");
        }
    }
    
    // Bean의 이름을 service로 설정
    @Component("service")
    public class ConflictServiceV2 implements ConflictService {
        @Override
        public void test() {
            System.out.println("Conflict V2");
        }
    }
    
    // componentScan의 범위를 conflict 패키지 하위로 설정
    @ComponentScan(basePackages = "com.example.springconcept.conflict")
    public class ConflictApp {
        public static void main(String[] args) {
            ApplicationContext context = new AnnotationConfigApplicationContext(ConflictApp.class);
    
            // Service 빈을 가져와서 실행
            ConflictService service = context.getBean(ConflictService.class);
    
            service.test();
        }
    }
    ```

    - `ConflictingBeanDefinitionException` 발생

  - 수동 Bean 등록 VS 자동 Bean 등록

    ```java
    // conflictService 이름으로 Bean 생성
    @Component
    public class ConflictService implements MyService {
        @Override
        public void doSomething() {
            System.out.println("ConflictService 메서드 호출");
        }
    }
    
    public class ConflictServiceV2 implements MyService {
        @Override
        public void doSomething() {
            System.out.println("ConflictServiceV2 메서드 호출");
        }
    }
    
    // 수동으로 Bean 등록
    @Configuration
    public class ConflictAppConfig {
    		
    		// conflictService 이름으로 Bean 생성
        @Bean(name = "conflictService")
        MyService myService() {
            return new ConflictServiceV2();
        }
    
    }
    
    @ComponentScan(basePackages = "com.example.springconcept.conflict2")
    public class ConflictApp2 {
        public static void main(String[] args) {
            ApplicationContext context = new AnnotationConfigApplicationContext(ConflictApp2.class);
    
            // Service 빈을 가져와서 실행
            MyService service = context.getBean(MyService.class);
    
            service.doSomething();
        }
    }
    ```

    - 수동 Bean 등록이 자동 Bean 등록을 오버라이딩해서 우선권을 가진다.

    - 의도한 결과라면 다행이지만, 아닌 경우(실수)가 대부분이다. → 버그 발생

    - Spring Boot에서는 수동과 자동 Bean등록의 충돌이 발생하면 오류가 발생한다.

      <aside> ⛔ **주의사항**

      - 테스트 시 자동 빈 등록 충돌 **`conflict`** 패키지의 Bean Name을 지워주세요.
      - **테스트 시 반드시 `@SpringBootApplication` 으로 실행해야 합니다.** </aside>

      ```java
      Consider renaming one of the beans or enabling overriding by setting spring.main.allow-bean-definition-overriding=true
      ```

  - 설정 변경(application.properties)

    ```java
    // 수동, 자동 Bean을 동시에 등록할 때 이름이 같으면 수동 Bean이 오버라이딩
    spring.main.allow-bean-definition-overriding=true 
    
    // 기본값
    spring.main.allow-bean-definition-overriding=false
    ```



## 😊 Spring Bean 등록하기

### 1. 자동 등록 - `@ComponentScan`

> **Spring이 특정 패키지 내 `@Component`, `@Service`, `@Repository`, `@Controller` Annotation이 붙은 클래스를 자동으로 찾아서 Bean으로 등록하는 기능**

- **비유**: Chef가 식료품 저장고에서 필요한 재료(Bean)를 알아서 찾아오는 과정 → 개발자가 일일이 등록하지 않아도 됨.

- **동작 방식**:

  1. Application 실행 시 `@ComponentScan`이 지정된 패키지 탐색
  2. `@Component` 계열 Annotation이 붙은 클래스를 자동으로 찾음
  3. 찾아낸 클래스를 **Spring Bean**으로 등록
  4. 등록된 Bean은 **의존성 주입(DI)** 등에 활용됨

- **Annotation 상세**

  - `@ComponentScan`
    - 기본: `@SpringBootApplication`이 붙은 클래스 패키지를 기준으로 하위 패키지를 스캔
    - 주요 속성:
      - `basePackages`: 특정 패키지 지정
         → `@ComponentScan(basePackages = {"com.example", "com.another"})`
      - `basePackageClasses`: 특정 클래스 기준 스캔
         → `@ComponentScan(basePackageClasses = MyApp.class)`
      - `excludeFilters`: 스캔 제외 필터
      - `includeFilters`: 특정 조건만 스캔

- **Bean 이름 규칙**

  - `@Component` 클래스명에서 앞글자만 소문자로 변경해 Bean 이름으로 등록

    ```java
    @Component
    public class MyService {} 
    // → "myService"라는 이름으로 등록
    ```

------

### 2. 수동 등록 - `@Configuration`, `@Bean`

> **개발자가 명시적으로 Bean을 생성하고 등록하는 방법**

- **동작 방식**:

  1. `@Configuration` 클래스를 Spring이 Bean으로 등록
  2. `@Configuration` 클래스 내 `@Bean` 메서드를 찾아 호출
  3. `@Bean` 메서드 이름으로 Bean을 생성 및 등록

- **예시**

  ```java
  @Configuration
  public class AppConfig {
      @Bean
      public TestService testService() {
          return new TestServiceImpl();
      }
  }
  ```

  - `testService`라는 이름의 Spring Bean 생성

- **주의사항**

  - `@Configuration` 없이 `@Bean`만 사용할 경우, 메서드마다 새로운 객체를 생성할 수 있다.
  - `@Configuration`을 사용하면 **CGLIB**을 통해 프록시 클래스를 생성하여 Bean을 싱글톤으로 관리.

------

### 3. Bean 충돌 (이름이 같은 Bean 중복 등록)

#### (1) 자동 등록 vs 자동 등록

- **문제 상황**
  - 같은 이름(`@Component("service")`)을 가진 두 개 이상의 클래스 존재
- **결과**:
  - `ConflictingBeanDefinitionException` 예외 발생 → Application 실행 실패

#### (2) 수동 등록 vs 자동 등록

- **문제 상황**

  - 자동 등록된 Bean과 같은 이름의 수동 등록 Bean 존재

- **결과**:

  - **수동 등록이 자동 등록을 덮어쓴다(override)**
     → 의도하지 않은 오버라이딩 시 **버그** 발생 가능

- **Spring Boot 기본 설정**

  - `spring.main.allow-bean-definition-overriding=false`
     → 충돌 시 **오류 발생** (오버라이딩 불허)

- **설정 변경 방법**

  - `application.properties`에 설정 추가

    ```java
    ini
    
    
    spring.main.allow-bean-definition-overriding=true
    ```

    → 수동 Bean이 자동 Bean을 오버라이딩하도록 허용

------

## 📍 정리 요약



| 구분               | 내용                                                         |
| ------------------ | ------------------------------------------------------------ |
| **자동 등록**      | `@ComponentScan`을 통해 `@Component` 계열 Annotation을 찾고 자동 등록 |
| **수동 등록**      | `@Configuration` + `@Bean`을 통해 명시적으로 등록            |
| **Bean 이름 규칙** | 클래스명 첫 글자 소문자로 등록 (`MyService` → `myService`)   |
| **충돌 발생 시**   | 수동 등록이 자동 등록을 덮어씀 (Spring Boot는 기본적으로 충돌 오류 발생) |
| **설정 변경**      | `spring.main.allow-bean-definition-overriding=true`로 오버라이딩 허용 가능 |
