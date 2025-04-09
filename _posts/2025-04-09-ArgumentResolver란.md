`ArgumentResolver`는 Spring Framework에 사용되며,컨트롤러 메서드의 파라미터(매개변수)를 해석하고 주입해주는 역할

### 간단 설명

컨트롤러 메서드에 우리가 정의한 `@RequestParam`, `@PathVariable`, `@ModelAttribute`, `@RequestBody` 같은 **파라미터를 어떻게 넣을지 결정해주는 역할**

------

###  개념 정의

> `HandlerMethodArgumentResolver`는 스프링 MVC에서 컨트롤러 메서드의 파라미터를 어떻게 바인딩할지를 결정하는 인터페이스

------

###  예시

```java
@GetMapping("/hello")
public String hello(@RequestParam String name) {
    return "Hello " + name;
}
```

위 메서드에서 `name` 파라미터는 채우는 과정

RequestParamMethodArgumentResolver`를 이용해서
 요청에서 `name`이라는 파라미터 값을 찾아서 이 매개변수에 넣는다

------

###  커스텀 ArgumentResolver도 가능함

자신만의 어노테이션을 만들어서 새로운 방식으로 파라미터를 바인딩하고 싶을 때
 `HandlerMethodArgumentResolver`를 직접 구현해서 사용할 수 있다

```java
public class LoginUserArgumentResolver implements HandlerMethodArgumentResolver {
    
    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        return parameter.getParameterAnnotation(LoginUser.class) != null;
    }

    @Override
    public Object resolveArgument(...) {
        // 세션에서 사용자 정보 꺼내서 주입
        return session.getAttribute("loginUser");
    }
}
```

------

### 요약

| 키워드    | 설명                                                 |
| --------- | ---------------------------------------------------- |
| 역할      | 컨트롤러 메서드의 파라미터 값을 적절히 넣어주는 객체 |
| 위치      | `HandlerMethodArgumentResolver` 인터페이스           |
| 기본 제공 | `@RequestParam`, `@PathVariable` 등                  |
| 확장 가능 | 커스텀 어노테이션과 함께 사용하여 직접 구현 가능     |
