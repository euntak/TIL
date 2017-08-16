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

## 왜 중요한가 ?

## 어떻게 사용하는가 ?
