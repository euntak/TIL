# Web Performance

#### created by BoostCamp_오주의 마법사

---

## Index

#### Why ?
#### Offscreen-images
#### Optimize-images
#### Oversize-images
#### Speed-index
#### Text-compression

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
* 용량은 어떻게 줄일수 있을까 ?
* 이미지는 어떻게 lazy-loading을 해야 할까?
* 최소 비용 최대 효율 !

막막 ... 할게 너무 많다!

---?image=assets/pwa-lighthouse.png&size=contain

<h2 style="font-family: Helvetica Neue; font-weight: bold; color:#000000">성능 측정도구</h2>
<h3 style="font-family: Helvetica Neue; font-weight: bold; color:#000000">LIGHT HOUSE</h3>

[https://developers.google.com/web/tools/lighthouse](https://developers.google.com/web/tools/lighthouse/?hl=ko)  

웹 앱의 품질을 개선하는 오픈 소스 자동화 도구

+++?image=assets/light-house-exam.png&size=contain


+++

### Google Developer Tools Performance

애플리케이션에서 작업이 실행될 때 모든 작업을 기록하고 분석할 수 있다!

+++?image=/assets/main-page-performance.png&size=contain

---

## offscreen-images

![LIGHTHOUSE](/assets/pwa-lighthouse.png)

+++

### 화면에 보이지 않는 이미지 요청 케이스
![HTTP1-HTTP2](/assets/http1-http2.png)

Note:
Remember to explain why it's for everyone: no sign-up, nothing to install.
Just MD. Then git-commit.

+++

### 화면에 보이지 않는 이미지 요청 케이스 (2)

+++?image=/assets/render-tree-construction.png&size=50% 50%

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

## text-compression

+++

### 텍스트가 압축되지 않은 것 같다 (1)

+++

### 텍스트가 압축되지 않은 것 같다 (2)

---

### 여러분의 사이트는 어떠신가요 ?

---

## QnA

---

## 감사합니다