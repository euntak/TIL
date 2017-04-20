# 자바스크립트에서의 반복문 작성

### LINK

[https://hacks.mozilla.org/2015/04/es6-in-depth-iterators-and-the-for-of-loop/?utm_source=javascriptweekly&utm_medium=email](https://hacks.mozilla.org/2015/04/es6-in-depth-iterators-and-the-for-of-loop/?utm_source=javascriptweekly&utm_medium=email)
[https://github.com/nhnent/fe.javascript/wiki/April-27-%E2%80%94-May-1,-2015](https://github.com/nhnent/fe.javascript/wiki/April-27-%E2%80%94-May-1,-2015)

### ES5에서의 반복 

```javascript

/* 
ES5 이후로 당신은 내장된 forEach 메소드를 사용할 수 있다.
break, countinue, return 같은 명령이 먹히지 않는다.
*/
myArray.forEach(function (value) {
  console.log(value);
});

for (var index in myArray) {    // don't actually do this
  console.log(myArray[index]);
}

/*
위의 for in 루프에서의 index에 할당된 값은 `문자열`이다!
그렇기 때문에 "0", "1", "2"와 같은 실제 숫자값이 들어가지 않는다.
for-in은 문자열 키를 가진 일반 Object 객체들을 위해 만들어졌다.
Array에는 적합하지 않다.
*/

```

### ES6에서의 반복

```javascript

for (var value of myArray) {
  console.log(value);
}

// Object에서의 프로퍼티 반복
for (var key of Object.keys(someObject)) {
  console.log(key + ": " + someObject[key]);
}

/*
지금까지는 배열의 요소를 루프시키는 가장 간결하고 직접적인 문법이다.
모든 for-in의 함정들을 피해간다.
forEach()와 다르게, break, continue, return과 함께 동작한다.
*/
```

### Map & Set 객체

```javascript 

// Set : make a set from an array of words
var uniqueWords = new Set(words);

for (var word of uniqueWords) {
  console.log(word);
}

// Map
for (var [key, value] of phoneBookMap) {
  console.log(key + "'s phone number is: " + value);
}
```


