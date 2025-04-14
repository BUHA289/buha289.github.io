## Formatter란?

### Formatter

<aside> 📚 주로 사용자 지정 포맷을 적용해 데이터 변환을 처리할 때 사용된다. `Formatter`는 `ConversionService`와 비슷한 목적을 가지지만 문자열을 객체로 변환하거나 객체를 문자열로 변환하는 과정에서 포맷팅을 세밀하게 제어할 수 있다.

</aside>

- 객체를 특정한 포맷에 맞춰서 문자로 출력하는 기능에 특화된것이 

  ```
  Fomatter
  ```

  이다.

  - `Converter`보다 조금 더 세부적인 기능이라고 생각하면 된다.

#### Formatter 적용

코드 예시

숫자(10000)를 금액 형태(10,000)로 변환하는 Formatter



```java
@Slf4j
public class PriceFormatter implements Formatter<Number> {
	
	@Override
  public Number parse(String text, Locale locale) throws ParseException {
    log.info("text = {}, locale={}", text, locale);
		
		// 변환 로직
		// NumberFormat이 제공하는 기능
		NumberFormat format = NumberFormat.getInstance(locale);
		// "10,000" -> 10000L
		return format.parse(text);
  }

  @Override
  public String print(Number object, Locale locale) {
			log.info("object = {}, locale = {}", object, locale);
			// 10000L -> "10,000"
      return NumberFormat.getInstance(locale).format(object);
  }
	
}
```



# 정리

**Formatter**는 데이터를 **특정 형식으로 변환하거나 출력 형식을 정의**하는 역할

 요약 :

| 용도        | 설명                                                        |
| ----------- | ----------------------------------------------------------- |
| 문자열 포맷 | `"Hello %s".formatted("World")` → `Hello World`             |
| 숫자 포맷   | `String.format("%.2f", 3.14159)` → `3.14`                   |
| 날짜 포맷   | `SimpleDateFormat` 사용하여 `2025-04-14` 같은 형식으로 출력 |

이런 포맷터는 **입력값을 사람이 읽기 쉬운 문자열로 바꾸거나**, 반대로 **문자열을 원하는 객체 타입으로 파싱**할 때 사용한다



## Spring Formatter란?

### FormattingConversionService

<aside> 📚 `ConversionService`와 `Formatter`를 결합한 구현체로 타입 변환과 포맷팅이 필요한 모든 작업을 한 곳에서 수행할 수 있도록 설계되어 있어서 다양한 타입의 변환과 포맷팅을 쉽게 적용할 수 있다.

</aside>

#### - 테스트

```java
public class FormattingConversionServiceTest {

    @Test
    void formattingConversionService() {

        // given
        DefaultFormattingConversionService conversionService = new DefaultFormattingConversionService();

        // Converter 등록
        conversionService.addConverter(new StringToPersonConverter());
        conversionService.addConverter(new PersonToStringConverter());
        // Formatter 등록
        conversionService.addFormatter(new PriceFormatter());

        // when
        String result = conversionService.convert(10000, String.class);

        // then
        Assertions.assertThat(result).isEqualTo("10,000");

    }

}
```

- `ConversionService`가 제공하는 `convert()`를 사용하면 된다.
- 

# 정리

Spring에서 **폼(form) 데이터 → 객체 변환**, 혹은 **객체 → 텍스트** 변환을 지원하기 위해 `Formatter`라는 인터페이스를 제공한다.

 즉, `Converter`와 함께 **Spring 타입 변환 시스템(type conversion system)**의 일부.

### 확인가능한 인터페이스 : `org.springframework.format.Formatter<T>`

```java
public interface Formatter<T> extends Printer<T>, Parser<T> {
    String print(T object, Locale locale);
    T parse(String text, Locale locale) throws ParseException;
}
```

- `print`: 객체 → 문자열 변환
- `parse`: 문자열 → 객체 변환

즉, **양방향 변환**을 지원함.