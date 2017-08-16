# Validation 

## 어떤 종류가 있는가 ?

### JSR-303 Bean Validation

|Annotation|Description|
|------------|-----------|
|@AssertFalse|해당 속성의 값이 false인지 체크한다.|
|@AssertTrue|해당 속성의 값이 true인지 체크한다.|
|@DecimalMax|해당 속성이 가질 수 있는 최대값을 체크한다.|
|@DecimalMin|해당 속성이 가질 수 있는 최소값을 체크한다.|
|@Digits|해당 속성이 가질 수 있는 정수부의 자리수와 소수부의 자리수를 체크한다.|
|@Future|해당 속성의 값이 현재일 이후인지 체크한다.|
|@Max|해당 속성이 가질 수 있는 최대 Length를 체크한다.|
|@Min|해당 속성이 가질 수 있는 최소 Length를 체크한다.|
|@NotNull|해당 속성의 값이 Null이 아닌지 체크한다.|
|@Null|해당 속성의 값이 Null인지 체크한다.|
|@Past|해당 속성의 값이 현재일 이전인지 체크한다.|
|@Pattern|해당 속성의 값이 정의된 Regular Expression에 부합하는지 체크한다. Regular Expression은 Java Regular Expression Convention(see java.util.regex.Pattern)에 맞게 정의해야 한다.|
|@Size|해당 속성이 가질 수 있는 최대, 최소 Length를 체크|
|@Valid|해당 객체에 대해 Validation Check|

* JSR-303 Spec을 준수하는 Annotation들은 payload, groups, message라는 속성을 공통적으로 가진다

### payload

Validation Constraint(제약)과 관련된 메타 정보를 정의하는데 사용
payload 속성의 값으로 심각도를 정의해두면 Validation Error가 발생하였을 경우 심각도 정보를 추출할 수 있게 된다.

```java
@NotNull(payload = Severity.Error.class)
    @Size(min = 1, max = 50, payload = Severity.Warning.class)
    private String title = "";

// 출처: http://javafactory.tistory.com/349 [FreeLife의 저장소]
```

위에서 적용된 `Serverity` 클래스 구현부이다. `Payload`를 상속 받는다.
```java
public class Severity {
    public static interface Warning extends Payload {
    };

    public static interface Error extends Payload {
    };
}

```java
// @Valid Annotaion을 통해서 유효성 검사를 거치게된다.

@Controller
@RequestMapping("/")
public class FormController {
	
	@RequestMapping(method = RequestMethod.GET)
	public String initForm(Model model) {
		return "form";
	}

	@RequestMapping(method = RequestMethod.POST)
	public String submitForm(@Valid Form form, BindingResult result) {
		String returnVal = "successForm";
		if(result.hasErrors()) {
			returnVal = "form";
		}
		return returnVal;
	}

}

// 출처: http://javafactory.tistory.com/349 [FreeLife의 저장소]
```




### groups

사용된 Validation Constraint의 그룹 정보를 정의하는데 사용된다.
특정 도메인 객체가 생성되는 시점의 Validation Check 대상 속성들과 해당 도메인 객체가 수정되는 시점의 Validation Check 대상 속성들이 다를 수 있기 때문에 이들에 대해 그룹을 부여하고 그룹별로 Validation을 수행하고자 하는 경우 활용할 수 있다.

### message
Validation Error가 발생하였을 경우 표현되는 메시지를 정의하는데 사용된다. 기본적으로 사용중인 Validator를 포함하는 라이브러리 내에 포함된 메시지 리소스 파일로부터 해당 Annotation의 **{fully-qualified class name}.message** 에 해당하는 메시지 값을 추출하게 된다. 예를 들어, `@NotNull Check시 에러가 발생`하면 **javax.validation.constraints.NotNull.message** 에 해당하는 메시지가 표현될 것이다.

ValidationMessages.properties 파일로부터 anyframe.sample.validation.constraint.Telephone.message을 key로 하는 메시지가 출력될 것이다. 다음은 ValidationMessages.properties 파일의 내용이다.

```
anyframe.sample.validation.constraint.Telephone.message=must match "0000-000(or 0000)-0000" (max 13)
```

### Hibernate Validator

|Annotation|Description|
|------------|-----------|
|@NotEmpty|해당 속성의 값이 Empty인지 체크|
|@URL|해당 속성의 값이 URL형식인지 체크|
|@Range(min=,max=)|숫자범위 체크|
|@Email|해당 값이 Email형식인지 체크|
|@Length(min=,max=)|문자열 길이 min과 max 사이인지 체크|

* JSR-303 Bean Validation과 같이 .properties에 해당 Error Messages를 정의하여 사용 할 수 있다. (Domain DTO에서 `message` 속성을 통해서도 설정 가능)

[참고 : Hibernate validator example](https://examples.javacodegeeks.com/enterprise-java/hibernate/hibernate-validator-example/)

## 왜 중요한가 ?

보안 XSS ( 크로스 사이트 스크립트 공격 방법 )

게시판에 글을 쓸때에, 스크립트 코드를 입력시킨다. 해당 사용자의 쿠키나 세션 정보를 이용해서 특정 서버로 정보를 전송한다. 이런 것을 막기 위한 유효성 체크!!! &gt;, 띄어쓰기, unicode 등등.. 원했던 의도 했던 입력 값을 입력 할 수 있게, 또 입력이 잘 했는지 검사하는 것을 습관화 해야 한다. 그러므로~ 유효성 체크를 반드시.. 해야 한다. 반드시!!! **사용자의 입력값을 믿어서는 안된다.** 어떤 값을 입력 할 때에 대한 유효성을 반드시! 거쳐야 한다.



