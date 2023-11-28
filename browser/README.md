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
<!DOCTYPE html>
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
<!DOCTYPE html>
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
<!DOCTYPE html>
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
<!DOCTYPE html>
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

## ❓requestAnimationFrame이 무엇인가요?

`requestAnimationFrame`은 Web API가 제공하는 함수로, 애니메이션과 같은 주기적인 렌더링을 최적화하는 데 유용하게 쓰입니다.

## ❓requestAnimationFrame은 왜 사용하나요?

`setInterval`과 같은 타이밍 함수는 모니터 주사율을 전혀 고려하지 않고 실행됩니다. 예를 들어 1초에 60프레임을 출력할 수 있는 모니터의 주사율에 맞추기 위해 `setInterval`의 주기를 `1000/60(16.66ms)`으로 설정해도 무거운 작업으로 인해 실행이 밀려서 프레임이 유실될 수 있습니다. 하지만 `requestAnimationFrame`은 브라우저가 프레임을 렌더하기 위한 페인트 단계에 들어서기 전에 항상 호출되어 모니터 주사율에 맞게 프레임을 출력하므로 부드러운 애니메이션을 구현할 수 있습니다.

또한 브라우저의 탭이 비활성 상태일 경우 애니메이션이 일시 정지되어서 자원을 아낄 수 있고 모바일 기기의 경우 배터리 소모를 줄일 수 있습니다.

## ❓requestAnimationFrame의 실행을 중단시키려면 어떻게 해야 하나요?

`requestAnimationFrame`은 양의 정수인 `requestID`를 반환하는데 이를 `cancleAnimationFrame`에 넘겨서 실행하면 애니메이션을 중단할 수 있습니다.

```js
let rafId;
let tick = 0;

function animate() {
  // tick이 5가 되면 중단
  if (tick === 5) return cancelAnimationFrame(rafId);

  tick += 1;
  rafId = requestAnimationFrame(animate);
}

requestAnimationFrame(animate);
```

## ❓requestAnimationFrame과 setTimeout의 콜백은 각각 어느 큐에서 처리되나요?

`setTimeout`은 태스크 큐, `requestAnimationFrame`은 애니메이션 프레임에서 처리됩니다. 우선순위는 마이크로 태스크 큐, 애니메이션 프레임, 태스크 큐입니다.

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

## ❓스켈레톤UI가 무엇인가요?

스켈레톤UI란 실제 데이터가 엔더링 되기 전에 보이게 될 컨텐츠의 윤곽을 먼저 그려주는 **로딩 애니메이션**입니다. 스켈레톤UI로 사용자의 이탈을 막고, 어떤 컨텐츠들이 보여질지 미리 알려주는 효과가 있습니다.

- ex) 스케레톤UI와 스피너
  <image src="../images/skeleton-ui.png" style="max-width:500px;"/>

스피너 또는 스켈레톤UI 사용 용도는 로딩 지속시간, 컴포넌트의 크기 등의 상황과 필요에 따라 사용됩니다.
<image src="../images/loading-animation.png" style="max-width:500px;"/>

[출처 블로그] https://bitly.ws/WUuD

## ❓주소창에 www.google.com을 입력했을 때 일어나는 과정을 설명해 주세요.

주소창에 해당 도메인 주소를 입력시 화면에 웹 브라우저가 출력되는 과정을 크게 두 가지로 나눠서 설명할 수 있습니다.
서버로 부터 데이터를 받아오는 과정(1~5 단계)과 받아온 데이터를 웹 브라우저에 렌더링 되는 과정(6~12단계)으로 나눠서 설명드릴 수 있습니다.

**데이터를 받아오는 과정**

1. www.google.com을 입력하면 입력한 URL 주소 중, 도메인 이름에 해당하는 google.com을 DNS 서버에서 검색합니다.

웹브라우저는 DNS 서버에 검색하기 전에 캐싱된 DNS 기록들을 먼저 확인합니다. 해당 도메인에 맞는 IP 주소가 존재하면, DNS 서버에 IP 주소 요청을 하지 않고 캐싱된 IP 주소를 바로 반환합니다. 일치하는 IP주소가 존재하지 않다면, DNS 서버 요청을 하게 됩니다.

2. 가장 가까운 DNS 서버에서 해당 도메인 이름에 해당하는 IP 주소를 찾아 사용자가 입력한 URL 정보와 함께 전달합니다.

DNS 서버가 호스팅하고 있는 서버의 IP 주소를 찾기 위해 DNS 쿼리를 전달합니다.
DNS 쿼리는 현재 DNS 서버에 원하는 IP 주소가 존재하지 않으면 다른 DNS 서버를 방문하는 과정을 IP 주소 찾을 때까지 반복합니다.

해당 도메인 이름에 맞는 지역 DNS를 탐색하며, root DNS 서버가 나올 때까지 거꾸로 탐색합니다.

> Ex) . -> com -> google.com

이와 같이 Local DNS 서버가 여러 DNS 서버를 차례대로 물어봐서 답을 찾는 과정을 Recursive Query라고 부릅니다.

(특정 도메인(www.google.com)의 IP 주소를 알아내는 과정에 대한 자세한 설명은 network/DNS look-up 과정 설명 참고할 것)

3. 전달 받은 IP 주소를 이용하여 웹 브라우저는 웹 서버서버와의 TCP 연결을 시작합니다.

해당 HTTP 요청 메세지는 TCP/IP 프로토콜을 사용하여 서버로 전송됩니다. TCP는 전송 제어 프로토콜로 데이터의 전송을 제어하고 데이터를 어떻게 보낼지, 어떻게 맞출지 결정합니다. IP의 특징인 `비신뢰성`과 `비연결성`으로 인해 IP 프로토콜 만으로는 통신을 할 수 없습니다. 그렇기에, 신뢰성과 연결성을 책임지는 TCP를 활용하여 통신을 합니다.

### UDP를 통한 연결

4. 웹 브라우저가 HTTP 요청을 서버로 전송합니다. (필요한 경우, HTTPS 보안 통신이 진행됩니다.)

전달 과정에서 status code를 통해

서버 요청에 따른 결과 및 상태를 전달합니다.

> 1xx : 정보가 담긴 메세지
> 2xx : response 성공
> 3xx : 클라이언트를 다른 URL로 리다이렉트
> 4xx : 클라이언트 측에서 에러 발생
> 5xx : 서버 측에서 에러 발생

HTTP 통신은 80번 포트를 사용합니다. 브라우저는 URL에 명시된 포트 번호가 없는 경우 기본적으로 80번 포트를 사용합니다.

만약 사용자가 입력한 주소가 https로 시작하면, 브라우저는 HTTPS 프로토콜을 사용하여 안전한 통신을 위해 443번 포트를 사용하게 됩니다. HTTPS는 데이터를 암호화하여 보안을 강화하는 프로토콜입니다.

5. 웹 서버가 요청을 처리하고 응답을 다시 웹 브라우저로 전송합니다.

6. 웹 브라우저가 전송 받은 HTML, CSS, JS 파일들을 웹 브라우저에 출력합니다.

웹 브라우저에 출력되는 단계를 `Critical Rendering Path`라고 합니다.

![critical-rendering-path](https://images.velog.io/images/eommoonjoo/post/061458db-0894-4ca0-8e55-bd0b362504e1/Critical-Rendering-Path.png)

7. DOM 트리 빌드

이전 단계에서 통신을 통해 받아온 HTML 파일들은 바이트 형태로 전달 받았다. 아래의 단계를 통해 객체 모델로 전환하는 작업이 수행되고 해당 프로세스의 최종 출력은 DOM(Document Object Model)이며 해당 형태가 트리 형태를 띄고 있기 때문에 `DOM Tree`라고 부릅니다.

-> 요약: 바이트 → 문자 → 토큰 → 노드 → 객체 모델

1. 변환 (바이트 → 문자)

- 바이트 형태의 파일을 지정된 인코딩에 따라 개별 문자로 변환합니다.

2. 토큰화 과정 (문자 → 토큰)

- “<” 문자를 만나면 상태를 태그 열림으로 변하고, 이후 만나는 a~z의 문자들은 “>” 문자를 만날때까지 태그 이름의 상태로 인식하게 됩니다.
- ">" 문자를 만난 후, 현재 토큰을 발행되고 상태는 다시 자료로 돌아갑니다.
- 이후 문자들을 소비하면서 문자 토큰이 생성되고 해당 과정은 "<" 문자를 만날 때까지 진행됩니다.
- "<" 문자에 만나면 다시 태크 열림 상태로 변합니다.
  "/" 문자는 종료 태그 토큰을 생성하며 태그이름 상태로 변경됩니다.
  해당 상태는 ">" 문자를 만날때까지 유지됩니다.
- 해당 과정을 모든 파일의 자료를 확인할 때까지 반복합니다.

3. 렉싱 (토큰 -> 노드)

- 생성된 토큰들을규칙 및 속성에 맞는 객체로 변환시킵니다.

4. DOM 생성 (노드 -> 객체 모델)

- 생성된 객체는 트리 데이터 구조로 연결이 됩니다.
- 해당 트리 데이터 구조는 원래 마크업에서 정의된상위-하위 관계도 포함이 됩니다.

<image src="../images/dom-tree.png" style="max-width:500px;">

8. CSSOM 트리 빌드

HTML에서 사용했던 객체 모델로 전환하는 작업이 CSS 파일에 똑같이 적용됩니다.
CSSOM(CSSObjectModel) 트리 형태를 만듦으로써 특정 객체에 최종 스타일로 계산할 때 상위 객체의 스타일을 하향식 규칙을 적용하는 방식으로 계산되는 스타일을 재귀적으로 세분화하게 됩니다.

<image src="../images/cssom.png" style="max-width:500px;">

9. Render Tree 생성

기존에 제작된 DOM과 CSSOM을 결합하여 Render Tree를 생성합니다.
Render Tree는 렌더링에 필요한 노드만 선택하여 페이지를 렌더링하는데 사용합니다.
Render Tree가 만들어지는 과정을 간략히 설명하면, Docoument 객체부터 각 노드를 순회하면서 각각에 맞는 CSSOM을 찾아 적용합니다. 그러면서 렌더와 관련된 요소들은 Render Tree에 포함시킨다.
\*meta 태그나 `display:none` 속성을 가진 요소들은 렌더와 관련이 없으므로 포함되지 않음.

<image src="../images/render-tree.png" style="max-width:500px;">

10. Layout

Render Tree의 노드들에 대해서 ViewPort 내에서 요소들에 정확한 위치와 크기를 계산하는 단계이다. 페이지 상에 존재하는 객체의 크기를 렌더링 트리의 루트부터 시작하여 모든 객체의 정확한 위치와 크기를 계산합니다.

11. Paint

Layout 과정에서 렌더링 엔진이 각 요소들이 어떻게 생겼고 어떻게 보여줄지 알게 되면 화면에 실제 px로 변환하는 과정을 거친다.
계산된 값들을 기반으로 화면에 필요한 요소들인 텍스트 이미지들이 실제로 그리는 작업을 실행합니다.
Layout 단계에서 계산된 모든 위치, 크기를 실제 px로 변환하여 화면에 출력합니다.

\*css에서 %나 em과 같은 상대적인 단위를 사용했을 때에는 Viewport에 맞춰 px단위로 변환됩니다.

### Reflow & Repaint

특정 액션과 이벤트에 따라 html의 요소와 크기나 위치의 크기를 변경해야 하는 경우가 발생하며 해당 과정을 reflow라고 합니다.

<aside>
💡 **Reflow 되는 경우**
- 페이지 초기 렌더링 시 (최초 Layout 과정)
- 윈도우 Resizing 시 (Viewport 크기 변경 시)
- 노드 추가 또는 제거
- 요소의 위치, 크기 변경 (left, top, margin, padding, border, 등등)
- 폰트 변경(텍스트 내용)과 이미지 크기 변경(크기가 다른 이미지로 변경 시)

</aside>

- 해당 과정이 발생하면 렌더링 트리와 각 요소들의 크기와 위치를 다시 계산해야 한다.
- reflow에 따라 다시 페인팅을 해줘야 하는 repaint 단계 역시 수행됩니다.
  - 즉, `repaint`는 `reflow`보다는 비교적 낮은 비용을 가지고 있다.

12. Composition

Layout과 Paint는 수행하지 않고 Layer의 합성만 실행시키는 단계이다.

13. 화면이 웹 브라우저에 출력됩니다.
