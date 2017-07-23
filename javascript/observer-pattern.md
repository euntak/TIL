# Observer Pattern
객체의 상태 변화를 관찰하는 관찰자들, 즉 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다 메서드 등을 통해 객체가 직접 목록의 각 옵저버에게 통지하도록 하는 디자인 패턴이다. 주로 분산 이벤트 핸들링 시스템을 구현하는 데 사용된다. 발행/구독 모델로 알려져 있기도 하다.

## 옵저버 패턴에 대한 이해

> 각 모듈들의 중간에서 서로의 상태 변화를 관찰하는 관찰자 `객체`

2. 각 모둘들은 서로 알아야하는 상태의 변화가 있을 경우에 상대 모듈에게 `필요한 데이터`와 함께 상태가 변화했음을 `Observer`를 통해서 `Notify`해야 한다.

3. `Observer`가 `Notify`받았을 때에는, 같이 넘겨 받은 `데이터`로 상태변화와 연관된 각 모듈의 핸들러들을 호출하여 각 모듈이 필요한 작업을 수행할 수 있도록 해준다.

4. 각 모듈에서는 어떤 **상태변화**에 따른 지정된 **핸들러**가 호출이 되어야 하는지 미리 `Observer`에 등록 되어있다는 가정하에 위와 같은 실행이 반복된다.

5. `Notify` 할때에, 중요한 부분은 `.call()`이라는 함수를 통해서 이벤트 핸들러에 등록된 핸들러를 호출하게 되는데, 이 때 `this` 키워드를 사용하기 위해서 `.call(context, arguments)`로 `this` 키워드를 전달받은 `context`에 고정 시켜주는 역할이 굉장히 중요함.


### 옵저버 패턴 실습 코드

```javascript
// 옵저버 객체 생성
var observer = {};

// 각 모듈에서 등록할 handler 목록을 담아둘 객체 등록
observer.handlers = {};
```

### 이벤트 핸들러 등록

```javascript

// 상태 변화 이벤트가 발생하면 실행될 이벤트를 등록하는 함수
// 핸들러 등록시 context(필요한 데이터)를 함께 전달받아 
// 내부에서 this를 사용시 적절한 context에서 실행될 수 있도록 설정
observer.register = function(eventName, handler, context) {
    
    // 해당 이벤트로 기존에 등록된 이벤트들이 있는지 확인
    var handlerArray = this.handlers[eventName];
    if (undefined === handlerArray) {
        // 신규 이벤트라면 새로운 배열을 할당
        // 핸들러를 바로 넣지 않고 배열을 할당하는 이유는
        // 한 이벤트에 여러개의 핸들러를 등록할 수 있도록 하기 위함
        handlerArray = this.handlers[eventName] = [];
    }

    // 전달받은 핸들러와 context를 해당 이벤트의 핸들러 배열에 추가한다
    handlerArray.push({ handler: handler, context: context});
}

```

### 이벤트 핸들러 해제

```javascript

  // 등록된 핸들러를 해제하는 함수를 작성합니다.
observer.unregister = function (eventName, handler, context) {
    var handlerArray = this.handlers[eventName];
    
    if (undefined === handlerArray) return;

    // 삭제할 핸들러와 컨텍스트를 배열에서 찾습니다.
    for (var hidx = 0; hidx < handlerArray.length; hidx++) {
      var currentHandler = handlerArray[hidx];

      // 찾았다면 배열에서 삭제하고 함수를 종료합니다.
      if (handler === currentHandler['handler']
       && context === currentHandler['context']) {
        handlerArray.splice(hidx, 1);
        return ;
      }
    }
  }
```


### 이벤트 핸들러에 Notify

```javascript

// 특정 상태가 변했을때 이벤트를 통보할 함수를 작성합니다.
observer.notify = function (evnetName, data) {
    // ​통보된 이벤트에 등록된 핸들러가 있는지 확인합니다.
    var handlerArray = this.handlers[eventName];
    if (undefined === handlerArray) return; // 없다면 함수를 리턴하여 종료합니다.

    // 핸들러 배열에 등록되어있는 핸들러들을 하나씩 꺼내 전달받은 데이터와 함께 호출합니다.
    for (var hidx = 0; hidx < handlerArray.length; hidx++) {
        var currentHandler = handlerArray[hidx];
        currentHandler['handler'].call(currentHandler['context'], data);
        // 전달받은 함수를 바로 호출하지 않고 call을 사용하여 호출하는 이유는
        // ​미리 등록시 함께 전달된 context 객체를 함수내부에서 this로 사용할 수 있게끔
        // 함수 내부로 전달하기 위함입니다.
        // 자바스크립트에서 this를 사용할때는 상당히 주의해야 합니다.
    }
}
```


### 옵저버 패턴을 이용한 예제


```javascript

var boss = new Person();          // 여기에 사장이 있습니다.
var manager = new Person();       // 팀장도 있고,
var programmer = new Person();    // 개발자도 있습니다.

// 서로 연관성을 한번 만들어 보겠습니다.

// 사장님이 말씀을 하십니다.
boss.speak = function(comment) { // 훈하 말씀을 담아서
  alert(comment);
  observer.notify("bossSpeak", comment);    // '사장이 말한다' 이벤트를 발생시킵니다.
};

// 이제 사장님 이하 직원들이 이야기를 새겨 들어야겠죠?
manager.listen = function (comment) {
  this.bossComment = comment;
  // 팀장님은 팀장님 답게 사장님의 훈하말씀을 본인의 마인드에 새겨놓습니다.

  // * call을 사용하여 context가 넘겨지지 않았다면 이 부분에서 this가
  // manager를 지칭할 것이라는 보장이 없게 됩니다.(자바스크립트의 특징으로 주의해야합니다.)
  // 그래서 notify에서 call을 사용하여 핸들러를 실행시키고
  // 첫번째 인자인 context로 manager를 넘겨주는 것입니다.
  // 그렇게 넘겨진 manager는 함수내에서 this에 할당되어 접근할 수 있게 됩니다.
};
observer.register("bossSpeak", manager.listen, manager);
// 옵저버에 등록하여 언제든 사장님의 말씀을 새겨들을 준비를 합니다.

// 자.. 그럼 우리 개발자는?
programmer.drop = function (comment) {    // 한귀로 듣고
  return comment;  // 한귀로 흘려 봅시다.
};
observer.register("bossSpeak", programmer.drop, programmer);
// 무념무상이 준비되었습니다.

boss.speak("... for an hour ...");    // 이제 사장님이 뭐라고 (한시간 동안)말씀을 하시면,
// 옵저버에 등록한 대로 자동으로
// manager.listen("... for an hour ...");     // 팀장님은 새겨듣고,
// programmer.drop("... for an hour ...");    // 개발자는 흘려 듣게 됩니다.
```


### 정리

옵저버 패턴을 이용하면 각각의 모듈이 어떤 상관관계를 갖는지 명시적으로 표현할 수 있습니다.
한 모듈 안에서 여러 다른 모듈의 함수를 호출하게 되면 우리가 흔히 말하는 스파게티 코드를 만들어 내게 됩니다. 예제의 boss.speak 함수에서 다른 두 함수의 manager.listen과 programmer.drop을 호출한다고 생각해보세요. 물론 그렇게 만들어도 동작은 하겠지만, 중간에 manager 객체가 빠진다고 생각해보면 옵저버를 사용하지 않은 경우에 manager.listen을 호출하는 boss.speak 함수까지 확인하여 관련 부분을 제거해 주어야 하지만 옵저버를 사용해서 구현됐다면 그저 manager와 그에 관련된 옵저버 등록 부분만 제거해주면 다른 boss나 programmer 객체에는 어떠한 수정도 가해지지 않습니다. 이처럼 옵저버 패턴을 사용하면 모듈 간(객체 간) 연관성은 유지하면서 소스 코드 상의 상호 의존성은 제거하는 좀 더 간결한 구조를 만들어 낼 수 있습니다.
