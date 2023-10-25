# Ref

> https://ko.legacy.reactjs.org/docs/refs-and-the-dom.html

- DOM 요소나 React 컴포넌트에 대한 참조를 생성하고 관리하는데 사용되는 특별한 속성
- render 메서드에서 생성된 DOM 노드나 React 엘리먼트에 접근하는 방법을 제공
- Use cases
	- 포커스, 텍스트 선택영역, 혹은 미디어의 재생을 관리할 때
	- 애니메이션을 직접적으로 실행시킬 때
	- 서드 파티 DOM 라이브러리를 React와 같이 사용할 때

## 사용 방법

### 1. React.createRef() API를 이용하는 방법 (React 16.3에서 추가, 권장)

```jsx
class MyComponent extends React.Component {

	constructor() {
		super();
		this.myref = React.createRef();
	}

	render() {
		return <input type="text" ref={this.myref} />;
	}
	
	const handler = () => {
		this.myref.current.focus();
	}
}
```

### 2. 콜백 ref를 이용하는 방법

```jsx
<input ref={ x => this.myinput = x } />
```

- 변수를 이용해서 \<input> 요소를 직접 제어하는 것이 가능
- 예시
	- this.myinput.focus();

