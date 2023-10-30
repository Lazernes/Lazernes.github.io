---
title: "GDSC 3주차 과제 to do list 구현"
# excerpt: ""

wirter: Myeongwoo Yoon
categories:
  - Modern Javascript
tags:
  - Programing

toc: true
toc_sticky: true
 
date: 2023-10-02
last_modified_at: 2023-10-02
---
HTML
======

head
------
```html
<head>
  <meta charset="UTF-8">
  <title>투두리스트</title>
  <link rel="stylesheet" href="style.css">
</head>
```
"**\<title> ... </title>**" 에서 ... 부분에 웹 문서의 제목을 입력한다.<br/>
HTML문서에서 css파일을 읽어 들이기 위해서 "**\<link rel="stylesheet" href="style.css">**"를 입력한다.

body
------
```html
<body>
  <div class="container">
    <h1>Things to do</h1>
    <form>
      <input type="text" id="input-text" placeholder="Enter your to-do" />
      <button id="input-button" onclick="addTodoList()">+</button>
    </form>
    <section class="todo-section">
      <h2>to do <span id="todo-count"></span></h2>
      <ul id="todo-list"></ul>
    </section>
    <section class="done-section">
      <h2>done <span id="done-count"></span></h2>
      <ul id="done-list"></ul>
    </section>
  </div>
</body>
```
**\<div>** 태그를 이용해서 class속성을 사용해 문서 구조를 만들거나 스타일을 적용하고 제목을 나타내기 위해 **\<h1>** 태그를 이용해 **Things to do**를 입력한다.<br/>
폼을 만들기 위해서 **\<form>**태그를 이용하고 \<input> 태그를 이용해 사용자가 입력할 부분을 만들어 주는데 여기서 택스트 필드를 만들어 주고(**type="text"**) todo목록을 입력할 것을 알려주기 위해 **placeholder** 속성을 사용해 **Enter your to-do**를 표기한다. 그리고 **+**버튼과 클릭 이벤트를 만들기 위해서 **\<button id="input-button" onclick="addTodoList()">+</button>**을 입력한다.<br/>
to-do 섹션과 done 섹션을 나태내기 위해서 **\<section>** 태그를 이용해 두 개의 \<section> 태그 영역으로 나누었다. 각 섹션에서 **\<h2>** 태그를 이용해 각각 **to do**, **done**을 입력해 준다. css파일에서 **todo-count** 각 섹션에서 todo 목록과 done 목록을 만들어 주기위해서 **\<ul>** 태그를 이용한다.<br/>
```html
<script src="script.js"></script>
```
마지막으로 HTML문서에서 javascript파일을 읽어 들이기 위해서 "**\<script src="script.js"></script>**"를 입력한다.

index.html
------
```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>투두리스트</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h1>Things to do</h1>
    <form id="input-form">
      <input type="text" id="input-text" placeholder="Enter your to-do" />
      <button id="input-button" onclick="addTodoList()">+</button>
    </form>
    <section class="todo-section">
      <h2>to do <span id="todo-count"></span></h2>
      <ul id="todo-list"></ul>
    </section>
    <section class="done-section">
      <h2>done <span id="done-count"></span></h2>
      <ul id="done-list"></ul>
    </section>
  </div>
</body>
<script src="script.js"></script>
</html>
```
<br/>

CSS
======

```css
body {
  background: #ccc;
	padding: 20px;
}

h1, h2 {
  font-style: italic;
}
```
배경과 HTML파일에서 **\<h1>Things to do</h1>**와 **\<h2>to do <span id="todo-count"></span></h2>**, **\<h2>done <span id="done-count"></span></h2>**의 폰트를 설정해 준다.<br/>

```css
.container {
  width: 350px;
  height: 650px;
  background:#fff;
	border:1px solid #222;
	border-radius: 25px;
	padding: 20px;
	margin:30px auto;
  display: flex;
  flex-direction: column;
}
```
투두리스트의 전체적인 틀을 만들어 주기위해 container 속성을 만들어준다.<br/> 웹 화면에서 다음과 같이 나오는데<br/>
<p align="center"><img src="/assets/img/Modern-Javascript/GDSC-3주차/GDSC_Week3_1.png" width="400" height="300"></p><br/>

**to do**와 **done**사이에 공간을 만들고 구분선을 만들어 주기 위해서 다음 코드를 입력한다.
```css
.todo-section,
.done-section {
  flex: 0.5;
  border-top: 1px solid #000;
}

#todo-list,
#done-list {
  cursor: pointer;
  list-style: none;
  padding-left: 1rem;
  margin: 0;
  overflow: auto;
}
```
위 코드를 적용하면 다음과 같이 웹에 표시된다.
<p align="center"><img src="/assets/img/Modern-Javascript/GDSC-3주차/GDSC_Week3_2.png" width="400" height="300"></p><br/>
위 화면에서 사용자가 입력하는 부분이 너무 작아서 과제 예제를 참조해 다음과 같은 코드를 입력해 주었다.

```css
#input-text {
  width: 80%;
  height: 30px;
  font-size: 18px;
  border: 0;
  border-radius: 15px;
  outline: none;
  background-color: rgba(213, 213, 213, 0.5);
}
```
그리고 예제 코드를 보고 입력 부분과 to do부분 사이에 여백을 주기 위해 HTML파일과 CSS파일에 다음 코드를 추가해 주었다.
```html
<form id="input-form">
      <input type="text" id="input-text" placeholder="Enter your to-do" />
      <button id="input-button" onclick="addTodoList()">+</button>
    </form>
```
```css
#input-form {
  display: flex;
  align-items: center;
  justify-content: center;
  padding-top: 1rem;
  padding-bottom: 1rem;
}
```
위 코드들을 적용해 주면 다음과 같이 웹에 표시된다.
<p align="center"><img src="/assets/img/Modern-Javascript/GDSC-3주차/GDSC_Week3_3.png" width="400" height="300"></p><br/>

style.css
------
```css
body {
  background: #ccc;
	padding: 20px;
}

h1, h2 {
  font-style: italic;
}

.container {
  width: 350px;
  height: 650px;
  background:#fff;
	border:1px solid #222;
	border-radius: 25px;
	padding: 20px;
	margin:30px auto;
  display: flex;
  flex-direction: column;
}

#input-form {
  display: flex;
  align-items: center;
  justify-content: center;
  padding-top: 1rem;
  padding-bottom: 1rem;
}

#input-text {
  width: 80%;
  height: 30px;
  font-size: 18px;
  border: 0;
  border-radius: 15px;
  outline: none;
  background-color: rgba(213, 213, 213, 0.5);
}

.todo-section,
.done-section {
  flex: 0.5;
  border-top: 1px solid #000;
}

#todo-list,
#done-list {
  cursor: pointer;
  list-style: none;
  padding-left: 1rem;
  margin: 0;
  overflow: auto;
}
```
<br/>

Javascript
======
HTML과 CSS는 방학에 공부했던 책을 참조하여 어떻게든 해보았는데 Javascript는 아직 공부한지 2주정도밖에 되지 않아서 예제 코드를 보고 분석을 해보았다.<br/>

let itemList=[ ];
------
저장 공간을 만들어 주기 위해서 **itemList**라는 리스트를 만들어 준다.
```javascript
let itemList = [];
```

renderTodoItem
------
다음 함수를 만들어 주어 로컬 스토리지에서 값을 불러와 화면에 표시해준다.
```javascript
const renderTodoItem = () => {
  const savedTodo = localStorage.getItem("itemList");
  const todoList = document.getElementById("todo-list");
  const doneList = document.getElementById("done-list");

  // 덮어쓰지 않도록 초기화
  todoList.innerHTML = "";
  doneList.innerHTML = "";

  // 데이터가 존재하는지 확인
  if (savedTodo) {
    itemList = JSON.parse(savedTodo);

    itemList.forEach((savedItem) => {
      const item = document.createElement("li");

      const itemText = document.createElement("span");
      itemText.className = "item-text";
      itemText.addEventListener("click", toggleItem);
      itemText.innerText = savedItem.text;

      const deleteButton = document.createElement("button");
      deleteButton.className = "delete-button";
      deleteButton.addEventListener("click", removeItem);
      deleteButton.innerText = "🧹";

      item.appendChild(itemText);
      item.appendChild(deleteButton);

      if (!savedItem.isDone) {
        todoList.appendChild(item);
      } else {
        doneList.appendChild(item);
      }
    });
  }
  countItem();
};
```
위 코드에서 다음 부분은 HTML에 설정한 변수와 리스트를 저장해 두는 코드이다.

```javascript
const savedTodo = localStorage.getItem("itemList");
const todoList = document.getElementById("todo-list");
const doneList = document.getElementById("done-list");
```
다음 코드는 데이터가 존재하는지 확인하기 위해 입력된 코드이다.
```javascript
 if (savedTodo) {
    itemList = JSON.parse(savedTodo);

    itemList.forEach((savedItem) => {
      const item = document.createElement("li");
      const itemText = document.createElement("span");
      itemText.className = "item-text";
      itemText.addEventListener("click", toggleItem);
      itemText.innerText = savedItem.text;

      const deleteButton = document.createElement("button");
      deleteButton.className = "delete-button";
      deleteButton.addEventListener("click", removeItem);
      deleteButton.innerText = "🧹";

      item.appendChild(itemText);
      item.appendChild(deleteButton);

      if (!savedItem.isDone) {
        todoList.appendChild(item);
      } else {
        doneList.appendChild(item);
      }
    });
  }
```
**JSON.parse** 메서드는 인수로 받은 문자열을 자바스크립트 객체로 환원해서 반환하는 메서드이고 이를 itemList에 저장한다. <br/><br/>
**createElement** 메서드는 매개변수를 만들어 주는데 HTML에서 작성한 **li**와 **span**을 매개변수로 받는다. **itemText.className = "item-text";**는 HTML에서 불러온 요소의 class 이름을 css에서 지정한 선택자 이름으로 변경한다. **addEventListener** 메서드로 이벤트 리스너를 등록해주고 이때 **click**은 이벤트 유형을 뜻하고 **toggleItem**은 이벤트가 발생했을 때 처리를 담당하는 콜백 함수의 참조이다. **innerText**는 요소안의 text값들을 가져온다.<br/>
**deleteButton**또한 앞에서 설명한 것과 같이 click 이벤트가 발생했을 때 **removeItem**을 한다.<br/>
**appendChild(itemText)**,**appendChild(deleteButton)** 메서드는 부모 노드(**item**)에 추가할 노드(**itemText**,**deleteButton**)를 의미한다.

addTodoList, toggleItem, removeItem, countItem
------
다음 코드들은 투두리스트의 기능들을 만들어 주는 코드이다. 과제에서 요구한 투두리스트의 기능은 다음과 같다.<br/>
　• input에 할 일을 입력한 뒤 추가 버튼을 누르면 todo 목록에 추가된다.<br/>
　• todo 목록의 아이템을 클릭하면 done으로, done 목록의 아이템을 클릭하면 todo로 이동할 수 있게 한다.<br/>
　• 아이템 삭제를 누르면 화면에서 삭제될 수 있도록 한다.<br/>
```javascript
const addTodoList = () => {
  event.preventDefault;
  const inputObject = {
    id: Date.now(),
    text: document.getElementById("input-text").value,
    isDone: false,
  };
  if (inputObject.text) {
    itemList = [...itemList, inputObject];
    localStorage.setItem("itemList", JSON.stringify(itemList));
    renderTodoItem();
  }
};

const toggleItem = (e) => {
  const itemObject = itemList.find(
    (inputObject) => inputObject.text === e.target.innerText
  );
  itemObject.isDone = !itemObject.isDone;
  localStorage.setItem("itemList", JSON.stringify(itemList)); // 로컬 스토리지 갱신
  renderTodoItem();
};

const removeItem = (e) => {
  const filteredList = itemList.filter(
    (inputObject) =>
      inputObject.text !== e.target.parentNode.innerText.slice(0, -2)
  );
  localStorage.setItem("itemList", JSON.stringify(filteredList)); // 로컬 스토리지 갱신
  renderTodoItem();
};

const countItem = () => {
  const todoCount = document.getElementById("todo-count");
  todoCount.innerText = ` (${itemList.filter((item) => !item.isDone).length})`;

  const doneCount = document.getElementById("done-count");
  doneCount.innerText = ` (${itemList.filter((item) => item.isDone).length})`;
};
```
**addTodoList**는 새로운 할 일을 입력 받을 때 로컬 스토리지에 추가하는 함수이다. 이때 **inputObject**객체를 만들고 이 inputObject의 **text**를 로컬 스토리지에 저장해줘 **Enter your todo**에 입력을 해주면 **itemList**에 저장된다.<br/>
**toggleItem**은 isDone의 상태를 반대로 바꾸어 주는 함수이다. click이벤트가 발생하면 to do에 있던 목록이 done으로 가거나 done에 있던 목록이 to do로 가게 해준다.<br/>
**removeItem**은 로컬 스토리지에서 값을 삭제한다. 🧹을 클릭하면 to do나 done에 있는 목록이 삭제된다.<br/>
**countItem**은 todo, done의 수를 카운트한다.<br/>

script.js
------
```javascript
let itemList = [];

const renderTodoItem = () => {
  const savedTodo = localStorage.getItem("itemList");
  const todoList = document.getElementById("todo-list");
  const doneList = document.getElementById("done-list");

  todoList.innerHTML = "";
  doneList.innerHTML = "";

  if (savedTodo) {
    itemList = JSON.parse(savedTodo);

    itemList.forEach((savedItem) => {
      const item = document.createElement("li");
      const itemText = document.createElement("span");
      itemText.className = "item-text";
      itemText.addEventListener("click", toggleItem);
      itemText.innerText = savedItem.text;

      const deleteButton = document.createElement("button");
      deleteButton.className = "delete-button";
      deleteButton.addEventListener("click", removeItem);
      deleteButton.innerText = "🧹";

      item.appendChild(itemText);
      item.appendChild(deleteButton);

      if (!savedItem.isDone) {
        todoList.appendChild(item);
      } else {
        doneList.appendChild(item);
      }
    });
  }
  countItem();
};

const addTodoList = () => {
  event.preventDefault;
  const inputObject = {
    id: Date.now(),
    text: document.getElementById("input-text").value,
    isDone: false,
  };
  if (inputObject.text) {
    itemList = [...itemList, inputObject];
    localStorage.setItem("itemList", JSON.stringify(itemList));
    renderTodoItem();
  }
};

const toggleItem = (e) => {
  const itemObject = itemList.find(
    (inputObject) => inputObject.text === e.target.innerText
  );
  itemObject.isDone = !itemObject.isDone;
  localStorage.setItem("itemList", JSON.stringify(itemList));
  renderTodoItem();
};

const removeItem = (e) => {
  const filteredList = itemList.filter(
    (inputObject) =>
      inputObject.text !== e.target.parentNode.innerText.slice(0, -2)
  );
  localStorage.setItem("itemList", JSON.stringify(filteredList));
  renderTodoItem();
};

const countItem = () => {
  const todoCount = document.getElementById("todo-count");
  todoCount.innerText = ` (${itemList.filter((item) => !item.isDone).length})`;

  const doneCount = document.getElementById("done-count");
  doneCount.innerText = ` (${itemList.filter((item) => item.isDone).length})`;
};

window.onload = renderTodoItem();

```