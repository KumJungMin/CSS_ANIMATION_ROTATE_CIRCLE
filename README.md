# 설명 박스, hover시 변하는 애니메이션

## 1. preview

<img src="https://j.gifs.com/r8rQrk.gif" />


## 2. 코드 분석

### 1) html
- `div< CLASS="square">` : 내부 요소를 감싸는 부모 요소이다.
- `<span> </span>` : `square`의 자식요소, 움직이는 원의 역할을 한다.
- `<div> </div>` : `square`의 자식요소, `p`, `h2`, `a`의 부모역할을 한다.

<br/>

#### (1) 하나의 div에 span, div를 자식 요소로 둔다.
```html
<div class="square">
      <span> </span>     <!-- 움직이는 원1 -->
      <span> </span>     <!-- 움직이는 원2 -->
      <span> </span>     <!-- 움직이는 원3 -->
      <div>              <!-- p, h2, a를 포함하는 부모요소 -->
        <h2>Heading text</h2>
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. 
          Iste officiis unde consequatur nesciunt. Recusandae, quam consectetur reprehenderit assumenda natus non?</p>
        <a href="#none">Read more</a>
      </div>
    </div>
```

<br/><br/>

### 2) css

#### (1) body안의 요소들을 상하좌우 중앙 배치한다.

```css

body {
    margin: 0; 
    display: flex;            /*내부 요소들이 차례로 배치 */
    justify-content: center;  /*내부 요소의 좌우 정렬 상태를 가운데로 설정*/
    align-items: center;      /*요소는 세로 중앙 배치*/
    height: 100vh;            /*웹 크기 변화에 따라 변경되는 반응형 수치*/
  }

```

<br/>

#### (2) 찌그러진 원 만들기
- 부모 요소에서 `position :relative`를 하고 자식 요소를 `position :absolute`를 하면 고정적 위치 설정 가능하다.
- 부모 요소에서 `relative`이라고 안하면, `span`은 `body`를 기준으로 생각한다.
- `border-radius` : `/`를 이용하여 2가지를 %를 입력하면 찌그러진 원을 만들 수 있음
- `transition: 0.5s;` : 이 요소에 이벤트 발생시 0.5초 딜레이를 주고 발생시킨다.

```css

 
.square {
  width: 400px;
  height: 400px;
  position: relative;           /*span의 고정적 위치 설정(1)*/
}

.square span {
  position: absolute;           /*span의 고정적 위치 설정(1)*/
  width: 100%;                  /*부모 요소 안에서 100% -> 부모요소와 같은 크기*/
  height: 100%;
  border: 1px solid #fff;
  border-radius: 40% 60% 65% 35% / 40% 45% 55% 60%;    /* 찌그러진 원 만들기(1) */
  transition: 0.5s;                                    /* 이벤트 딜레이 주기(1)*/
}

```

<br/>

#### (3) 원 <span>들의 이벤트를 설정한다.
- square에 hover시, span에 이벤트가 발생하게 설정한다.
- span들에 이벤트가 발생하면 몇초 후 시작하는 지 설정한다.
- `animation-direction: reverse`을 사용하여 애니메이션 방향을 반대로 만든다.  
- `square:hover span:nth-child(1)`은 square의 hover시 -> span 형제중 첫째에게 어떤 효과가 적용됨.
  
```css

 
.square:hover span {                         /*square를 hover시 border가 없어짐*/
  border: none;
}

.square span:nth-child(1) {                  /*span의 형제 중 1번째*/
  animation: circle 6s linear infinite;      /*circle이벤트   6초   부드러운 애니메이션   무한반복*/
}
.square span:nth-child(2) {
  animation: circle 4s linear infinite;
  animation-direction: reverse;              /* 애니메이션 방향을 반대로도 가능 */
}
.square span:nth-child(3) {
  animation: circle 10s linear infinite;
}


/*square의 hover시 -> span에 어떤 효과가 적용됨.

.square:hover span:nth-child(1) {             
  opacity: 0.3;                              /* 투명도가 낮아짐 */
  background-color: crimson;                 /* 배경색이 변경됨 */
} 
.square:hover span:nth-child(2) {
  opacity: 0.6;
  background-color: darkorange;
}
.square:hover span:nth-child(3) {
  opacity: 0.9;
  background-color: yellowgreen;
}

```





<br/>

#### (4) `keyframe`을 사용하여 요소들이 회전하게 한다.

- CSS 애니메이션에서 구간을 정하고 각 구간별로 다른 스타일을 적용시킬 수 있다.
- `animation-name`은 사용자가 직접 지정한 이름으로, @keyframes 가 적용될 애니메이션의 이름이다.
- `스테이지`는 0~100% 의 구간
- `CSS 스타일`은 각 스테이지(구간)에 적용시킬 스타일

```css
@keyframes circle {
  0% {
    transform: rotate(0);       
  }
  100% {
    transform: rotate(360deg);  /*요소들이 360도 돌게 함*/
  }
}

```

<br/>

#### (5) 글자들을 담는 div에 효과를 준다.

- `div`의 부모인 `square`가 `relative`이므로, `div`에 `absolute`를 주어 고정위치를 준다.
- `top`, `left`에 `50%`를 주고, `translate(-top값, -left값)`을 넣어 항상 중앙에 오게 한다.
- <a href="https://www.biew.co.kr/entry/CSS3-Transform-%EC%86%8D%EC%84%B1-scale-rotate-translate-skew-%EC%8B%A4%EB%AC%B4%EC%98%88%EC%A0%9C-%EC%B2%A8%EB%B6%80%ED%8C%8C%EC%9D%BC-%ED%8F%AC%ED%95%A8">(추천) 깔끔하게 정리된 translate의 속성 설명</a>

```css
.square div {
  position: absolute;                   /*중앙위치(1)*/  
  top: 50%                              /*중앙위치(2)*/  
  left: 50%;                            /*중앙위치(3)*/  
  transform: translate(-50%, -50%);     /*중앙위치(4)*/  
  width: 70%;
  text-align: center;
  color: #fff;
}

.square div a {                         /*링크태그 꾸미기*/  
  color: #fff;
  border: 1px solid #fff;
  border-radius: 40% 60% 65% 35% / 40% 45% 55% 60%;
  padding: 8px;
  text-decoration: none;
}

```
















