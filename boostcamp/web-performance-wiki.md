Web Performance
===============

[발표 PDF 파일](https://github.com/BoostCamp2-OZ/reservation-system/blob/B_OZ/etc/web-performance-oz.pdf)

### 주제 선정 이유
* 우리 팀프로젝트의 성능 문제점에 대한 고민  

### 각 주제에 대한 발표 순서
* 문제점이 왜 일어났을까?  
* 어떠한 방법으로 해결할 수 있을까?  
* Before VS After  


Web performance 측정 도구 : [LIGHTHOUSE](https://developers.google.com/web/tools/lighthouse/?hl=ko)

### Index
* offscreen-images (화면에 보이지 않는 이미지 요청 케이스)
* optimize-images (이미지 최적화)
* oversize-images (이미지가 큰데, 보이는 뷰는 작다.)
* speed-index (화면에서 주요 컨텐츠가 사용자가 느낄 수 있는 로드 시점 측정)
* text-compression (텍스트의 압축)
* options
    * first-interavice
    * first-meaningful
    * 국내 혹은 외국 유명사이트들을 분석해보자 !  
    * 외부의 사이트들은 혹시 어떠한 방법으로 해결 했는가?  

### 발표 조사 자료들
* [Performance의 중요성](https://github.com/BoostCamp2-OZ/reservation-system/wiki/Performance%EC%9D%98-%EC%A4%91%EC%9A%94%EC%84%B1)
* [Web Page 표시의 병목](https://github.com/BoostCamp2-OZ/reservation-system/wiki/Web-Page-%ED%91%9C%EC%8B%9C%EC%9D%98-%EB%B3%91%EB%AA%A9)
* [HTTP 와 Connection Pool](https://github.com/BoostCamp2-OZ/reservation-system/wiki/HTTP%EC%99%80-Connection-Pool)
* HTTPS/HTTP2
* If-modified-since
* Expire
* Chrome 개발자 도구로 성능 측정하기
    - Req, Server Time, Response
    - Performance 패널
* 렌더링 성능
    - [DOM Tree vs Render Tree](https://github.com/BoostCamp2-OZ/reservation-system/wiki/DOM,-CSSOM,-Render-Tree)
    - [Reflow & Repaint](https://github.com/nhnent/fe.javascript/wiki/Reflow%EC%99%80-Repaint)
    - [랜더링 성능 측정하기](https://github.com/BoostCamp2-OZ/reservation-system/wiki/%EB%9E%9C%EB%8D%94%EB%A7%81-%EC%84%B1%EB%8A%A5-%EC%B8%A1%EC%A0%95%ED%95%98%EA%B8%B0)
* 전송 용량 줄이기
    - Concatenation
    - Minification / Obfuscation
    - Gzip
    - [Optimizing Images](https://github.com/BoostCamp2-OZ/reservation-system/wiki/Optimize-images)
* DNS
    - DNS Lookup 과정
    - A, CNAME
    - 도메인 분산 vs 도메인 통합

### QnA
-----
[QnA](https://github.com/BoostCamp2-OZ/reservation-system/wiki/QnA)

### 참고자료
------
* [Google PageSpeed](https://developers.google.com/speed/pagespeed/)
* [Google PageSpeed guide](https://developers.google.com/speed/docs/insights/about)
* [Steve Sourders blog](https://stevesouders.com/)

* [your website design should load in 4seconds](https://www.hobo-web.co.uk/your-website-design-should-load-in-4-seconds/)
* [how to achieve 100/100 in google page speed](https://moz.com/blog/how-to-achieve-100100-with-the-google-page-speed-test-tool)
* [웹 성능 최적화의 오늘과 내일](http://www.dbpia.co.kr/Journal/ArticleDetail/NODE01842945)
* [웹 사이트 속도 개선 방법](http://nuts84.tistory.com/25)
* [웹사이트에 부스트를 달아보자!](https://www.slideshare.net/heungrae_kim/jco-frontend)
* [Critical Rendering Path (구글)](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/?hl=ko);
* [VP, PSI](https://www.instartlogic.com/blog/perceptual-speed-index-psi-measuring-above-fold-visual-performance-web-pages)