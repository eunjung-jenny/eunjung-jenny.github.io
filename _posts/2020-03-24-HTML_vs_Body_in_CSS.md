---
title: "CSS에서 HTML과 Body의 차이"
date: 2020-03-24 11:32
tags: [CSS, Front-end]
---

- [HTML vs Body in CSS](https://css-tricks.com/html-vs-body-in-css/) 포스팅을 통해 <html> 과 <body> 의 차이에 대해 알아보았다.

## # HTML 과 Body 의 관계

```html
<html>
  <head>
    <!-- Metadata -->
  </head>
  
  <body>
    <!-- content -->
  </body>
</html>
```

- <body> 는 <html> 의 자식 요소
  - 가장 위에 작성된 <html> 은 document 의 root element 
  - <html> 안쪽에는 <head> 와 <body> 만 존재

- <html> 의 또 다른 선택자 `:root` 는 `html` 태그 선택자보다 우선순위가 높고 ID 선택자보다 낮다고 한다.

## # global styles 를 <html> 에 적용하면 되는가?

- <html> 이 <body> 의 부모 요소라면 모든 global styles 를 <html> 에 적용시키면 되겠다 라고 생각할 수 있으나, 다음의 속성들은 명세상 <body> 에 바로 적용시키게끔 되어있다.
  - `background`
  - `background-color`
  - `margin` (top, bottom, left, right)
  - `font`

## # 그렇다면 global styles 는 <body> 에?

- *그렇지도 않은데,* style 을 <html> 에 적용하는 게 더 적합한 경우는 다음과 같다.

### 글씨 크기 조절을 위해 rem units 을 사용하는 경우

- `rem` 은 root 요소인 <html> 의 글씨 크기에 대한 상대 단위로, [<html> 의 글씨 크기 자체를 변경](https://snook.ca/archives/html_and_css/font-size-with-rem)해줌으로써 `rem` 을 보다 편하게 사용할 수 있다.

  - 글씨 크기로 `px 단위`를 사용하는 경우, IE 에 한하여 글씨 크기 조절 기능이 없어 유저 경험이 저하된다.
  - <body> 의 default 글씨 크기를 조절한 뒤 `em 단위`( **부모 요소**의 글씨 크기에 대한 상대 단위 ) 를 사용하는 접근 방식의 경우, 자식 요소에 영향을 주게 된다. (compounding issue)

  ```css
  body { font-size: 62.5%; } /* 1em = 1px */ 
  h1 { font-size: 2.4em; } /* = 24px */
  li { font-size: 1.4em; } /* = 14px */
  li li { font-size: 1.4em; } /* ? */
  ```

  - <html> 의 default 글씨 크기를 조절한 뒤 `rem 단위` 를 사용하는 접근 방식의 경우, 부모 요소의 영향을 제거할 수 있다. ( IE9 : rem 단위 지원)

  ```css
  html { font-size: 62.5%; } /* 1em = 1px */
  li { font-size: 1.4rem; } /* = 14px */
  li li { font-size: 1.4rem; } /* = 14px */
  ```

  - rem 단위를 지원하지 않는 브라우저를 고려한다면, 아래와 같이 px 단위를 병기하는 것이 좋다. (이 때, rem 단위를 아래에 작성!)

  ```css
  html { font-size: 62.5%; }
  li { font-size: 14px; font-size: 1.4rem; }
  ```

### background-color ??

- <html> 에 background-color 속성을 적용한 경우, <body> 의 background-color 속성은 <div> 와 마찬가지로 자식 요소들이 차지하는 영역만큼만 채워지게 된다.
- 반대로 <html> 에 background-color 속성을 적용하지 않는 경우, <body> 의 background-color 속성은 전체 viewport 영역을 채우게 된다.
- [reference](https://css-tricks.com/just-one-of-those-weird-things-about-css-background-on-body/)

```html
<!-- 다음의 코드에서 html 선택자 부분을 주석처리하고 비교해 볼 것 -->
<html>
  <head>
  </head>
  <body>
    <style>
      html { background-color: grey; }
      body { background-color: skyblue; }
    </style>
    Hello world!
  </body>
</html>
```



## # Wrap-up

- JS 를 사용할 때에도 차이가 있다.
  - `document.rootElement` | `document.body`
- 더 자세한 내용은 언제나 문서를 참고.

## # 개인평

- 그동안 css 작업을 하면서 대충 넘어가던 부분을 살펴보았는데 개인적인 소감으로는 `rem 단위` 로 글씨 크기를 조절하는 테크닉이 새롭고 유익했다.

