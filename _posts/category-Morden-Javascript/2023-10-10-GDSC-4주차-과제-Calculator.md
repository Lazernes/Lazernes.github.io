---
title: "GDSC 4주차 과제 Calculator"
excerpt: "."

wirter: Myeongwoo Yoon
categories:
  - Modern Javascript
tags:
  - Programing

toc: true
toc_sticky: true
 
date: 2023-10-10
last_modified_at: 2023-10-10
---
HTML
======
<p align="center"><img src="/assets/img/Modern-Javascript/GDSC-4주차/GDSC_Week4_1.jpg" width="300" height="500"></p>

다음과 같은 계산기 구조를 만들기 위해서 index.html을 다음과 같이 작성해주었다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Calculator</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="calculator">
    <div class="display">
      <div class="input"></div>
    </div>
    <div class="buttons">
      <button class="num">7</button>
      <button class="num">8</button>
      <button class="num">9</button>
      <button data-type="operator" class="operator">/</button>
      <button class="num">4</button>
      <button class="num">5</button>
      <button class="num">6</button>
      <button data-type="operator" class="operator">x</button>
      <button class="num">1</button>
      <button class="num">2</button>
      <button class="num">3</button>
      <button data-type="operator" class="operator">-</button>
      <button class="num">0</button>
      <button data-type="ac" class="ac">AC</button>
      <button data-type="result" class="result">=</button>
      <button data-type="operator" class="operator">+</button>      
    </div>
  </div>
</body>
<script src="script.js"></script>
</html>
```

연산자는 **operator**, 숫자는 **num**, 초기화는 **ac**, 결과(=)는 **result**라는 클래스를 각각 주었다. 위 HTML파일을 완성하면 다음과 같이 나타난다.<br/>
<p align="center"><img src="/assets/img/Modern-Javascript/GDSC-4주차/GDSC_Week4_2.png"></p>


CSS
======
위 버튼들의 배열을 CSS를 통해 배치해보고 버튼에 색을 넣어본다. 또한 피연산자나 결과가 보일수 있게 display 박스를 만든다.<br/>

```css
.calculator {
  margin: 100px auto;
  width: 350px;
  height: 600px;
  border: 1px solid #ccc;
  background-color: #000;
}

.input {
  height: 20px;
  text-align: right;
}

.display {
  display: flex;
  flex-direction: column;
  height: 130px;
  font-size: 50px;
  border: 1px solid #fff;
  margin: 5px 5px 0 5px;
  padding: 10px;
  border-radius: 5px;
  background-color: #000;
  color: #fff;
}

.buttons {
  padding: 5px;
  display: grid;
  height: calc(100% - 170px);
  gap: 5px;
  grid-template-columns: repeat(4, 1fr);
}

button {
  color: #000;
  font-size: 36px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.num {
  background-color: #fff;
}

.operator,
.result,
.ac {
  background-color: orange;
}
```
위 파일에서 **grid-template-columns: repeat(4, 1fr);**는 컨테이너를 열 방향으로 4등분을 해주는 역할을 한다.<br/>
CSS파일을 완성하면 다음과 같다.<br/>
<p align="center"><img src="/assets/img/Modern-Javascript/GDSC-4주차/GDSC_Week4_3.png" width="300" height="500"></p>

지금은 버튼을 눌러도 아무런 변화가 나오지 않는다.

Javascript
======

먼저 다음과 같은 변수들과 배열을 선언한다. 
```javascript
const buttons = document.querySelectorAll("button");
const operators = document.querySelectorAll(".operator");
const displayEle = document.querySelector(".input");
const num = document.querySelectorAll(".num");

let Operator = " "; // 연산자
let previousNum = " "; // 이전 값
let resentNum = " "; // 최근 값
```

기본적인 계산 기능은 calculate 함수를 만들어 구현한다. 이때 switch 문을 이용하여 각 연산자 버튼을 눌렀을 때 계산해준다. 이 함수는 previousNum, Opertor, resentNum을 인자로 받는다.
```javascript
let calculate = (n1, operator, n2) => {
  let result = 0;
  switch (operator) {
    case "+":
      result = Number(n1) + Number(n2);
      break;
    case "-":
      result = Number(n1) - Number(n2);
      break;
    case "x":
      result = Number(n1) * Number(n2);
      break;
    case "/":
      result = Number(n1) / Number(n2);
      break;
    default:
      break;
  }
  return String(result);
};
```

calculator 함수를 다음과 같이 만들었다.
```javascript
let calculator = () => {
  let isFirstDigit = true;

  buttons.forEach((item) => {
    item.addEventListener("click", (e) => {
      let action = e.target.classList[0];
      let click = e.target.innerHTML;

      if (action === "operator") {
        Operator = click;
        previousNum = displayEle.textContent;
        displayEle.textContent = '';
        isFirstDigit = true; 
      }
      if (action === "num") {
        if (isFirstDigit && click === "0") {
          return;
        }

        if (displayEle.textContent === "" && Operator === "") {
          displayEle.textContent = click;
          previousNum = displayEle.textContent;
        }

        else if (displayEle.textContent !== "" && Operator === "") {
          displayEle.textContent = 
          displayEle.textContent + click;
          previousNum = displayEle.textContent;
        }

        else if (displayEle.textContent === "" && Operator !== "") {
          displayEle.textContent = click;
          resentNum = displayEle.textContent;
        }

        else if (displayEle.textContent !== "" && Operator !== "") {
          displayEle.textContent = 
          displayEle.textContent + click;
          resentNum = displayEle.textContent;
        }

        isFirstDigit = false; 
      }

      if (action === "result") {
        displayEle.textContent = calculate(
          previousNum,
          Operator,
          resentNum
        );
        isFirstDigit = true; 
      }
      if (action === "ac") {
        displayEle.textContent = "";
        previousNum = "";
        Operator = "";
        resentNum = "";
        isFirstDigit = true; 
      }
    });
  });
};
calculator();
```

위 함수에서 **let isFirstDigit = true;**는 예를 들어, 012 처럼 첫번째 자리에 0이 오지 않도록 하기위해 만들었다. 이제 각 **if 문**을 보자.
```javascript
if (action === "operator") {
        Operator = click;
        previousNum = displayEle.textContent;
        displayEle.textContent = '';
        isFirstDigit = true; 
      }
```
위 코드는 연산자를 눌렀을 때를 보여준다.

```javascript
if (action === "num") {
       if (isFirstDigit && click === "0") {
        return;
       }

       if (displayEle.textContent === "" && Operator === "") {
        displayEle.textContent = click;
        previousNum = displayEle.textContent;
      }

      else if (displayEle.textContent !== "" && Operator === "") {
        displayEle.textContent = 
        displayEle.textContent + click;
        previousNum = displayEle.textContent;
      }

      else if (displayEle.textContent === "" && Operator !== "") {
        displayEle.textContent = click;
        resentNum = displayEle.textContent;
      }

      else if (displayEle.textContent !== "" && Operator !== "") {
        displayEle.textContent = 
        displayEle.textContent + click;
        resentNum = displayEle.textContent;
      }

      isFirstDigit = false; 
    }
```
**isFirstDigit && click === "0"**은 첫 번째 숫자이고 입력된 값이 0인 경우 아무 작업도 수행하지 않도록 한다. **displayEle.textContent === "" && Operator === ""**는 창이 비어있고 연산자를 누르지 않았을때, 다음 **else if**는 창이 비어있지 않고 연산자를 누르지 않았을 때, 다음 **else if**는 창이 비어있고 연산자를 눌렀을 때, 그 다음은 창이 비어있지 않고 연산자를 누르지 않았을 때, **isFirstDigit = false**는 첫 번째 숫자 입력 후에는 첫 번째 숫자가 아님을 표시한다.

```javascript
if (action === "result") {
        displayEle.textContent = calculate(
          previousNum,
          Operator,
          resentNum
        );
        isFirstDigit = true; 
      }
```
위 코드는 "="를 눌렀을 때 calculate 함수를 실행한다.

```javascript
if (action === "ac") {
      displayEle.textContent = "";
      previousNum = "";
      Operator = "";
      resentNum = "";
      isFirstDigit = true; 
    }
```
위 코드는 "AC" 버튼을 눌렀을 때 초기화를 한다.

완성된 script.js는 다음과 같다.
```javascript
const buttons = document.querySelectorAll("button");
const operators = document.querySelectorAll(".operator");
const displayEle = document.querySelector(".input");
const num = document.querySelectorAll(".num");

let Operator = " ";
let previousNum = " ";
let resentNum = " ";

let calculate = (n1, operator, n2) => {
  let result = 0;
  switch (operator) {
    case "+":
      result = Number(n1) + Number(n2);
      break;
    case "-":
      result = Number(n1) - Number(n2);
      break;
    case "x":
      result = Number(n1) * Number(n2);
      break;
    case "/":
      result = Number(n1) / Number(n2);
      break;
    default:
      break;
  }
  return String(result);
};

let calculator = () => {
  let isFirstDigit = true;

  buttons.forEach((item) => {
    item.addEventListener("click", (e) => {
      let action = e.target.classList[0];
      let click = e.target.innerHTML;

      if (action === "operator") {
        Operator = click;
        previousNum = displayEle.textContent;
        displayEle.textContent = '';
        isFirstDigit = true; 
      }
      if (action === "num") {
        if (isFirstDigit && click === "0") {
          return;
        }

        if (displayEle.textContent === "" && Operator === "") {
          displayEle.textContent = click;
          previousNum = displayEle.textContent;
        }

        else if (displayEle.textContent !== "" && Operator === "") {
          displayEle.textContent = 
          displayEle.textContent + click;
          previousNum = displayEle.textContent;
        }

        else if (displayEle.textContent === "" && Operator !== "") {
          displayEle.textContent = click;
          resentNum = displayEle.textContent;
        }

        else if (displayEle.textContent !== "" && Operator !== "") {
          displayEle.textContent = 
          displayEle.textContent + click;
          resentNum = displayEle.textContent;
        }

        isFirstDigit = false; 
      }

      if (action === "result") {
        displayEle.textContent = calculate(
          previousNum,
          Operator,
          resentNum
        );
        isFirstDigit = true; 
      }
      if (action === "ac") {
        displayEle.textContent = "";
        previousNum = "";
        Operator = "";
        resentNum = "";
        isFirstDigit = true; 
      }
    });
  });
};
calculator();
```
이제 계산기 기능을 사용하면 다음과 같이 나온다.<br/>
<p align="center"><img src="/assets/img/Modern-Javascript/GDSC-4주차/GDSC_Week4_4.png" width="300" height="500"></p>