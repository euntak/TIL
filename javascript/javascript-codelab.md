## Function Object

Pure JS funtion Object만 만들고, 함수 내부의 내용은 해석하지 않는다.
{{key : value}} 형태로 Function Object를 저장

* key는 function name 
* value는 functio 내용

```js
function book() {
    return 'JS';
}
// book() -> compile -> {{code}}
```

JS Pre compile 언어들은 함수 내부까지의 해석을 하는 것들이 있다.


## Function Definition

* Function Declaration
* Function Expression
* new Function(param1, param2, body)

Key, Value 특징상 함수의 이름을 중복해서 작성 할 수 없기 때문에, `Namespace (FE)`를 이용해서 이와 같은 문제를 해결 할 수 있다.

### Function Declaration

```js
function book() {
    return 'JS';
}
```

### Function Expression

```js
var outside = function inside(param) {
    if(param === 102) return param;
    return inside(param + 1);
} 
console.log(outside(100));
```
KEY VALUE 변경 시 { outside : { indide : FO } }

## JS Engine interpretation

* JS는 스크립팅 언어
    - 작성된 코드를 위에서 한 줄씩 해석하지만 차이가 있다.
* 함수 형태에 따라 해석 순서가 다름
    - 중간에 있는 코드가 먼저 해석 될 수 있다
* 함수 선언문 먼저 해석 (Scope Hoisting)
* 함수 표현식 해석 

```js
function sports() {
    debugger;
    var player = 11;

    function soccer() {
        return player;
    }

    var baseball = function() {};
    soccer();
};
sports();
```

### Code 해석 단계
모든 JS Function은 다음과 같이 동작한다.
첫번째 cycle 돌면서 `함수 선언문`을 찾아 FO(function object)을 등록
두번째 cycle 돌면서 `변수 (var)초기화`를 찾아 `undefined` 초기화
세번째 cycle 돌면서 JS 코드 해석 및 `debugger;` 동작

* 함수 선언문 해석
    * function sports(){};
* 변수 초기화
    * var sports; // undefined
    * var baseball; // undefined
* JS 코드 실행
    * debugger;
    * var sports = 11;
    * var baseball = function() {};

이러한 과정을 `호이스팅(Hoisting)`이라고 칭하며, `var`로 등록된 변수나, `function` 키워드로 등록된 `함수 선언문`이 JS 엔진에 의해서 해석될 때에 `위로 당겨진다(Hoisting)`라는 이유도 이러한 JS 엔진이 코드를 분석하는 특성 때문에 생겨난 개념인 것이다.

### 함수 선언문 오버라이딩

함수 이름이 같을 때 함수 코드 대체 (replace)

* JS는 파라미터 수, 데이터 타입을 체크하지 않음.
    * 왜냐 ? Function Object는 {key : value} 형식으로 저장되기 때문에!
* 초기화 단계에서 함수 선언문을 `FO`로 생성
* 코드 진행 중, 아래에 이름이 같은 함수 선언문이 있으면 `Overriding (replace)`된다.

### Do it!

다음과 같이 코드를 작성 했을 때에 예상 결과를 한번 생각해보기.

1. 함수 선언문 - `first()` - 함수 선언문
2. 함수 표현식 - `second()` - 함수 표현식
3. 함수 선언문 - `third()` - 함수 표현식
4. 함수 표현식 - `fourth()` - 함수 선언문

`debugger;`를 통해서 실행 Context 및 순서 확인하기

```js
window.onload(function () {
    debugger;
    function first() { console.log('first'); };
    console.log(first());
    function first() { console.log('first2'); };

    var second = function () { console.log('second'); };
    console.log(second());
    var second = function () { console.log('second2'); };

    function third() { console.log('third'); };
    console.log(third());
    var third = function() { console.log('third2'); };

    var fourth = function () { console.log('fourth'); };
    console.log(fourth());
    function fourth () { console.log('fourth2'); };
});
```

> 1, 4번의 경우에는 `Function Overriding`이 일어 난다. 때문에, 함수 표현식에서의 `fourth`의 값이 JS 엔진에 의해서 초기화 될 때에 `undefined`가 아닌 이전의 함수 선언문에서 정의 했던 `FO`가 초기 값이 된다.


### 파라미터 값 매핑

파라미터 수가 다를 때도 호출 가능 Why? FO = { key: value }이기 때문에 가능 `예상한 대로 동작은 안 할 수 있어도 호출은 가능하다`,
파라미터 순서에 의해서 매핑되기 때문에, 매핑이 되지 않은 파라미터들은 `undefined`로 초기화 된다.

```js
fn(one) {};
fn(12, 34, 56);
```

### arguments Object

* 호출한 함수의 파라미터 값 저장
* 호출한 함수의 파라미터에 작성한 순서로 저장
* 호출한 함수에 파라미터를 작성하지 않아도 생성

#### 장/단점

* 파라미터의 이름을 작성 할 때의 장점
    * 파라미터의 이름으로 쉽게 필요한 정보를 인식 할 수며있으며
    * 코드의 가독성을 높여준다
* 파라미터 수가 고정일 때
    * 파라미터의 이름을 붙이는 방법 사용
* 파라미터 수가 유동적일 때
    * `arguments` 사용
    * 함수 파라미터에 이름이 있으면 값 설정
    * 아닐 경우 `arguments`를 이용해 설정

## Scope

* 함수가 실행되는 영역
* 함수가 실행될 때 영향을 받는 행위

* split()은 String Object에 존재하므로 String Object가 Scope이다. 

### 사용 목적

* 범위의 제한 ( 신속한 검색/접근 )
* 같은 프로퍼티 이름 사용 가능 ( String의 indexOf(), Array의 indexOf() )
* `NameSpace`를 하나의 `Scope`로 정한다면, 위와 같은 `indexOf()`를 사용할 때에 좋은 구조일 것이다. 

### Scope 구조

* 계층적 구조
* 스코프 안에 스코프가 있는 형태

### Scope 설정 시점

* Function Object를 생성 할 때!
* 함수를 호출할 때 설정하지 않는다.
* Function 내부 코드에 대해서는 구조를 만들지 않는다

### Scope 구조 형성 방법

* 밖에서 안으로 들어가면서 `스코프 구조` 형성
* 안에서 밖으로 나가면서 `스코프 범위` 검색 (최상위 Root = Window)

```js
window.onload = (function() {
    debugger;
    function sales() {
        function get() {
            function discount() { console.log('discount'); };
            discount();
        };
        get();
    };
    sales();
})();
/*
function sales() {} {{Scope = window}}
-> sales() 
-> function get() {} {{Scope = sales}}
-> get() 
-> function discount() {} {{Scope = discount}}
-> discount() 
-> console.log('discount')
*/
```

### Global Object

* 프로그램의 시작점
    * 빌트인 오브젝트 중에 하나
* 글로벌 오브젝트 특성
    * 전체 프로그램을 통해 하나만 존재 한다. (모든 `<script>` 태그 내에 작성한 코드)
    * 오브젝트 이름을 작성하지 않고 함수를 호출하면 글로벌 오브젝트로 간주
        * `getBaseball()`, `sports.getBaseball()`
    * ES5에서 `'use strict'` 모드에서 `window`는 `undefined`
* 글로벌 오브젝트 생성
    * 첫 번째 `<script>`에서 한 번만 생성
    * 자바스크립트 실행 환경 설정
    * `new` 연산자를 통해서 생성 할 수 없다

### Global Scope

* 전체를 통해서 하나만 존재한다
    * 그로므로 `Scope`도 하나
    * Global Object === Global Scope
    * 모든 프로그램에서 사용 가능
* 최상위의 스코프
    * 최종 검색 스코프
    * 검색한 property가 없으면 `undefined` 반환

### Global Function/Valiable
함수 및 변수 동일

* 구분
    * 글로벌 함수/변수 : 전역 함수
    * 로컬 함수/변수 : 지역 함수
* 글로벌 함수/변수
    * 글로벌 Object에 작성한 함수
* 지역 함수/변수
    * 함수 안에서 `var` 키워드를 사용한 함수와 함수 선언문/변수
    * 함수 안에서 `var` 키워드를 사용하지 않으면 글로벌 함수/변수
    * `strict` 모드일 때, `var` 키워드를 사용하지 않고 함수/변수를 선언하면 에러
    * 글로벌 오브젝트에서 `var` 키워드를 사용하여 함수/변수 선언 가능

### strict mode

* `'use strict'` 선언

```js
'use strict'

var outside = 'global';
var getValue = function() {
    inside = 'local';
    return inside;
}
console.log(getValue());
```

```js
'use strict'

var side = 'global';
var getValue = function() {
    side = 'local';
    return side;
}
console.log(getValue());
// none error
```

### binding

* 구조적으로 결속된 상태로 만드는 것
* 대상 : Object, property name
```js
var get = {
    ...qty : value,
    ...price : value
}
```

* Binding의 목적
    * Scope 결정
    * Scope에서 이름 식별 - `식별자 해결`
* Binding 시점 분류
    * 정적 바인딩(Lexical, Static Binding)
    * 동적 바인딩(Dynamic Binding)

### Scope Chain

* Scope가 상하 구조로 연결된 개념/구조

```js
function a() { // window
    function b() { // a()
        function c() { // b()
            console.log(cval);
        }
    }
}
// scope chain
```

* Scope 체인을 사용해서 근접한 Scope 프로퍼티 (function, valiable) 검색
* ES5에서 Scope Chain 개념 폐지
    * [렉시컬 환경(Lexical Environment)](https://medium.com/dailyjs/javascript-basics-the-execution-context-and-the-lexical-environment-3505d4fe1be2) 개념 사용
    * 간단히 Scope Chain에서의 트리구조가 아닌, 자기 자신의 FO에 외부 Scope를 저장하는 개념
    
### Binding 시점

* 정적 바인딩
    * 초기화 단계에서 바인딩
    * 함수 선언문 `이름 바인딩`
    * 변수, 함수 표현식 `이름 바인딩`
    * 대부분이 정적 바인딩
    * `값`은 바인딩 대상이 아니다. Object에 {key, value} 형식이기 때문에, `value=값`이라고 하면, `덮어씌워야지` 바인딩 되지 않는다.

* 동적 바인딩
    * 실행 단계에서 바인딩
    * `eval` 함수, `with`문

### Binding 시점의 중요성

* 바인딩 할 때에 Scope가 결정되기 때문
* function Object 생성 시점에 Scope 결정
    * 인식한 Scope를 `[[Scope]]`에 설정
    * 정적 바인딩은 `[[Scope]]`를 Scope로 사용 변경되지 않는다.
* 같은 단계의 모든 함수의 Scope가 같다.

```js
function sports() {
    var prize = 123;
    function soccer(){ prize += 123 }
    function baseball(){ return prize; }
}
// 위의 soccer(), baseball()의 [[Scope]]는 sports()내의 스코프로 동일하다.
```

### Property 검색 방법

* [[Scope]] 프로퍼티에서 검색 (함수를 실행할 때 [[Scope]]에서 `프로퍼티의 이름`으로 검색)
* 프로퍼티 검색을 위한 추가 처리가 필요하지 않다
    * function Object를 생성할 때 `정적으로 바인딩`했기 때문.
* 처리 속도에 영향을 미치지 않음


### Scope Binding

```js
function sports() { // [[Scope]] = window
    var value = 123;

    function soccer() { // [[Scope]] = sports
        return value;
    }

    var baseball = function() { // [[Scope]] = sports
        return value; // [[Scope]] 인 sports내에 value 찾음
    };
    debugger;
    baseball();
};
sports();
```

## Lexical Environment

* (Context == 묶음,덩어리) 이는 변경이 있을 때에 초기화를 다시 해야 한다 
* Excution Contexts 함수의 실행 영역, 함수 코드를 실행하고 실행 결과를 저장
* 함수를 실행하면 메모리에 올라간다
* ES5 스펙상 (외부 프로그램에서 접근 불가 하다)
* Excution Context 처리 단계
    * 준비 단계 (this context init)
    * 초기화 단계 (outter lexical environment copy inner lexical)
    * 코드 실행 단계 (parameter mapping...)