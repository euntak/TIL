## FE 심화학습 주제

1. ES6 + Webpack + functional method..

## Mentoring

### 동기 호출과 비동기 호출을 섞어서 사용하지 말자.

```js
setTimeout(function() {
	callback(cachingData);
}, 0);
```

### Promise

Thenable Object

	- Defferd Object
	- Promise Object ( A/A+/B Type )
	- Blue bird ( Library )


requestAnimationFrame() callback
- 브라우저에게 나 지금 프레임이 변경되야 할 것 같아. 라고 알려주는 콜백, 안정적으로 애니메이션을 적용하기 위해서는 보통 60fps는 16.8ms인데, 이는 브라우저에서 16.8ms 동안 프레임을 찍어내야 할 때에 html Dom 파싱, css 붙이기 render Object만들기 등 최소한의 비용적인 시간이기 때문에, 이보다 낮은 ms로 어떤 DOM을 조작하려고 하면 실제로는 동작하지 않는 현상이 발생한다. 그렇기 때문에, 이러한 불필요한 작업을 안하게 도와주는 메서드이다.


메소드 레벨링 ( 코드 5~6줄 정도의 적당한량이 좋다 )


## BE 심화학습 주제

1. MVC (Restful API)
2. Spring Security (Login interceptor..), Authorization
	- 권한에 대한
3. 단위 테스트 ( Controller )
4. 서버사이드 렌더링 ( View ) Framework ( 타일즈, 타임리프 (Thymeleaf 2.x) )
5. 트랜잭션 ( @Transactional ) AOP.. 
6. JDBC Template, Mybatis (SQL Mapper), Hibernate(ORM)


## Mentoring

원리 중심의 기본적인 질문들. Java 자체에 대한 질문.. 웹서비스는 어떻게 이루어저 있는가?
정의 용어 원리 동작 방식.. 등등 소스코드의 동작 원리 


IDE의 자동완성, 오버로딩된 메서드들이 존재한다. 그의 목적에 맞게 만들어놨기 때문에, 개발할 때에 그에 맞게 주의 깊게 잘 써야 한다. 1.5에서 1.8업그레이드 됬을 때, 변화된 메서드나 개선된 메서드 Deprecated된 메서드들이 있어서 항상 주의 깊게 봐야 한다.

### 보안 XSS ( 크로스 사이트 스크립트 공격 방법 ) 
게시판에 글을 쓸때에, 스크립트 코드를 입력시킨다. 해당 사용자의 쿠키나 세션 정보를 이용해서 특정 서버로 정보를 전송한다. 이런 것을 막기 위한  **유효성 체크!!!** \&gt;, 띄어쓰기, unicode 등등.. 
원했던 의도 했던 입력 값을 입력 할 수 있게, 또 입력이 잘 했는지 검사하는 것을 습관화 해야 한다.
그러므로~ 유효성 체크를 반드시.. 해야 한다. 반드시!!! 사용자의 입력값을 믿어서는 안된다. 어떤 값을 입력 할 때에 대한 유효성을 반드시! 거쳐야 한다.


### 컨벤션 지키기
(Sun Micro에서 만든 JAVA Convention PDF 보기)


### DI (@Autowired) @Inject (JAVA)
[DI 주입에 대한 이해](http://vojtechruzicka.com/field-dependency-injection-considered-harmful/)

spring이 왜 DI를 쓰게 되었나, DI를 사용하면서 뭐가 편해졌나, 등등 근본적인 이해

문제 해결과, 지식의 습득은 다르다.
문제 해결은 Google, Stack Overflow를 통해서 해결하고 나서 이제 그 해결 방법을 통해서 지식을 습들 할 수 있어야 한다.
어떻게 지식을 습득하고, 왜 이렇게 작성을 해야 하며, 이것을 어떻게 응용을 할 수 있는지에 대한 고민을 꾸준히 해야 한다. 지식은 책으로 배우는것이 좋다. (원리를 알아야 한다.)

1. property (실제로 사용하지 않는다. Spring Framework가 아니면, property에 값을 주입할 수 있는 방법이 없기 때문)
2. setter 
3. constructor

### SQL 표준을 명시하라 

INNER JOIN, LEFT JOIN, AS, ON ..등등... ANST SQL 암묵적인 코드는 되도록이면 사용하지 말자.

### 좋은 개발자 

잘 짜여진 오픈소스들을 잘 이해하고, 내 비즈니스 로직에 잘 적용하는 개발자도 좋은 개발자.
잘못된 지식을 습득하는 것을 모르면 안된다. 하나를 배우더라도 제대로된 지식으로 배워야 한다. 그래서 책을 보더라도 그 책을 절대적으로 믿지말고 다방면으로 정보를 모아야 한다.


### RESTFUL API

어떤 리소스가 URI(인디게이터 상위개념임), URL(위치) , URI가 조금 더 맞는 개념이다. 
직관적으로 작성을 해야한다. 상위에서 하위 계층으로 계층적으로 작성!

/products/categories/{productId:[\\d]+} 와 같이 정규표현식을 쓸 수 있다.
/categories/1/products

페이징은 리소스라고 하지 않는다. ( query Param으로 받는다 )

HTTP Status Code (상태코드)를 많이 사용한다. 표준적인 error code를 사용하자
Twitter Restful API 잘 작성되어있으니 참고
천재 개발자들 만들어논 라이브러리들을 참고


### EXCEPTION

Java는 exception은 비용이 많이 들어간다. stackTrace를 쭉 찍기때문에.. 꼭 Exception이 필요한가? 에 대한 고민을 한다. 따라서 적절한 상황에 따라서 다음과 같이 에러를 처리 할 수 있다.

1. 적절한 값으로 치환,
2. 적절한 값으로 null 처리,
3. Exception 발생 ( 상황이 프로그램이 종료되야 할 만한 문제일 경우에 필요할 것 같다 )

### SQL 좋은 작성

검색 할 수 있는 조건의 폭을 최대한 줄여서 해야 한다.
ON은 두개의 테이블이 조인 할 수 있는 제약 조건이다.
WHERE은 조인 된 결과에 대한 조건을 거는 부분이다.

* Oracle Hint - DBMS에 알려주는 정보. 쿼리 최적화를 할 때에 개발자가 조건을 거는데 요즘 안쓴다.

### HTTPServletRequest

HTTPServletRequest 객체는 tomcat에서 작성 된다. 그렇기 때문에, HTTPServletRequest를 파라미터로 받는 메서드를 작성하게 되면, 반드시 Controller에서만 호출할 수 밖에 없다. 그렇기 때문에 HTTPServletRequest를 주입받게 되면, Controller에 강력하게 커플링 되는 형식이다.

다른 Service에서 호출 할 수 있냐 ? 없다면, 고민을 좀 해봐야 할 로직
Service단에서 Map 형태로 JSON 리턴 값을 만들어서 보내는 것 보다는 해당 리턴 값을 DTO, VO로 만들어야 루즈한 커플링을 만들 수 있다.

### Interceptor, MessageConvertor, ArgumentResolver

FormData, File, 어떤 요청이 들어오는건지 검사하는 Spring MessageConvertor,

add Interceptor
add MessageConvertor
add ArgumentResolver
add ViewResolver

어떤 Request가 들어오면, 이 Argument 처리할 수 있는 Resolver List 중에 Resolver를 찾는다. support (methodParameter) 메서드를 통해서 처리 할 수 있는 Resolver를 찾는다. 그래서 적절하게 처리해줄 수 있는 애를 찾고 넘겨주고 종료.

실제 구현체 내부에서는 if()로 검증을 할 필요가 없다.

스프링이 jackson json을 통해서 Map이나 List로 리턴하는 것들을 자동으로 컨버팅 해준다..

실제로 DI에서 Interface부분을 따로 작성해서 Impl하는 부분을 Service나 Dao에서 작성 하게 될 때에 Spring이 알아서 넣어준다. 실제로 Class나 Parameter도 Interface로 작성하는 것이 스프링 개짱이다.

### @scope(prototype)

@scope에 대한 공부

### 공부하기 좋은 책이나 레퍼런스 

* in Action 시리즈 괜찮다. 
* Online보단 Offline에서 어느정도 훑어보고 사는 것이 좋다.
* 강컴 닷컴
* 언어 백과사전 ( 참고서적,원리 서적 )