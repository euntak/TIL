# Critical Rendering Path의 이해

## 웹 성능의 기본적인 규칙

측정을 먼저하고 최적화해라!

## Critical Rendering Path (CRP)란? 

브라우저가 HTML, CSS, Javascript를 화면에 실제 픽셀로 변환하는 단계의 순서이다. 
그 경로를 최적화 할 수 있다면, 웹성능을 향상 시킬 수 있다.

* DOM <-> `Javascript or stylesheet` <-> CSSOM -> Redner Tree -> Layout -> Paint

## Dom Tree

* Caracters(<>구분해서 Tag추출)
* Tokens(추출한 태크들을 Tokenizer가 토큰으로 만듬)
* Nodes(Tokenizer가 시작과 종료 토큰을 만들면 Node간의 관계가 나타난다. 

예를들면, 
```html
<html>
	<head></head>
</html>
```
은, `<head>`태그가 `<html>`의 자식 노드가 되는 것이다


## CSSOM

* Caracters CSS 파서가 토큰들을 노드로 바꾼다.
* DOM Tree가 구성되는 과정과 비슷하지만 종속적인 규칙 때문에 약간의 차이점이 있다. 
* CSS 트리를 부분적으로 점진적으로 사용할 수 없다.
* 모든 CSS를 처리할 때까지 모든 렌더링을 중단한다.

```css
h1 { font-size: 16px }
div p { font-size: 12px }
``` 

```html
<div>
	<h1>Selection Title</h1>
	<p>Lorem ipsum...</p>
<div>
```

* 위와 같은 소스가 있다고 가정할 때에, 어떤 CSS rule이 더 빠를까? 
* 첫번째 `<h1>`태그를 만날때 적용
* 두번째 `<p>`태그를 만났으나 그 부모가 `<div>`인 경우만 적용
* 결론적으로  `<h1>`태그에 직접적으로 접근하는 것이 브라우저 입장에서는 더 빠르다.

## Render Tree

* DOM + CSSOM = Render Tree
* Render Tree는 눈에 보이는 것만 렌더링해서 가지고 있는다.
* 그렇기 때문에, `display:none`속성을 가지고 있는 node의 자식들(상속을 받기 때문)까지 Render Tree로 복사되지 않는다. 

## Layout

`<meta name = "viewport" content="width=device-width">`

* 레이아웃 뷰포트의 폭이 기기의 폭과 일치해야 한다.
* 위와 같은 meta에서 device-width를 설정하지 않으면 기본적으로 브라우저의 기본 뷰포트를 따라간다.
* (일반적으로 980px) 

* 뷰포트의 크기가 기본 320px일 때에, `div`, `p`, `span`, `em`의 width 값은 각각 얼마일까 ?

```css
body { width: 100% }
body div { width: 50% }
div p { width: 100% }
p span { width: 25% }
p em { width: 50% }
```

* div = 160px
* p = 160px
* em = 80px
* span = 40px

* 뷰포트의 크기가 변경되면(폰을 돌리거나 할때에) 브라우저는 변경된 크기에 알맞게 다시 렌더링을 진행한다.

> 그렇다면 ViewPort가 변경되는것에 대한 성능변화가 있을까?

스타일이나 레이어를 변경해서 렌더 트리를 업데이트 할 때마다 레이아웃 단계를 재시행할 가능성이 크다!
내용을 추가하거나, 스타일이 변경이 일어나면서 레이아웃 단계를 몇번더 거치는 상황이 있을 수 있다.

> 그렇다면 해결방법은 ?
  
* 사이트마다 다르다. 보통은 업데이트를 한번에 처리하는 방식으로 해결한다. 

## Paint

* 그려야할 크기와 정보를 가지고 픽셀단위로 그린다.

## Debugging

* 크롬 카나리아를 이용해서 프로파일링 데이터를 폰에 저장한다.
* Android기기에서 개발자옵션을 키고, 크롬을 설치, USB 디버깅을 통해서 폰으로 성능을 측정할 수 있다. (timeline)
* ios 기기에서는 웹킷 프록시를 이용해서 사용할 수 있다.
* 최근에 나온 성능 측정 도구로는 `lighthouse`가 있다. `lighthouse`에서 제공하는 `Best Practices`를 살펴보고, 이에 대한 정리가 필요하다.

### QnA) `점진적 HTML 전송?`이란? 어떠한 방식으로 이루어 지는가?
### QnA) 업데이트를 한번에 처리하는 방식은 정확히 어떤 방식으로 처리를 하는 것인가?
### QnA) 렌더링 방해 스크립트를 어떠한 방법으로 제거하는지, 비동기 스크립트를 수행하는 방법?
### QnA) Javascript가 랜더링 파이프라인에 어떻게 영향을 미치는가?