# CSS

## ❓Stacking Context(쌓임 맥락)에 대해 설명해 주세요.

Stacking Context는 CSS에서 레이아웃을 구성할 때 요소들이 어떻게 쌓이는지 결정합니다. 요소가 쌓이는 순서에 따라 어떻게 겹쳐지고 투명도, 배경색, z-index등의 속성이 어떻게 적용되는지 결정하는데 영향을 미친다. 또한 z-index보다 부모 요소의 z-index 우선순위가 더 높다.

### z-index가 설정되지 않았을 때 요소들이 쌓이는 순서

z-index 설정이 없다면, 일반적인 HTML 문서 흐름에 따라 쌓이게 된다. HTML 문서의 요소는 부모-자식 관계로 구성되며, 자식 요소가 부모 요소 안에서 먼저 나타나면 그에 속한 자식 요소가 먼저 쌓입니다. 또한, 동일한 부모를 가진 요소들은 나타나는 순서에 따라 쌓입니다.

만약 특정 요소에 position: absolute 또는 position: relative 등의 속성이 적용되어 있고, 그 요소에 z-index 속성이 설정되어 있다면, 이 요소는 일반적인 문서 흐름에서 벗어나고 새로운 스태킹 컨텍스트를 형성하게 됩니다. 이때는 z-index 속성의 값에 따라 Stacking Context(쌓임 맥락)가 결정됩니다.

## ❓Box-model에 대해 설명해 주세요.

CSS에서 Box-Model은 기본적으로 모든 HTML 요소를 감쌉니다. Box-Model의 구성요소로 가장 외각에 `margin` 영역으로 보통 다른 element와 상하좌우 간격을 두기 위해 사용됩니다. 그 다음은 `border` 영역으로 경계선을 그리기 위해 사용되며, 선의 색상과 모양, 두께를 지정할 수 있습니다. 그 아래는 `padding` 영역으로, content와 border 사이의 간격을 지정하기 위해 사용됩니다. 마지막으로 가장 안쪽에 있는 영역은 `content` 영역으로 **width(너비)**와 **height(높이)**를 지정해 줄 수 있습니다.

### Box-sizing

Box-Sizing은 **width(너비)**와 **height(높이)** 속성의 대상 영역을 변경할 수 있습니다.
box-sizing 속성의 기본 값은 `content-box`입니다. 이는 width와 height 프로퍼티 대상 영역이 box-model의 content 영역을 의미합니다. 이렇게 설정할 경우, 반응형 레이아웃 설정 시 상대 단위인 **%**, **rem**을 사용하게 되면 box-model의 앨리먼트 값을 계산하기 매우 어려워지는데, 이를 해결하기 위해 box-sizing 속성에 `border-box`를 통해 해결할 수 있습니다.
`box-sizing: border-box`로 설정하게 되면, **margin**을 제외한 박스 모델 전체(border, padding, content)를 width와 height 속성의 대상 영역으로 지정할 수 있어 CSS Layout을 직관적으로 활용할 수 있습니다.

<image src='../images/box-sizing.png' style="max-width:500px;" alt='box-sizing 이미지'/>

**content-box** : 기본값, width, height 프로퍼티 값은 content 영역을 의미한다.

**border-box** : width, height 프로퍼티 값은 content 영역, padding, border가 포함된 값을 의미한다.

## ❓selector에 대해 설명해 주세요.

- CSS에서 선택자를 통해 특정 요소들을 선택하여 스타일 적용할 수 있는데, 이를 가능하게 하는 것을 `selector` 라고 합니다.

### 종류

#### Universal Selector

```css
* {
  margin: 0;
  text-decoration: none;
}
```

- HTML 페이지 내부의 모든 요소에 같은 CSS 속성을 적용합니다.
- 보통 예제 처럼 margin, padding 초기화 등 기본 값을 정해둘 때 사용합니다.

#### Type Selector

```css
p { background: yellowgreen; color: darkgreen; }

<!-- HTML -->
<p>태그 선택자(Type Selector)</p>
<div>태그 선택자(Type Selector)</div>
```

- HTML 요소를 직접 지칭합니다.

#### Class Selector

```css
.class1 { background: yellowgreen; color: darkgreen; }
div.class2 { background: darkgreen; color: yellowgreen; }

<!-- HTML -->
<p class="class1">클래스 선택자(Class Selector)</p>
<p class="class2">클래스 선택자(Class Selector)</p>
<div class="class2">클래스 선택자(Class Selector)</div>
```

- 주어진 값을 class 속성 값으로 가진 HTML 요소를 찾아 선택합니다.

#### ID Selector

```css
#id1 { background: yellowgreen; color: darkgreen; }
div#id2 { background: darkgreen; color: yellowgreen; }

<!-- HTML -->
<p id="id1">ID 선택자(ID Selector)</p>
<p id="id2">ID 선택자(ID Selector)</p>
<div id="id2">ID 선택자(ID Selector)</div>
```

- 마침표 대신 `#` 를 사용하고 id 속성을 사용합니다.

### 우선순위

- 스타일 우선순위는 아래와 같습니다.
  1. !important 선언을 한 사용자 스타일
  2. !important 선언을 한 제작자 스타일
  3. 제작자 스타일
  4. 사용자 스타일
  5. 사용자 도구 선언

#### 우선 순위 계산법

```css
!important > id [ 100 ] > class [ 10 ] > tag [ 1 ] > * [ 0 ]
```

- !important는 무조건 우선순위가 가장 높습니다.
- 나머지 선택자는 위의 숫자를 각각 점수로 부여하여 계산되고, 높은 순서대로 높은 우선순위를 가지게 됩니다.
- 예를 들자면 `li#selector1 {}` 는 101이라는 값을 가집니다.
- 만약 우선순위가 같다면, 마지막에 지정된 스타일이 우선으로 적용됩니다.
