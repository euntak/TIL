* QnA) React, Angular가 각광 받는 이유? React에서의 특징중에 한가지인 `virtual dom`이 동작하는 원리 및 이점?

> 추상화가 잘 되어있다. Angular는 눈에 띈 layer별 구분이 잘 되어있다.
비즈니스 로직에 포커싱을 맞춰서 개발할 수 있다. Spring이랑의 비슷한 느낌의 Angular ! 초기 러닝커브가 있다. 정형화된 방법으로 개발할 수 있다.

React는 포커싱이 View에 잡혀있다.(DOM과 View가 추상화가 잘되어있다) 다이렉트로 View를 컨트롤하지 않고 해당 데이터를 컨트롤하여 View에 반영되어있다.

> `virtual dom`은 view를 배치화해서 마지막에 한번으로 반영한다.
> [React Guide](http://d2.naver.com/helloworld/9297403)

* QnA) Http1.1과 Http2.0으로 넘어오면서 차이점?

> 가장 큰 차이점 `Multiplexing`, `Server Push`..
HTTP1.1 같은 도메인의 경우 커넥션이 제한 (브라우저마다 다르다)
그래서 나온게 Domain sharding (도메인 별로 나눈다)  
**필수 HTTPS로 해야 한다**  
**브라우저도 HTTPS여야만 동작하는 경우 Video tag, GEOip..**

* QnA) Optimizing Images는 보통 어떠한 방법으로 이루어지는지나요?
    * images를 upload할 경우 Images를 Optimizing하는 전용 서버를 따로 두나요?
    * 전용 images Optimizing하는 라이브러리나, 어떤 전용 툴이 있나요?
    * 실무에서는 어떠한 방법으로 Images를 압축하나요?
    * 비손실과 손실 압축을 하는 경우 무슨 기준으로 나누어 지나요? ( 동영상이나 사진을 Optimizing 할 때에 기준이 따로 나뉘나요? )

> 전용 서버를 둔다. 이미지를 사이즈별로 분리한다음에 (배치로) 업로드한다.
라이브러리도 많다. 색상을 적게 쓰면 압축률이 높다.  
비손실 - 이미지가 쓰는 색상을 변하지 않는 상태에서 갯수를 줄인다  
손실 - 최대한의 갯수를 줄인다. (체감할 수 없는 정도로 알고리즘이 성장)

* QnA) CDN을 이용하여 얻는 이점은 어떤 것이 있나요?

> 캐싱 서버, `Cookie less` - 쿠키가 필요없는, static한 파일요청만 간다.  
국가가 다를때에, 데이터 센터가 나뉠때에, 그것을 CDN으로 묶으면 편리한다.

* QnA) CRP (Critical Rendering Path) 
    * 위와 같은 최적화하는 방법중에 하나로 제시되어있는 것을 봤는데요, Progresive Rendering에서의 script의 lazy-loading이나 페이지당 최소화하는 리소스들을 로딩하는 방법이 궁금합니다. 저희가 진행했던 reservation-service 프로젝트에서 적용 할 수 있는 방법이 있을까요?

    > 눈에 보이는 것만 로딩, 안보이는 것들은 `lazy-loading` 

    * `14KB`는 TCP의 특성상 한번의 왕복으로 얻을 수 있는 최대의 크기인가요? 
    
    > 그렇다.

* QnA) Gzip Compression Level은 어느정도가 적당한 것인가요?

> 상황에 맞게 사용한다.

* QnA) 하드웨어 가속을 사용하면 좋은 이점이 어떤 것이 있을까요? 하드웨어 가속을 무분별하게 사용하면 오히려 해가된다고 하는데, 정확하게 어떤 부분에서 해가 되는 것인가요? (eg.Component Flicking Component에서 Hardware acceleration 옵션을 끄고 켤 수 있는것을 봤습니다)

> GPU를 사용하여 `합성` === 움직이는 것들을 layer별로 나누어서 관리
> 무분별하게 사용할 경우 GPU의 메모리가 증가한다. GPU의 메모리는 한정적이기 때문에 브라우저가 죽는 경우가 생긴다.

* QnA) `<script src="app.js" async></script>`와 같이 async 키워드를 스크립트 태그에 추가하면, 스크립트가 사용 가능해질 때까지 기다리는 동안 DOM 생성을 차단하지 말라고 브라우저에 지시하여 성능이 크게 향상된다고 알고 있는데요. `async` 키워드를 쓰는 경우와 쓰지말아야 하는 경우가 있나요? `DOM을 조작하거나 CSSOM을 조작하지 않는다면 async를 사용해볼 수 있나요?`

> libray를 로딩할 때에 `async` 키워드를 사용한다.

* QnA) 점진적 HTML 전송 이란? 어떠한 방식으로 이루어 지는가? (TCP의 `slowstart` 특성으로 인해서 HTML의 전송이 점직적으로 발생하는 것인가요?)

* QnA) ViewPort가 변경 될 시 Layout의 업데이트를 한번에 처리하는 방식은 정확히 어떤 방식으로 처리를 하는 것인가?