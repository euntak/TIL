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

|Company|성능과 수익과의 관계|
|-------|---------------|
|Bing|페이지가 2초 느려지면 유저당 수익이 4.3% 감소|
|Google|400ms만 느려져도 유저당 검색률이 0.59% 감소|
|Yahoo|400ms만 느려져도 full-page traffic이 5~9% 감소|

<!-- Note:
Bing, Google, Yahoo와 같은 주요 검색 엔진에서 웹 사이트의 성능저하가 비즈니스에 얼마나 영향을 끼치는지에 대한 조사에 따르면, Web Performance는 웹 서비스 업체의 `수익과 직결`되기 때문에 성능을 최적화하여 속도를 개선하는 것이 굉장히 중요하다. -->

+++

### 웹 성능을 결정하는 3가지 영역

* Back-End
* Network
* Front-End

> Steve souders "서비스 응답 시간의 80~90%는 Front-End에서 소모된다"

+++

### 우리가 만든 사이트는 ?

![mainPage](/assets/main-page-performance.png)

+++ 

### 성능 개선을 해보자 !

* 근데 어떤 것을 개선해야 하지 ?
* 용량을 줄이는건 맞는 것 같은데 얼마나 줄일 수 있지 ?
* 이미지는 얼마나 압축을 해야하는거지 ?
* 최소 비용 최대 효율을 위해 할 수 있는건 뭐가 있을까 ?

무엇 부터 해야하는지 모르는 나란놈..

---?image=/assets/pwa-lighthouse.png&size=contain

<!-- <h2 style="font-family: Helvetica Neue; font-weight: bold; color:#000000">성능 측정도구</h2> -->
<!-- <h3 style="font-family: Helvetica Neue; font-weight: bold; color:#000000">LIGHT HOUSE</h3> -->
### LIGHTHOUSE

[https://developers.google.com/web/tools/lighthouse](https://developers.google.com/web/tools/lighthouse/?hl=ko)  

웹 앱의 품질을 개선하는 오픈 소스 자동화 도구

+++?image=/assets/light-house-exam.png&size=contain


+++

### Google Developer Tools Performance

애플리케이션에서 작업이 실행될 때 모든 작업을 기록하고 분석할 수 있다!

+++?image=/assets/main-page-performance.png&size=contain

---?image=/assets/webpack.png&size=contain

## TEXT-COMPRESSION

+++

### 1. 초기 메인페이지의 로딩이 느리다 !

어떻게 개선을 할 것인가 ?  

```
서버사이드 렌더링 ?
서버와의 요청 횟수를 줄이자
페이지에서 사용하는 리소스들의 크기를 줄이자
안보이는 리소스들을 미리 불러오지 말자
```

@[2-3]
@[](Front-End Performance 향상 = 최소비용 최대효율!)

+++

### Before

* 리소스 크기 : 500KB 이상
* 시간 : 500ms 이상

![main-before-network](/assets/main-before-network.png)

+++

### After

* 리소스 크기 : 180KB
* 시간 : 37ms 

![main-after-network](/assets/main-after-network.png)

<p>리소스 크기 <span style="font-family: Helvetica Neue; font-weight: bold; color:#CC0000">약 64% 감소</span>, 시간 <span style="font-family: Helvetica Neue; font-weight: bold; color:#CC0000">약 92%</span> 감소!</p>

@[](외쳐 갓팩!)

---

## OFFSCREEN-IMAGES

+++


```
서버사이드 렌더링 ?
서버와의 요청 횟수를 줄이자
페이지에서 사용하는 리소스들의 크기를 줄이자
안보이는 리소스들을 미리 불러오지 말자
```

@[4]
@[](화면에 보여지지 않는 이미지들을 미리 요청하지 않는다!)

+++

### 이미지 동적 로딩

**IntersectionObserver**의 활용

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

@[3](InterectionObserver 설정)
@[10-17, 19](DOM이 노출되면 이미지 로딩)
@[17](보여진 이미지는 Observer에서 제거)
@[24-27](화면에 나타나는 것을 감지하기 위한 Element를 등록)

+++

### 예상치 못한 결과

이미지가 채워지기 전 DOM이 기준이 되어버려서 생각보다 많은 이미지들이 불려짐

그래서! 리스트에 보여지는 모든 이미지의 최소 높이는 200정도로 가정하고 테스트를 수행했습니다.

@[](우리에게 필요한건... 스피드!)

+++

### Before

![main-before-network](/assets/main-before-network.png)

+++

### After

![main-after-network](/assets/main-after-network.png)

+++?video=http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4

+++?video=https://player.vimeo.com/video/230771105

<p>리소스 크기 <span style="font-family: Helvetica Neue; font-weight: bold; color:#CC0000">약 64% 감소</span>, 시간 <span style="font-family: Helvetica Neue; font-weight: bold; color:#CC0000">약 92%</span> 감소!</p>

@[](외쳐 갓팩!)

---

## optimize-images

+++

### 압축 (1)

+++

### 압축 (2)

---

## oversize-images

+++

### 실제 이미지가 큰데, 보이는 뷰는 작은 케이스 (1)

+++

### 실제 이미지가 큰데, 보이는 뷰는 작은 케이스 (2)

---

## speed-index

+++

### 화면에서 주요 컨텐츠가 사용자가 느낄 수 있는 로드 시점 (1)

+++

### 화면에서 주요 컨텐츠가 사용자가 느낄 수 있는 로드 시점 (2)

---

### 여러분의 사이트는 어떠신가요 ?

---

## QnA

---

## 감사합니다