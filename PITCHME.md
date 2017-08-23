# Web Performance

#### created by BoostCamp_오주의 마법사

---

## Index

#### offscreen-images
#### optimize-images
#### oversize-images
#### speed-index
#### text-compression

---

### Why ?

|Company|성능과 수익과의 관계|
|-------|---------------|
|Bing|페이지가 2초 느려지면 유저당 수익이 4.3% 감소|
|Google|400ms만 느려져도 유저당 검색률이 0.59% 감소|
|Yahoo|400ms만 느려져도 full-page traffic이 5~9% 감소|

Note : 
Bing, Google, Yahoo와 같은 주요 검색 엔진에서 웹 사이트의 성능저하가 비즈니스에 얼마나 영향을 끼치는지에 대한 조사에 따르면, Web Performance는 웹 서비스 업체의 `수익과 직결`되기 때문에 성능을 최적화하여 속도를 개선하는 것이 굉장히 중요하다.

+++

### 웹 성능을 결정하는 3가지 영역

* Back-End
* Network
* Front-End

@[1]("<blockquote><p>Steve souders "서비스 응답 시간의 80~90%는 Front-End에서 소모된다"</p></blockquote>")

+++

### 수익 ? 유저 ?

없지만 그래도 해보자! 나중엔.. 생기겠지..

---?image=assets/pwa-lighthouse.png&size=contain

<h3 style="font-family: Helvetica Neue; font-weight: bold; color:#000000">성능 측정도구</h3>
<h3 style="font-family: Helvetica Neue; font-weight: bold; color:#000000">LIGHT HOUSE</h3>
[https://developers.google.com/web/tools/lighthouse](https://developers.google.com/web/tools/lighthouse/?hl=ko)  

<p style="font-family: Helvetica Neue; font-weight: bold; color:#000000">웹 앱의 품질을 개선하는 오픈 소스 자동화 도구</p>

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