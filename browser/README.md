# Browser

## ❓크로스 브라우징이란 무엇인가요?

- 크로스브라우징이란 "웹페이지의 상호 호환성"을 의미하며, 다른 기종 혹은 플랫폼에 따라 다르게 구현되는 기술을 어느 한 쪽에 치우치지 않도록 공통요소를 사용하여 웹페이지를 제작하는 기법을 말합니다.

- 크로스 브라우징은 모든 브라우저에서 100% 똑같이 보이도록 만드는 동일성이 아닌, 동등성(등가성)을 의미하는 것이 중요합니다.
  그 이유는 브라우저마다 렌더링 엔진이 다르기 때문에 완전히 똑같이 보이도록 하는 것은 사실 불가능한 부분이 있기 떄문입니다.

- 따라서 크로스 브라우징을 고려할 때에는 서비스를 사용하는 사용자의 브라우저 점유율에 따라 우선적으로 적용하는 것이 좋습니다.

참고 자료

- Cross Browsing 가이드: https://modernizr.com/

## ❓CSS를 통한 방법 외에 크로스 브라우징을 달성하기 위한 방법엔 무엇이 있나요?

## ❓브라우저의 렌더링 과정(CRP)에 대해 설명해 주세요.

### Renderer Process

#### Main Thread

- parse: HTML을 분석해 DOM 트리를 생성합니다.
- style: CSS를 분석해 computed style을 계산하고 CSSOM을 생성합니다.
- layout: 각 요소가 어떤 크기로 어디에 위치해야 할지 계산하고 fragment 트리라는 변경불가능한 트리를 생성합니다.
- pre-paint: 실제로 화면에 그려지기 전에 다시 그려져야 할 요소를 결정합니다.
- paint: 실제로 화면을 그리는 과정이 아니라 어떻게 그려야 할지 계획하는 단계입니다. DOM의 배치 순서가 아니라 Stacking order(쌓임 맥락)에 따라 Fragment tree를 순회합니다.
- layerization: 어떤 DOM 요소가 자체 Graphics Layer에 들어갈지 파악하는 단계입니다.

#### Compositor Thread

- commit: Layerization로 생성된 composited layer list는 pre-paint의 Property Tree와 함께 합성 스레드로 복사됩니다.
- tiling: 커밋을 한 후에 pending tree에서 받은 레이어를 더 작은 타일로 분할하는 작업입니다.
- raster: draw 명령어를 실행하는 과정입니다. Blink는 Skia라는 그래픽 라이브러리를 사용하여 비트맵 이미지를 생성하고 이를 GPU 메모리에 저장합니다.
- activate: 쿼드들을 Compositor Frame이라는 하나의 데이터로 묶고, GPU 프로세스로 전송합니다.

### Viz Process

- aggregate: 전송된 Compositor Frame들을 최종적으로 하나로 합치는 과정입니다.
- draw: GPU 프로세스의 메인 스레드에 있는 그래픽 라이브러리 SKIA에 전달해 DrawQuad,OpenGL, Vulkan 같은 명령어를 실행합니다. 그러면 GPU가 화면에 픽셀을 그리면서 한 프레임이 제출되고 렌더링 파이프라인이 끝납니다.

## ❓리플로우와 리페인트가 무엇인가요?

- 리플로우: 레이아웃 과정부터 다시 진행하는 것을 말합니다.
- 리페인트: 페인트 과정부터 다시 진행하는 것을 말합니다.

## ❓리플로우와 리페인트 중 비용이 더 적게 드는 것은 무엇인가요?

리페인트는 페인트 단계만 다시 수행하기 때문에 레이아웃 단계부터 다시 수행하는 리플로우보다 비용이 적게 듭니다.

## ❓애니메이션을 구현할 때 발생하는 많은 리플로우와 리페인트를 최대한 최적화하려면 어떻게 해야 할까요?

CSS로 구현한 애니메이션이라면 `will-change` 속성을 사용해서 브라우저에게 특정 속성이 변경될 것임을 미리 알려줄 수 있고, 스크립트로 구현한 애니메이션이라면 `requestAnimationFrame`을 사용해 안정적인 fps를 보장할 수 있습니다.

## ❓script 태그의 속성인 async과 defer의 용도는 무엇이고 어떤 차이가 있나요?

브라우저의 HTML 파서는 HTML 파싱 도중 `<script>` 태그를 만나면 파싱을 중단하고 스크립트를 로딩합니다. `async`와 `defer`는 HTML 파싱을 중단하지 않고 비동기적(병렬적)인 스크립트 로딩을 하도록 하는 속성입니다. 두 속성은 스크립트의 로딩 완료 후 동작에서 차이가 있습니다.

### async

`async` 속성 사용시 스크립트 로딩이 완료되면 HTML 파싱이 중지되고 스크립트가 실행되며 스크립트 실행이 종료되면 중지됐던 파싱을 다시 시작합니다.  
HTML 파싱이 완료되기 전에 스크립트가 실행되므로 아직 생성되지 않은 DOM요소를 참조하는 코드가 스크립트에 있다면 오류가 발생할 수 있으며, 스크립트를 실행하는 동안 HTML 파싱을 중지하므로 사용자가 최소한의 콘텐츠를 보는 시점이 지연됩니다.

```html
<!doctype html>
<html lang="en">
  <head>
    <title>Document</title>
    <script async src="main.js"></script>
  </head>
  <body>
    ...
  </body>
</html>
```

<image src='../images/script-async-single.png' />

#### async 속성의 스크립트가 여러개인 경우

정의된 스크립트 태그의 순서에 상관없이 다운로드가 먼저 완료된 순서대로 실행됩니다. 각각의 스크립트 파일이 실행될 때마다 HTML은 중지와 재시작을 반복하게 되며, 스크립트 파일이 순서에 의존적이어서 `b.js`의 올바른 실행에 `a.js`가 필요한 상황이라면 문제가 생길 수 있습니다.

```html
<!doctype html>
<html lang="en">
  <head>
    <title>Document</title>
    <script async src="a.js"></script>
    <script async src="b.js"></script>
    <script async src="c.js"></script>
  </head>
  <body>
    ...
  </body>
</html>
```

<image src='../images/script-async-multi.png' />

- a, b, c 순서로 정의되었지만 로드가 완료된 순서인 b, a, c로 실행됩니다.

### defer

`defer` 속성 사용시 스크립트 로드가 일찍 완료되어도 HTML 파싱이 완료되기까지 기다렸다가 실행됩니다.  
HTML 파싱을 중단시키지 않아 최소한의 콘텐츠를 빨리 보여줄 수 있지만 문서에 스크립트에 의존하는 콘텐츠가 많다면 완전한 콘텐츠를 보여주는 시점이 지연될 수 있다는 단점이 있습니다.

```html
<!doctype html>
<html lang="en">
  <head>
    <title>Document</title>
    <script defer src="main.js"></script>
  </head>
  <body>
    ...
  </body>
</html>
```

<image src='../images/script-defer-single.png' />

#### defer 속성의 스크립트가 여러개인 경우

HTML 파싱이 완료되는대로 스크립트가 정의된 순서에 따라 실행됩니다.

```html
<!doctype html>
<html lang="en">
  <head>
    <title>Document</title>
    <script defer src="a.js"></script>
    <script defer src="b.js"></script>
    <script defer src="c.js"></script>
  </head>
  <body>
    ...
  </body>
</html>
```

<image src='../images/script-defer-multi.png' />

- 로드는 b, a, c 순서로 완료되었지만 실행은 정의된 순서에 따라 a, b, c 순서로 됩니다.

## ❓display: none, opacity: 0, visibility: hidden은 무슨 차이가 있나요?

|                      | 영역 차지 | 클릭 가능 | tab focus | 스크린 리더 |       변경시       |
| -------------------- | :-------: | :-------: | :-------: | :---------: | :----------------: |
| `display: none`      |     X     |     X     |     X     |      X      |      리플로우      |
| `visibility: hidden` |     O     |     X     |     X     |      X      |      리페인트      |
| `opacity: 0`         |     O     |     O     |     O     |      O      | 초깃값에 따라 다름 |

### ⚠️ opacity 초깃값에 따라 다른 렌더링 파이프라인 동작

- `opacity: 1` -> `opacity: 0`은 리플로우가 발생하고
- `opacity: 0.99` -> `opacity: 0`은 리페인트가 발생합니다.

`opacity`가 1이 아닌 요소는 새로운 쌓임맥락(stacking context)을 생성해서 애초에 다른 레이어에 속하므로 `opacity` 속성이 변한다해서 레이아웃 과정을 다시 할 필요가 없기 때문입니다.

- `will-change: opacity`를 사용하면 `opacity`의 초깃값이 1이어도 리플로우나 리페인트가 발생하지 않고 compositor 스레드에서 작업을 처리합니다.
- `animation`을 사용하면 리플로우나 리페인트 없이 GPU와 compositor 스레드에서 작업을 처리합니다.

## ❓웹사이트 성능 최적화에는 어떤 방법이 있나요?

웹사이트의 성능 최적화 방법은 크게 `로딩 성능`을 개선하는 것과 `렌더링 성능`을 개선하는 방법이 있습니다.

### 로딩 성능 개선

로딩 성능은 서버에서 웹 페이지에 필요한 HTML, CSS, Javascript, 미디어 소스(Image, Video) 등의 리소스를 다운로드할 때의 성능을 말합니다.

1. 렌더 블로킹 최적화

- CSS는 head, JS는 body 하단에서 불러온다.
- async/defer 속성 사용

```js
<script async src="test.js"></script> // async: 다운로드 후 즉시 실행
<script defer src="test.js"></script> // defer: 웹페이지가 모두 그려지고 DOM이 들어왔을 때 스크립트를 실행

<script></script> // 반드시 순서대로 실행되어야 할때
<script async></script> // 빨리 실행되어야 할때
<script defer></script> // 마지막에 파싱해도 상관없을때
```

2. 이미지 최적화

- 이미지 지연 로딩: 첫 화면에 보여지는 이미지가 아닐 경우 img tag에 lazy속성을 주어 다운로드 할 리소스를 제거한다.

### 렌더링 성능 개선

렌더링 성능을 페이지에서 화면에 주요 리소스가 페이지에 그려질 때의 성능을 말합니다.

1. CSS 최적화

- 리플로우(레이아웃에 영향을 줘서 레이아웃부터 다시 그리는 것)와 리페인트(레이아웃에 영향을 주지 않는 속성을 변경하여 레이아웃을 건너뛰고 페인트 작업부터 수행하는 것)을 고려하여 스타일 작성
- 사용하지 않는 css 제거

2. HTML 최적화

- inline-style을 사용하지 않는다.
- 복잡한 DOM tree 지양한다. dom이 작고 얕을수록 계산이 빠르기 때문입니다.
- 불필요하게 감싸는 요소 제거

3. 에니매에션 최적화

- `transform`은 리플로우와 리페인트 모두 발생시키지 않고 합성만 발생시키는 속성이기 때문에 애니메이션에서 사용 시 렌더링 속도를 향상시킬 수 있다.
- position 설정 시 absolute나 fixed로 설정하면 주변 요소에 영향을 주지 않는다.

### 웹페이지 최적화 도구

Google에서 제시한 성능지표인 Core Web Vital에 대해 알아보겠습니다.

<image src="../images/core-web-vitals.png" style="max-width:500px;">

**콘텐츠가 포함된 최대 페인트(LCP, Largest Contentful Pain)**

LCP는 페이지에서 핵심 요소의 콘텐츠 로딩 속도를 측정합니다.
가장 큰 요소는 보통 이미지나 동영상이다.

**첫 입력 지연(FID, First Input Delay)**

사용자가 페이지와 **처음 상호 작용(링크 클릭이나 버튼 클릭)할 때, 이에 대한 응답**으로 브라우저가 이벤트를 처리하기 까지의 시간을 측정합니다.

FID에 영향을 미치는 것은 긴 태스크, JavaScript의 실행 시간, JavaScript의 번들, 렌더 블로킹 등이 있습니다.

**누적 레이아웃 변경(CLS, Cumulative Layout Shift)**

CLS는 페이지 방문자의 시각적인 안정성을 측정합니다. 사용자에게 웹 페이지가 표시되고 최종적으로 변화되는 정도를 측정한 수치입니다.
예를 들어 `갑자기 광고나 다른 요소가 나오며 원하지 않은 요소를 클릭하는 속임수를 방지`하기 위한 척도로 쓰입니다.
