# Web Performance

#### created by BoostCamp_오주의 마법사

---

## Index

#### Why ?
#### LIGHTHOUSE
#### Text-compression
#### Offscreen-images
#### Optimize-images
#### Oversize-images
#### Speed-index


---

### Why ?

열심히 팀 과제를 수행하던중.. 

@[]](이거 메인페이지 왜이렇게 느려졌냐 ?)  
@[](메인 페이지가 로딩되는게 눈에 보여 !)  
@[](이거 좀 버벅거리는 것 같지 않냐 ?)  
@[](그래도 메인페이지인데.. 좀 더 빨랐으면 좋겠어 !)  
@[](하.....)  

Note:
열심히 팀 과제를 수행하던 도중.. 갑자기 또 다른 과제가 들이닥쳤다

+++

### 왜 느려지는 걸까 ?

* Back-End
* Network
* Front-End

> Steve souders : "서비스 응답 시간의 80~90%는 Front-End에서 소모된다"

@[](Front-End 성능 향상 = 최소비용 최대효율!)

+++

### 근데 어떻게 개선 해야하지 ?

가장 병목을 초래하는 순서대로 진행하는 것이 좋다.

![aa6a](https://s3.ap-northeast-2.amazonaws.com/notes-file-uploads/aa6a6044179489a69b5902ba6a5212f4fegg.jpg)

---?image=/assets/pwa-lighthouse.png&size=contain

### LIGHTHOUSE

[https://developers.google.com/web/tools/lighthouse](https://developers.google.com/web/tools/lighthouse/?hl=ko)  

웹 앱의 품질을 개선하는 <span style="font-family: Helvetica Neue; font-weight: bold; color:#CC0000">오픈 소스 지동화 도구</span>

Note: 최근 Chrome브라우저의 Developer tool Audits에 포함되었으며, 가장 최근에 구글에서 개발한 툴 이므로 믿고 쓰셔도 될 것 같습니다

+++?image=/assets/light-house-exam.png&size=contain

Note: 이 화면이 LIGHTHOUSE가 웹 페이지를 분석한 결과를 보여주는 페이지 입니다. 저희 팀은 LIGHTHOUSE 크롬 익스텐션을 통해서 분석을 했구요, 보시는 화면에서 Performance 와 Best Practice에 초점을 맞춰 개선을 진행 했습니다.

+++

### TIMELINE에서 주어진 제약 상황

```
User agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.101 Safari/537.36
Device Emulation Nexus 5X: Enabled
Network Throttling 562.5ms RTT, 1.4Mbps down, 0.7Mbps up: Enabled
CPU Throttling 4x slowdown: Enabled
```

실제로는 이렇게 극한적인 상황을 만들어서 테스트를 하지 않고 여러가지 상황을 고려해야 하나
저희는 lighthouse가 제공한 환경을 그대로 사용하였습니다.
지표에 보여지는 빨간색 성능에 대한 것들은 성능을 개선했을때의 최대 potential time 과 size이다.

---?image=/assets/webpack.png&size=contain

## TEXT-COMPRESSION

+++

### 첫번째 개선

```
서버사이드 렌더링 적용
서버와의 요청 횟수를 줄이자
페이지에서 사용하는 리소스들의 크기를 줄이자
안보이는 리소스들을 미리 불러오지 말자
```

@[2-3]
@[](서버사이드 렌더링은 다음 기회에.)

Note: FE 개선을 방향으로 잡았기에 서버사이드 렌더링을 제외하고, 나머지를 차례대로 진행 하겠습니다.

+++

### Before

* 리소스 크기 : 500KB 이상
* 시간 : 500ms 이상

![main-before-network](https://s3.ap-northeast-2.amazonaws.com/notes-file-uploads/main-before-network.png)

Note: 개선하기 이전에 요청하는 네트워크 파일요소들이 이렇게 많네요..

+++

### After

* 리소스 크기 : 180KB
* 시간 : 37ms 

![main-after-network](https://s3.ap-northeast-2.amazonaws.com/notes-file-uploads/main-after-network.png)

<p>리소스 크기 <span style="font-family: Helvetica Neue; font-weight: bold; color:#CC0000">약 64% 감소</span>, 시간 <span style="font-family: Helvetica Neue; font-weight: bold; color:#CC0000">약 92%</span> 절약!</p>

@[](외쳐 갓팩!)

Note: 개선하기를 진행하고나서 리소스의 크기는 64%감소하였고, 시간은 약 90%정도 절약되었습니다.

---

## OFFSCREEN-IMAGES

+++

### 두번째 개선

```
서버와의 요청 횟수를 줄이자
페이지에서 사용하는 리소스들의 크기를 줄이자
화면에 보여지지 않는 리소스들을 미리 불러오지 말자
```

@[3]  
@[](화면에 보여지지 않는 이미지들을 미리 요청하지 않는다!)  
@[](가장 빠르고 최상으로 최적화된 리소스는 전송되지 않는 리소스)  

+++

### Why?

이미지의 개수가 많거나 용량이 큰 페이지를 불러올 경우 쓸데없는 네트워크 비용이 증가  
미지를 불러오는 과정에서 서비스 속도에 문제가 발생할 소지있음  

+++

### 이미지 동적 로딩

**IntersectionObserver API**의 활용

```js
function _settingIntersectionObserver() {

    var interectionObserver = new IntersectionObserver
    (function (entries, observer) {
        entries.forEach(function (entry) {
            if (!entry.isIntersecting) {
                return;
            }

            var target = entry.target;
            var $lazyImg = $(target).find('img');

            if ( $lazyImg.data('lazy-img') ){
                var source = $lazyImg.data('lazy-img');
                $lazyImg.attr('src', source);
                $lazyImg.removeAttr('data-lazy-img');

                observer.unobserve(target);
            }

        });
    });

    Array.from($productContainer.find('li.item'))
    .forEach(function (el) {
        interectionObserver.observe(el);
    });
}
```

@[3](IntesectionObserver 설정)  
@[10-17, 19](DOM이 노출되면 이미지 로딩)  
@[18](보여진 이미지는 Observer에서 제거)  
@[24-27](화면에 나타나는 것을 감지하기 위한 Element를 등록)  

Note: 
IntersectionObserver를 쓴 이유 : 이미지 동적 로딩에서 가장 중요한 코드는 해당 엘리먼트가 현재 화면 내에 보이는지 알아내는 것! 이를 위해 엘리먼트의 크기와 위치 값을 돌려주는 `Element.getBoundingClientRect` 함수를 사용하는 경우가 많습니다.
하지만, 이 함수를 호출할 때마다 브라우저는 엘리먼트의 크기와 위치값을 최신 정보로 알아오기 위해 문서의 일부 혹은 전체를 다시 그리게 되는 `리플로우(reflow)` 현상이 발생한다는 점! 그렇기 때문에 **스크롤 이벤트**, **리사이즈 이벤트**와 같이 모든 엘리먼트를 순회하면서 호출하기 때문에 `사용자 interaction 성능이 떨어 질 수 있다`

이를 해결하기 위해서, 스크롤이나 리사이즈 이벤트가 과도하게 발생하는 경우가 많으므로 throttle 또는 debounce 등을 사용해 실행 빈도를 조절할 수도 있습니다. 또는, requestAnimationFrame을 이용해 이벤트 핸들러를 처리하는 방법들을 사용했었다.

이러한 문제점들을 해결해줄 IntersectionObserver API는 아직 몇몇 브라우저의 최신 버전에서만 사용할 수 있지만 지원하지 않는 브라우저를 위해 `Polyfill`을 제공하고 있습니다.

+++

### 예상치 못한 결과

이미지가 채워지기 전 DOM크기가 기준이 되어버림

이로 인해서 예상보다 많은 이미지들이 불려짐 

@[](그래서! 리스트에 보여지는 모든 이미지의 최소 높이는 200정도로 가정하고 테스트를 수행함)  
@[](<img> 태그에 width와 height를 미리 알고 있다면, inline-style를 지정해주면 덜그럭 거리지 않는다.)  

+++

### Before

![main-before-offscreen](https://s3.ap-northeast-2.amazonaws.com/notes-file-uploads/main-offsreen-before-min.png)

+++

### After

![main-after-offscreen](https://s3.ap-northeast-2.amazonaws.com/notes-file-uploads/main-offsreen-after-min.png)

+++

#### 이미지 동적 로딩 예시

![network-example](https://s3.ap-northeast-2.amazonaws.com/notes-file-uploads/network-example.mp4)
@[](AweSome!)

---
## Images


이미지를 압축하는 방법은 크게 2가지 이다.

```
Optimize-images
Oversize-images
```

@[](이미지의 파일 크기 = 총 픽셀 수 X 각 픽셀을 인코딩하는 데 사용되는 바이트 수)  
@[1](이미지 픽셀을 인코딩하는 데 사용되는 바이트의 수를 최적화하는 것)  
@[2](총 픽셀 수를 최적화하는 것)  

---

## optimize-images

+++

### Webp VS other image format
**Webp**는 지원하는 브라우저가 한정적이다!  

```html
<picture>
  <source media="(min-width: 650px)" srcset="img_pink_flowers.jpg">
  <source media="(min-width: 465px)" srcset="img_white_flower.jpg">
  <img src="img_orange_flowers.jpg" alt="Flowers" style="width:auto;">
</picture>
```

@[](지원하는 브라우저를 위해서 사용하는 picture Element Tag)

---

## oversize-images

+++

### 실제 이미지가 큰데, 보이는 뷰는 작은 케이스

![over-size-image](https://s3.ap-northeast-2.amazonaws.com/notes-file-uploads/main-over-size-min.png)
@[](낭비)

+++

파일 업로드 할때에, 이미지 라이브러리를 활용하여 리사이징 진행,
large, middle, small 등 프로젝트에 적용 할만한 사이즈로 
리사이징 하고, Before After 비교

Header에 Content Distposition: attachment; 헤더가 내려오는 이유??

---

## speed-index

일반적으로 브라우저가 로드 이벤트에 도달 할 때까지의 시간의 평균  
즉, 페이지의 보이는 부분이 표시되는 평균 시간입니다.

> 그러나 실제 사용자가 화면에서 주요 컨텐츠를 느낄 수 있는 로드 시점은 아닐 수도 있다.

+++

![speed-index-exam](https://s3.ap-northeast-2.amazonaws.com/notes-file-uploads/speedindex-exam-min.png)

Note : 
그림과 같이 사용자가 컨텐츠를 느끼는 시점은 두 예제에서 다르게 나타난다.
첫번째는 점차적으로 콘텐츠가 로딩이 되지만, 두번째는 점진적인 렌더링이 이루어지지 않고 한번에 로드가 된다.
이는 큰 차이가 있으며 후자가 speedindex가 최적화 되지 않았음을 나타내기도 한다.

+++

### 어떻게 speed index를 최적화 할 수 있나?

* 콘텐츠 효율성 최적화
* 주요 렌더링 경로 최적화

+++


### 콘텐츠 효율성 최적화

```
불필요한 다운로드를 제거
다양한 압축 기술을 통해 각 리소스의 전송 인코딩을 최적화
캐싱 활성화
```

@[1](offsreen images)
@[2](optimize images, oversize images, text compression)
@[3](content caching)

+++

### 주요 렌더링 경로 최적화

Critical Rendering Path (CRP) 조사  
브라우저가 HTML, CSS, Javascript를 화면에 실제 픽셀로 변환하는 단계의 순서  
`렌더링 차단 리소스`에 대한 이해  

+++

Before

speedline으로 측정한 이미지

+++

After

speedline으로 측정한 이미지 (2)

+++

---

### 여러분의 사이트는 어떠신가요 ?

Lighthouse로 개선 전과 후를 비교한 이미지 첨부,
특히 performance 세부 탭을 강조하여 얼만큼 개선이 되었고,
여기서 어떤걸 더 적용하면 더 개선이 되는지에 대한 설명 첨부

---

## QnA

질의 응답

---

## 감사합니다