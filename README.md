# [과제] 숙련주차 과제 답

## 문제 내용

### 1. 추가하기 버튼을 클릭해도 추가한 아이템이 화면에 표시되지 않음.

dispatch 연결 후, onSubmitHandler에 dispatch 함수를 통해 addTodo 리듀서로 객체를
전달해서 해결

- Form.jsx

```jsx
const dispatch = useDispatch();

const onSubmitHandler = (event) => {
  event.preventDefault();
  if (todo.title.trim() === '' || todo.body.trim() === '') return;

  dispatch(
    addTodo({
      id: id,
      title: todo.title,
      body: todo.body,
      isDone: false,
    })
  );
};
```

![](https://i.imgur.com/UEteC3p.png)

### 2. 추가하기 버튼 클릭 후 기존에 존재하던 아이템들이 사라짐.

...연산자를 사용하여 todos아이템을 가져온다 > action.payload로 받아온 데이터를
추가

todos: [...기존 데이터, 새로 추가될 데이터]

- todos.js (기존 리듀서)

```jsx
  switch (action.type) {
    case ADD_TODO:
      return {
        ...state,
        todos: [action.payload],
      };
```

- todos.js (해결)

```jsx
  switch (action.type) {
    case ADD_TODO:
      return {
        ...state,
        todos: [...state.todos, action.payload],
      };

```

### 3. 삭제 기능이 동작하지 않음.

todos 아이템을 ...연산자로 가져오고 > filter를 통해 state의 아이디와 payload가
일치하지 않는것들만 보여줌

- todos.js

```jsx
case DELETE_TODO:
  return {
	...state,
	todos: state.todos.filter((v) => v.id !== action.payload),
  };
```

![](https://i.imgur.com/BNszP6L.png)

### 4. 상세 페이지에 진입 하였을 때 데이터가 업데이트 되지 않음.

useEffect 훅을 사용하고 id가 변경 되면, dispatch로 getTodoByID 스토어에 있는
todo에 값을 저장.

- Detail.jsx

```jsx
useEffect(() => {
  dispatch(getTodoByID(id));
}, [dispatch, id]);
```

### 5. 완료된 카드의 상세 페이지에 진입 하였을 때 올바른 데이터를 불러오지 못함.

to의 값을 index ➜ todo.id로 변경

```jsx
<StLink to={`/${index}`} key={todo.id}>
```

```jsx
<StLink to={`/${todo.id}`} key={todo.id}>
```

![](https://i.imgur.com/mX61tlY.png)

### 6. 취소 버튼 클릭시 기능이 작동하지 않음.

onTogglesStatusTodo 함수에 아래와 같이 파라미터로 id를 넘겨줬으니,

```jsx
const onToggleStatusTodo = (id) => {
  dispatch(toggleStatusTodo(id));
};
```

아래 코드가 아닌

```jsx
<StButton borderColor="green" onClick={onToggleStatusTodo}>
```

이것처럼 map value의 id를 전달해줘야 한다. (화살표 함수는 렌더 되자마자 실행되는
것을 막아줌)

```jsx
<StButton borderColor="green" onClick={() => onToggleStatusTodo(todo.id)}
>
```
