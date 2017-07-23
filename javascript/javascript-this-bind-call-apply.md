# Javascript `this`, `bind()`, `apply()`, `call()`

## Javascript 에서의 `this`

저도 Javascript를 배우면서 `this` 키워드에 대한 이해가 정말 부족하다고 느꼈었습니다. 특히나.. `변할 수 있기 때문`입니다.
Javascript에서 간단하게 `this`를 구별할 수 있는 방법, `this`를 적절하게 사용할 수 있는 방법에 대해서 알아둬야 할 것 같습니다.

### 객체에서 function을 호출 할 때에 다음과 같이 합니다.

```javascript 
app = { 
    init : function() {
        console.log(this);
    }
}

// (함수에서 this는 호출하는 주체) 즉, app.init()에서  `.` 앞에 함수를 호출하는 주체가 되는 것이 `this` 대상입니다. 
app.init();
```

위와 같이 있을 때에, `init()` 내부의 `this`는 `app` 객체가 되는 것입죠

### 그렇다면, 호출하는 주체가 없으면 `this`는 어떤걸 가리킬까요?

```javascript
setTimeout(function() {
    console.log(this); // 너는 무엇이니
}, 1000);

// 주체가 없으면 this == window
```

다음과 같이 `setTimeout()`, `setInterval()`과 같은 함수 내에 callback 함수에서의 `this`는 **`window`** 객체를 가리키게 됩니다!

### 그러면 `변할 수 있는 this`는 언제 결정이 되는 걸까요?

```javascript
// this는 호출 할 때에 결정 된다. 단, 한번 결정되면 함수가 종료될때까지 변경할 수 없다!

function Animal(type, legs) {  
  this.type = type;
  this.legs = legs;  
  this.logInfo = function() {
    console.log(this === myCat); // => false
    console.log('The ' + this.type + ' has ' + this.legs + ' legs');
  }
}
var myCat = new Animal('Cat', 4);  
// "The undefined has undefined legs" 출력
// 혹은 엄격모드라면 TypeError 출력
setTimeout(myCat.logInfo, 1000); 
```

아마도 setTimeout으로 myCat.logInfo()를 호출할 때, myCat 객체가 출력될 거라고 예상할 수 있습니다. 하지만 setTimeout의 매개변수로 전달되었기 때문에 메소드는 객체로부터 분리 되어있고, 1초 뒤 함수 실행이 되기 때문에, logInfo가 함수로써 실행되기 때문에 여기서의 this는 전역 객체이거나 엄격 모드에서라면 undefined가 됩니다.
그렇기 때문에 객체의 정보를 기대한 것대로 출력하지 못합니다.

## `this`를 고정하는자 `bind()`

`this`를 고정하는 방법에는 여러가지가 있는데, 처음으로 살펴볼 아이는 `bind()`입니다. 이어서 계속 보실까요?

### 그럼 어떻게 해결해야 하는가 ?

함수는 `.bind()` 메소드를 사용해 문맥을 강제로 지정시킬 수 있습니다. 아래와 같이 분리된 메소드가 myCat 객체로 바인딩 된다면 이 문제는 아주 간단하게 해결 할 수 있습니다.

```javascript
function Animal(type, legs) {  
  this.type = type;
  this.legs = legs;  
  this.logInfo = function() {
    console.log(this === myCat); // => true
    console.log('The ' + this.type + ' has ' + this.legs + ' legs');
  };
}
var myCat = new Animal('Cat', 4);  
// "The Cat has 4 legs" 출력
setTimeout(myCat.logInfo.bind(myCat), 1000);  
```

`myCat.logInfo.bind(myCat)`는 함수 실행임에도 불구하고, **`bind()`는 return 값이 `함수`** 이기 때문에, 첫번째 인자로 등록된 `myCat`이 주체가 되어 `this`값을 `myCat`객체에 고정시켜주는 역할을 하는 것을 볼 수 있습니다.


```javascript
var Module = {
    init : function() {
        this.bindEvents();
    },
    bindEvents : function() {
        $( "button" ).on( "click", function () {
            // 여기서의 this는 ELEMENT를 가리킵니다.
            $( this ).addClass( "active" );
        });

        $("h3").on("click", this.toggle.bind(this, "name", "props"));
    },
    toggle : function(name, props) {
        console.log(name, props); // "name", "props"
    }
}
// .bind(context, arg1, arg2, ...)

```

`bind()`는 `this`를 고정하는 역할을 하지만 동시에 해당 callback 함수에 arguments들을 나열하여 필요한 정보를 전달 할 수 있습니다.

### this를 고정하지 않고 arguments를 보내는 방법은 ?

```javascript
$("h3").on("click", this.toggle.bind(undefined, "name", "props"));
```

위와 같이 첫번째 인자에 `undefined`나, `null`과 같이 의미 없는 것을 인자로 전달하게 되면, `this`는 고정되지 않고, 인자만 전달할 수 있습니다.


## call() VS apply()

이번에는 `call()`과 `apply()`에 대해서 알아보도록 하겠습니다.

`bind()`와의 차이점은 `bind()`는 `return 값이 함수`인 아이였습니다. 때문에, 콜백을 등록하는데에 용이 했습니다.
하지만, `call()`과 `apply()`는 `bind()`와 같이 `this`를 고정 함과 동시에 `함수를 호출`하는 녀석들입니다.

### 그럼 call()과 apply()의 차이점은 뭐지?

간단하게 말해서 `this`를 고정하는건 공통이지만, 그 뒤에 `bind()`와 같이 인자로 정보를 넘겨줄수 있는 방식에서 차이점이 있습니다. 앞서 살펴본 `bind()`에서 인자를 전달하는 방법은 `순서대로 나열` 하는 방식이였습니다.


먼저 call()을 살펴볼까요?
```
fun.call(thisArg[, arg1[, arg2[, ...]]])
```

그다음 apply()를 살펴볼까요?
```
fun.apply(thisArg, [argsArray])
```

자 이제 둘을 비교해보죠, 먼저 `call()`은 보시는 것 과 같이, 첫번째 인자로 `thisArg`를 두번째 인자부터는 `arg1,2,3..`을 받는 것을 확인 할 수 있습니다. 이는 `bind()`와 같은 형태로 인자를 받는 다는 것을 알 수 있겠네요!

한마디로 `call()`에서 인자를 전달하는 방법은 `순서대로 나열`하는 방식입니다.

그 다음, `apply()`를 살펴보면, 2개의 인자만을 받는데, 첫번째로 `thisArg` 두번째로는 `[argsArray]`를 받는 걸로 봐서 인자를 `배열 형태`로 `하나`만 받는 것을 알 수 있겠네요!

한다미로 `apply()`에서 인자를 전달하는 방법은 `여러 가지 인자를 하나의 배열 형태로 받는` 방식입니다.


## 정리 

`this`, `bind()`, `call()`, `apply()`등을 알아봤는데, 이번 기회를 통해서 조금 애매했던 기능들의 차이점, 역할에 대해서 조금 더 명확하게 알 수 있었으면 좋겠습니다.

## 참고

* [bind](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) 
* [apply](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
* [call](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
* [this](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this)
* [hoisting](https://developer.mozilla.org/ko/docs/Glossary/Hoisting)
* [event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)