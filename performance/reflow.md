## Reflow & Repaint

![mozilla.org reflow](https://www.youtube.com/watch?v=ZTnIxIA5KGw)

> A repaint occurs when changes are made to an elements skin that changes visibillity, but do not affect its layout.

Repaint는 어떤 DOM element가 layout의 변경이 없이 가시적인 요소가 변경 되었을 때 발생하는 이벤트이다.

outline, background-color 속성을 포함 되는 element를 예로 들면, `Repaint`의 비용은 굉장히 비싸다, `DOM tree`안에 포함된 다른 nodes들을 모두 검사를 해야하기 때문이다. 

`Reflow`는 전체 페이지나 혹은 일부 페이지의 layout들이 변경 되게 되니 좀 더 성능에 크리티컬한 영향을 미친다. 


### Reflow

생성된 DOM 노드의 레이아웃 수치(너비, 높이, 위치 등) 변경 시 영향 받은 모든 노드의(자신, 자식, 부모, 조상(결국 모든 노드)) 수치를 다시 계산하여(Recalculate), 렌더 트리를 재 생성하는 과정이며 Layout 단계에 해당한다.

#### 렌더트리 배치 과정

렌더트리의 각 요소(렌더러)들은 아래와 같은 과정을 거쳐 화면에 배치된다.

* 최상위 렌더러의 위치는 0,0으로 결정되며, 뷰포트(브라우저 창에서 보이는 영역 크기) 만큼의 면적을 갖는다.
* HTML 문서의 <html> 요소에 해당하는 최상위 렌더로로부터 계층적으로 배치된다.
* 배치는 화면의 왼쪽에서 오른쪽, 그리고 위에서 아래 순으로 이루어진다.
* 배치가 순차적으로 진행될 때 나중에 등장하는 요소는 앞서 등장한 요소의 위치나 크기에 영향을 미치지 않는다.

#### Reflow Visualization Video
![mozilla.org reflow](https://www.youtube.com/watch?v=ZTnIxIA5KGw)

#### what forces Layout / Reflow
[what-forces-layout](https://gist.github.com/paulirish/5d52fb081b3570c81e3a)