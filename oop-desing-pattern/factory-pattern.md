# Factory-pattern

### Wiki

팩토리 메서드 패턴(Factory method pattern)은 객체지향 디자인 패턴이다. Factory method는 부모(상위) 클래스에 알려지지 않은 구체 클래스를 생성하는 패턴이며. 자식(하위) 클래스가 어떤 객체를 생성할지를 결정하도록 하는 패턴이기도 하다. 부모(상위) 클래스 코드에 구체 클래스 이름을 감추기 위한 방법으로도 사용한다.

Factory Method라는 패턴 이름이 적절하지 못한데, 이름으로 인해 객체를 생성하는 메소드를 Factory method라 오해하는 개발자가 많이 있다(Allen Holub의 말을 인용.) 이런 생성 메소드가 모두 Factory method 패턴을 사용하는 것은 아니다. Template Method의 생성 패턴 버전으로 볼 수 있는데 Template Method를 알지 못한다면 그 패턴을 먼저 이해하는 것이 Factory Method를 이해하기 수월할 것이다.

Factory Method가 중첩되기 시작하면 굉장히 복잡해 질 수 있다. 또한 상속을 사용하지만 부모(상위) 클래스를 전혀 확장하지 않는다. 따라서 이 패턴은 extends 관계를 잘못 이용한 것으로 볼 수 있다. extends 관계를 남발하게 되면 프로그램의 엔트로피가 높아질 수 있으므로 Factory Method 패턴의 사용을 주의해야 한다.


### 간단한 예

```javascript
//Our pizzas
function HamAndMushroomPizza(){
  var price = 8.50;
  this.getPrice = function(){
    return price;
  }
}

function DeluxePizza(){
  var price = 10.50;
  this.getPrice = function(){
    return price;
  }
}

function SeafoodPizza(){
  var price = 11.50;
  this.getPrice = function(){
    return price;
  }
}

//Pizza Factory
function PizzaFactory(){
  this.createPizza = function(type){
     switch(type){
      case "Ham and Mushroom":
        return new HamAndMushroomPizza();
      case "DeluxePizza":
        return new DeluxePizza();
      case "Seafood Pizza":
        return new SeafoodPizza();
      default:
          return new DeluxePizza();
     }
  }
}

//Usage
var pizzaPrice = new PizzaFactory().createPizza("Ham and Mushroom").getPrice();
alert(pizzaPrice);
```

### LINK

* [https://ko.wikipedia.org/wiki/%ED%8C%A9%ED%86%A0%EB%A6%AC_%EB%A9%94%EC%84%9C%EB%93%9C_%ED%8C%A8%ED%84%B4](https://ko.wikipedia.org/wiki/%ED%8C%A9%ED%86%A0%EB%A6%AC_%EB%A9%94%EC%84%9C%EB%93%9C_%ED%8C%A8%ED%84%B4)