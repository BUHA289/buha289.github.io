## Validation

### 검증(Validation)

<aside> 📚 특정 데이터(주로 클라이언트의 요청 데이터)의 값이 유효한지 확인하는 단계를 의미한다.

</aside>

<aside> 💡 Controller의 주요한 역할 중 하나는 Validation 이다. HTTP 요청이 정상인지 검증한다.

</aside>

- **Validation이란?**

  - 시스템이 미리 정의한 사양(specification)에 부합하고 있는지 검증하는 것

  - 예시

    ![img](https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F0331453b-82cb-46bc-ae3e-2afedb5d545e%2Fimage.png?table=block&id=1342dc3e-f514-81b7-a116-e701d3db782e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=580&userId=&cache=v2)

    - 식당 메뉴판(specification)에 있는 메뉴만 주문이 가능하다.

    ![img](https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F39a83498-1024-44eb-b34a-5a81740ffcd9%2Fimage.png?table=block&id=1342dc3e-f514-813f-b41b-c22ae9aed7e1&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=580&userId=&cache=v2)

    - 안내를 통해 제대로된 주문을 받을 수 있다.

- **Validation을 사용하는 이유**

  - 예시

    ![img](https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2Fabe8e8a9-b547-40cf-9670-83ac5447c089%2Fimage.png?table=block&id=1342dc3e-f514-817e-a07b-c9003a7787fd&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=580&userId=&cache=v2)

    - 주문서 작성 페이지에서 잘못된 입력값으로 인해 서버에 오류가 발생한다면?

      ex) 휴대폰 번호에 숫자가 아닌 문자가 들어간 경우

    - 서버의 문제로 인해 작성 페이지에서 Error 페이지로 이동된다면?

    - Error 페이지로 이동되어 작성중인 폼이 모두 리셋되어 다시 작성해야 한다면?

    - 이러한 서비스의 유저는 굉장한 불편함을 겪게된다.

- **Validation의 역할**

  1. 검증을 통해 적절한 메세지를 유저에게 보여주어야 한다.
  2. 검증 오류로 인해 정상적인 동작을 하지 못하는 경우는 없어야 한다.
  3. 사용자가 입력한 데이터는 유지된 상태여야 한다.

<aside> 💡 실제 서버를 운영하다보면 기능의 의도와 다른 다양한 사용 방법들을 보게된다. ex) Enter로 입력이 완료되도록 만들었지만 누군가는 Click, Tab + Enter를 누르듯

</aside>

- 검증의 종류

  1. 프론트 검증

     - 해당 검증은 유저가 조작할 수 있음으로 보안에 취약하다.

     - 보안에 취약하지만 그럼에도 꼭 필요하다

       ex) 비밀번호에 특수문자가 포함되어야 한다면 즉각적인 alert 가능 → 유저 사용성 증가

  2. 서버 검증

     - 프론트 검증없이 서버에서만 검증한다면 유저 사용성이 떨어진다.
     - API Spec을 정의해서 Validation 오류를 Response 예시에 남겨주어야 한다.
       - API 명세서를 잘 만들어야 그에 맞는 대응을 할 수 있다.
     - 서버 검증은 선택이 아닌 필수이다.

  3. 데이터베이스 검증

     - Not Null, Default와 같은 제약조건을 설정한다.
     - 최종 방어선의 역할을 수행한다.

<aside> 💡 기본적으로 프론트, 서버, 데이터베이스 모두 검증을 꼼꼼하게 하는것이 바람직하다. Validation으로 수많은 Error와 문제들을 방지할 수 있다.

</aside>

### BindingResult

<aside> 📚 Spring에서 기본적으로 제공되는 Validation 오류를 보관하는 객체이다. 주로 사용자 입력 폼을 검증할 때 많이 쓰이고 Field Error와 ObjectError를 보관한다.

</aside>

- **BindingResult**

  ![img](https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F3dfdf27b-a92f-4bbd-8858-9447abebc293%2Fimage.png?table=block&id=1342dc3e-f514-817d-a5f4-e0fe731d7df1&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=670&userId=&cache=v2)

  - Errors 인터페이스를 상속받은 인터페이스이다.
  - Errors 인터페이스는 에러의 저장과 조회 기능을 제공한다.
  - BindingResult는 `addError()` 와 같은 추가적인 기능을 제공한다.
  - Spring이 기본적으로 사용하는 구현체는 `BeanPropertyBindingResult` 이다.

- **파라미터에 BindingResult가 없는 경우**

  - 예시코드

    ```java
    @Data
    public class MemberCreateRequestDto {
        private Long point;
        private String name;
        private Integer age;
    }
    ```

    ```java
    // View 반환
    @Controller
    public class BingdingResultController {
    	
        @PostMapping("/v1/member")
        public String createMemberV1(@ModelAttribute MemberCreateRequestDto request, Model model) {
            // Model에 저장
            System.out.println("/V1/member API가 호출되었습니다.");
            model.addAttribute("point", request.getPoint());
            model.addAttribute("name", request.getName());
            model.addAttribute("age", request.getAge());
    
            // Thymeleaf Template Engine View Name
            return "complete";
        }
    
    }
    ```

    ```html
    <!DOCTYPE html>
    <html xmlns:th="<http://www.thymeleaf.org>">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <h1>Member 생성이 완료되었습니다!</h1>
        <ul>
            <li><span th:text="${point}">포인트</span></li>
            <li><span th:text="${name}">이름</span></li>
            <li><span th:text="${age}">나이</span></li>
        </ul>
    </body>
    </html>
    ```

  - **Postman**

    - 정상 요청

      ![img](https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F91338158-3442-4b34-9068-920819c5d5c4%2Fimage.png?table=block&id=1342dc3e-f514-81dc-b21b-e71e6b60751a&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=860&userId=&cache=v2)

      - 정상 적인 응답이 온다.
      - viewName인 `complete`페이지로 이동한다.

    - 잘못된 요청(Long point 필드에 문자열 데이터)

      ![img](https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F72cd616e-13d4-488d-bb5c-6cf2b546be99%2Fimage.png?table=block&id=1342dc3e-f514-8166-9c9d-cf22a232f5ac&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=860&userId=&cache=v2)

      - 검증 오류(400 Bad Request)가 발생하고 Controller가 호출되지 않는다.

        ![img](https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F1493e208-cf6d-4d8f-afad-f2b87c34f770%2Fimage.png?table=block&id=1342dc3e-f514-8114-afde-fbbcc64729f2&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=860&userId=&cache=v2)

- **파라미터에 BindingResult가 있는 경우**

  <aside> ⛔ **주의사항 :** BindingResult 파라미터는 검증대상 파라미터 뒤에 위치해야만 한다.

  </aside>

  - 예시코드

    ```java
    @Controller
    public class BindingResultController {
    
        @PostMapping("/v2/member")
        public String createMemberV2(
                // 1. @ModelAttribute 뒤에 2. BindingResult가 위치한다.
                @ModelAttribute MemberCreateRequestDto request,
                BindingResult bindingResult,
                Model model
        ) {
    
            System.out.println("/V2/member API가 호출되었습니다.");
    
            // BindingResult의 에러 출력
            List<ObjectError> allErrors = bindingResult.getAllErrors();
            System.out.println("allErrors = " + allErrors);
    
            // Model에 저장
            model.addAttribute("point", request.getPoint());
            model.addAttribute("name", request.getName());
            model.addAttribute("age", request.getAge());
    
            return "complete";
        }
        
    }
    ```

  - **Postman**

    - 정상 요청

      ![img](https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F8f6b30a6-85ec-40b6-930f-17f8f030eae8%2Fimage.png?table=block&id=1342dc3e-f514-81ba-bcd3-dc5e20e1e138&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=860&userId=&cache=v2)

      - 정상 응답이 온다.

    - 잘못된 요청

      ![img](https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F3f16e0d1-99d9-4090-961f-cd3025d9bea3%2Fimage.png?table=block&id=1342dc3e-f514-810a-9f95-edc309dc2a11&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=860&userId=&cache=v2)

      - ```
        @ModelAttribute
        ```

         필드 or 객체에 파라미터 바인딩 오류가 발생

        - BindingResult에 오류가 보관되고 Controller는 정상적으로 호출된다.

          ![img](https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F271228a2-e667-4ae6-9b46-f00883f41bdb%2Fimage.png?table=block&id=1342dc3e-f514-81a4-859c-c439776c57fd&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=860&userId=&cache=v2)

<aside> 💡 @ModelAttribute는 파라미터를 필드 하나하나에 바인딩한다. 파라미터에 Binding Result가 함께 있는 경우  만약 그중 하나의 필드에 오류가 발생하면 해당 필드를 제외하고 나머지 필드들만 바인딩 된 후 Controller가 호출된다.

</aside>



## 🛡️ Validation (검증)

### 1. Validation이란?

> **클라이언트 요청 데이터가 시스템이 정의한 사양(specification)에 맞는지 확인하는 과정**

- **주요 목적**: 잘못된 입력을 사전에 걸러내고, 서버 오류 및 사용자 불편을 방지
- **Controller의 핵심 역할 중 하나**
- **비유**
  - 식당 메뉴판에 없는 메뉴는 주문 불가
  - 잘못된 주문은 사전에 안내하여 오류를 막는다

------

### 2. Validation이 필요한 이유

- 잘못된 입력값으로 서버 에러 발생 시:
  - 작성 중이던 폼이 리셋되어 사용자 불편 초래
- → 사전 Validation을 통해 사용자 경험(UX) 향상, 서버 안정성 강화

------

### 3. Validation의 3단계



| 단계                  | 설명                                 | 특징                                                        |
| --------------------- | ------------------------------------ | ----------------------------------------------------------- |
| **프론트엔드 검증**   | 클라이언트(브라우저)에서 입력값 검증 | 빠른 피드백 제공 (Alert 등), 하지만 조작 가능하여 보안 취약 |
| **서버 검증**         | 서버단에서 요청 데이터 검증          | 보안상 반드시 필요. API Spec에 오류 Response를 명시         |
| **데이터베이스 검증** | DB 제약조건 (NOT NULL, DEFAULT 등)   | 최종 방어선 역할                                            |

> **프론트 → 서버 → DB** 3단계 모두 Validation을 꼼꼼히 수행해야 서비스 신뢰성을 높일 수 있다.

------

## 📦 BindingResult (Spring의 Validation 오류 보관소)

### 1. BindingResult란?

> **Spring이 제공하는 검증 오류 저장 객체**
>  사용자 입력 폼 검증 결과를 보관하며 오류 정보를 관리

- `Errors` 인터페이스를 상속
- 추가 기능 제공 (`addError()` 등)
- 기본 구현체: `BeanPropertyBindingResult`

------

### 2. BindingResult 유무에 따른 차이



| 상황                     | 결과                                                         |
| ------------------------ | ------------------------------------------------------------ |
| **BindingResult가 없음** | 파라미터 바인딩 실패 시 400 Bad Request 발생, Controller 호출 불가 |
| **BindingResult가 있음** | 바인딩 실패 오류가 BindingResult에 저장되고 Controller는 정상 호출 |

------

### 3. 코드 예시 비교

#### ❌ BindingResult 없는 경우

```java
@PostMapping("/v1/member")
public String createMemberV1(@ModelAttribute MemberCreateRequestDto request, Model model) {
    // 요청 데이터가 잘못되면 400 Bad Request 발생
    // Controller 호출 자체가 안됨
}
```

- 잘못된 데이터 입력 → 400 오류 → 폼 리셋

------

#### 🅾️ BindingResult 있는 경우

```java
@PostMapping("/v2/member")
public String createMemberV2(
    @ModelAttribute MemberCreateRequestDto request,
    BindingResult bindingResult,
    Model model
) {
    // 오류가 BindingResult에 저장
    // Controller는 정상 호출되어 사용자에게 에러메시지 제공 가능
}
```

- 잘못된 데이터 입력 → 오류는 BindingResult에 저장 → 나머지 데이터는 정상 처리
- 사용자에게 친절한 에러 메시지를 제공할 수 있음

>  **주의**: `BindingResult`는 검증 대상 파라미터 바로 뒤에 선언해야 한다!

------

## 최종 요약



| 항목                    | 설명                                                     |
| ----------------------- | -------------------------------------------------------- |
| Validation 정의         | 입력값이 사양(specification)에 부합하는지 검증           |
| Validation 단계         | 프론트엔드 → 서버 → 데이터베이스                         |
| BindingResult           | Spring의 검증 오류 저장 객체, Controller 호출 여부 결정  |
| BindingResult 없는 경우 | 오류 발생 시 Controller 호출 불가 (400 Bad Request)      |
| BindingResult 있는 경우 | 오류는 보관하고 Controller 호출, 사용자 친화적 대응 가능 |