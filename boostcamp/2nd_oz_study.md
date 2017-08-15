# Spring에서의 DI

## DI 란?

Dependency Injection(DI)라고 불리며, 이는 IoC(Inversion of Control 제어의 역전)을 구현 하는 방법중에 하나이다.

각 계층사이, 또는 각 클래스 사이의 의존 관계를 Bean 설정 정보를 바탕으로 IoC Container가 자동으로 연결해주는 역할을 한다.

Spring FrameWork에서는 이러한 `IoC Container` 역할을 하면서 `Dependency Injection Pattern`을 통해서 객체간의 Lifecycle (생성, 소멸, 초기화, 서비스등..)을 관리한다.

Spring에서 BeanFactory와 ApplicationContext
Spring Framwork는 설정 파일에 정의되어 있는 설정정보를 초기화하고 초기화된 Bean에 접근하기 위해 
BeanFactory와 그 하위에 ApplicationContext 인터페이스를 가지고 있다.

## DI의 방법

### Setter(Method) Injection

인자가 없는 생성자나 인자가 없는 `static factory` 메소드가 `bean`을 인스턴스화 하기 위하여 호출 된 후 `bean`의 setter method를 통해서 실체화 하는 방법

`Setter(Method) Injection`해당 클래스의 객체를 나중에 재구성하거나 다시 주입 할 수 있게한다.
`JMX MBeans`를 통한 관리는 매력적인 사용 사례이다.

* `필수`가 아닌 옵션 종속성에 더 적합한 방법이다.
* 옵션 종속성이 아니면 종속성을 사용하는 모든 곳에서 Null이 아닌 검사를 수행해야 한다.

```java
private DependencyA dependencyA;
private DependencyB dependencyB;
private DependencyC dependencyC;
 
@Autowired
public void setDependencyA(DependencyA dependencyA) {
    this.dependencyA = dependencyA;
}
 
@Autowired
public void setDependencyB(DependencyB dependencyB) {
    this.dependencyB = dependencyB;
}
 
@Autowired
public void setDependencyC(DependencyC dependencyC) {
    this.dependencyC = dependencyC;
}
```

### Constructor Injection

생성자의 파라미터를 통해서 주입 받는 방법으로 필요로하는 객체가 어떤 것이 있는지 명확하게 알 수 있으며, 순서의 의존하므로 사용하는 곳에서 불편할 수 있다.

* `필수, 의무적인` 의존성에 대해 더 적합한 방법이다.
* Spring 팀은 애플리케이션 구성 요소를 불변 객체로 구현하고 필요한 종속성이 null이 아닌지 확인하기 위해 생성자 삽입을 지향한다.
* 하지만, 많은 수의 생성자 인수는 클래스가 너무 많은 책임을 갖고 있음을 뜻하므로 적절한 분리하기 위해 리팩토링을 고려해야 한다.

```java
private DependencyA dependencyA;
private DependencyB dependencyB;
private DependencyC dependencyC;
 
@Autowired
public DI(DependencyA dependencyA, DependencyB dependencyB, DependencyC dependencyC) {
    this.dependencyA = dependencyA;
    this.dependencyB = dependencyB;
    this.dependencyC = dependencyC;
}
```

### Property(Field) Injection

`Single Responsibility Principle Violation` 단일 책임 원칙에 위반하는 행위이다.

이는 너무 많은 DI를 설정 할 수 있고, 너무 쉽게 설정 할 수 있다. 하나의 Class에서 너무 많은 일을 하는 것은 단일 책임 원칙에 위반하는 행위기 때문에, 리팩토링을 고려해봐야 한다.

만약, Spring이 아닌, 다른 DI Framwork를 사용하게 된다면, 밑에 있는 코드는 정상적으로 작동할까?를 생각해보면, 당연히 NPE가 떨어지면서 작동을 안할 것 같다.

Spring이 추구하는 이상적인 방법은, Framwork가 변경 되어도 정상적으로 작동이 될만큼 루즈한 커플링을 원하는 것 같다. 그래서 Spring Framework도 다양한 Interface를 제공하는 것이 아닐까.

* `Property(Field) Injection`는 해로운 것이다.
```java
@Autowired
private DependencyA dependencyA;
 
@Autowired
private DependencyB dependencyB;
 
@Autowired
private DependencyC dependencyC;
```

### 출처 

* [I's Story](http://isstory83.tistory.com/91)
* [Vojtech Ruzicka's Programming Blog](http://vojtechruzicka.com/field-dependency-injection-considered-harmful/)
* [JAVAJIGI.net](http://www.javajigi.net/pages/viewpage.action?pageId=3664#Spring%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC%EC%86%8C%EA%B0%9C%EC%99%80IoC%EB%B0%8FSpringIoC%EC%9D%98%EA%B0%9C%EB%85%90-2.1IoC%EB%9E%80%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%3F)