# 생명주기 함수

- 생명주기 함수를 사용하기 위해서는 클래스형 컴포넌트로 구현해야 함
	- 함수형 컴포넌트는 Hook 함수를 통해 비슷하게 구현 가능
- 1. **`constructor(props)`**
    - 컴포넌트가 생성될 때 호출되는 함수
    - 컴포넌트의 초기 설정을 담당하며, 이 메서드 내에서 `super(props)`를 호출하여 부모 클래스의 생성자를 실행해야 함
2. **`static getDerivedStateFromProps(props, state)`**
    - v16.3 이후 추가된 생명주기 함수로, 컴포넌트가 마운트될 때, 업데이트될 때 호출됨
    - 이 함수에서 반환된 객체는 현재의 state를 업데이트
3. **`render()`**
    - 컴포넌트의 UI를 렌더링하는 함수
    - 이 메서드에서는 순수한 함수로서 동작하며, 컴포넌트의 상태나 속성을 변경하지 않아야 함
4. **`componentDidMount()`**
    - 컴포넌트가 화면에 마운트된 후에 호출됨
    - 주로 네트워크 요청이나 초기 데이터 가져오기 등의 작업에 사용됨
5. **`shouldComponentUpdate(nextProps, nextState)`**
    - 컴포넌트가 업데이트되기 전에 호출되며, 업데이트가 필요한지 여부를 결정
    - 성능 최적화를 위해 사용됨
6. **`getSnapshotBeforeUpdate(prevProps, prevState)`**
    - 실제로 DOM에 변화를 반영하기 직전에 호출됨
    - 주로 스크롤 위치나 컴포넌트의 크기와 같은 정보를 얻을 때 사용됨
7. **`componentDidUpdate(prevProps, prevState, snapshot)`**
    - 컴포넌트의 업데이트가 DOM에 반영된 후에 호출됨
    - 업데이트 이후에 필요한 작업을 수행할 때 사용
8. **`componentWillUnmount()`**
    - 컴포넌트가 언마운트되기 전에 호출됨
    - 이 메서드에서는 리소스의 정리나 타이머의 제거 등의 작업이 수행됨

- 이러한 생명주기 함수를 적절하게 활용하면 컴포넌트의 수명 주기 동안 다양한 작업을 수행 가능
- 그러나 최신 버전의 리액트에서는 함수형 컴포넌트와 훅(React Hooks)을 사용하는 추세이며, 클래스형 컴포넌트보다 간결하고 효율적인 코드를 작성할 수 있음

# Hook 함수

- React Hooks는 함수형 컴포넌트에서 상태 및 생명주기와 같은 React의 기능을 사용할 수 있게 해주는 함수
- 훅을 사용하면 함수형 컴포넌트에서도 클래스형 컴포넌트에서 가능한 작업들을 수행할 수 있음

1. **`useState`**
    - 컴포넌트에서 상태를 추가할 수 있게 해주는 훅
    - 배열을 반환하며, 첫 번째 요소는 현재 상태 값이고, 두 번째 요소는 상태를 업데이트하는 함수
    `const [count, setCount] = useState(0);`
2. **`useEffect`**
    - 부수 효과(네트워크 요청, 데이터 구독 등)를 수행할 때 사용되는 훅
    - 컴포넌트가 마운트될 때, 업데이트될 때, 언마운트될 때 등 특정 시점에 함수를 실행할 수 있음    
    `useEffect(() => {   // 부수 효과 수행   return () => {     // 정리 작업 (언마운트 시에 호출됨)   }; }, [dependencies]);`
3. **`useContext`**
    - Context를 사용할 때 사용되는 훅으로, Context에서 값을 읽을 수 있게 해줌
    `const value = useContext(MyContext);`
4. **`useReducer`**
    - `useState`와 비슷하게 상태를 다루지만, 복잡한 상태 로직을 함수로 분리하여 처리할 때 유용
    `const [state, dispatch] = useReducer(reducer, initialState);`
5. **`useCallback`**
    - 메모이제이션된 콜백 함수를 생성
    - 성능 최적화에 활용
    `const memoizedCallback = useCallback(() => {   // 콜백 함수 내용 }, [dependencies]);`
6. **`useMemo`**
    - 메모이제이션된 값을 반환
    - 성능 최적화에 활용
    `const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);`
7. **`useRef`**
    - `ref` 객체를 생성하고, 해당 객체의 `current` 프로퍼티를 이용하여 DOM 요소나 다른 값에 접근할 때 사용
    `const myRef = useRef(initialValue);`
8. **`useImperativeHandle`**
    - 부모 컴포넌트에 노출되는 인터페이스를 지정할 때 사용
    `useImperativeHandle(ref, () => ({   // 노출할 메서드 및 프로퍼티 }), [dependencies]);`

- 이러한 훅들은 함수형 컴포넌트에서도 상태와 생명주기를 관리할 수 있게 해주어 코드를 간결하고 이해하기 쉽게 만듬
- 훅을 사용하면 클래스형 컴포넌트에서 발생하는 많은 문제들을 해결하면서 함수형 프로그래밍의 장점을 살릴 수 있음

# 상태변수
### 클래스형 컴포넌트에서의 상태변수 활용:

1. **상태변수 정의:**
    - `constructor` 메서드 내에서 `this.state`를 사용하여 초기 상태를 정의

```jsx
import React, { Component } from 'react';

class MyClassComponent extends Component {   
	constructor(props) {     
		super(props);     
		this.state = {      
			count: 0,    
		};  
	}  
	// ... 
}
```

2. **상태변수 활용:**
    - `this.setState`를 사용하여 상태를 업데이트하고, 렌더링이 필요한 경우 `render` 메서드에서 새로운 상태를 반영
```jsx
class MyClassComponent extends Component {  
	// ...    
	handleIncrement = () => {     
		this.setState((prevState) => ({ count: prevState.count + 1 }));   
	};    
	render() {     
		return (       
			<div>        
				 <p>Count: {this.state.count}</p>         
				 <button onClick= {this.handleIncrement}>Increment
				 </button>
			</div>     
		);   
	}
}
```    

### 함수형 컴포넌트에서의 상태변수 활용:

1. **상태변수 정의:**
    - `useState` 훅을 사용하여 상태를 정의
```jsx
import React, { useState } from 'react';  

function MyFunctionalComponent() {   
	const [count, setCount] = useState(0);   
	// ... 
}
```

2. **상태변수 활용:**
    - `setCount` 함수를 사용하여 상태를 업데이트하고, 컴포넌트가 리렌더링됩니다.
```jsx
function MyFunctionalComponent() {
	// ...    
	const handleIncrement = () => {     
		setCount(count + 1);   
	};    
	return (     
		<div>       
			<p>Count: {count}</p>       
				<button onClick={handleIncrement}>Increment</button>
		</div>
	);
}
```    

- 함수형 컴포넌트에서는 클래스형 컴포넌트와 달리 여러 상태를 한 번에 정의할 수 있으며, 상태값의 초기화는 함수의 호출 시에만 발생
- 또한, 함수형 컴포넌트에서는 `useState`를 통해 상태값을 직접 수정하는 것이 아니라 업데이트 함수를 사용하므로, 비동기 업데이트에 대한 이슈를 피할 수 있음

# useEffect 함수

- `useEffect` 훅은 함수형 컴포넌트에서 부수 효과를 수행하기 위해 사용되는 훅
- `useEffect`는 두 개의 매개변수를 받음
```jsx
useEffect(() => {   
	// 부수 효과 수행   
	// 반환된 함수는 cleanup 함수로, 컴포넌트가 언마운트되거나 업데이트되기 전에 호출됨
	return () => {     
		// 정리 작업   
	}; 
}, [dependencies]);
```

1. **Effect 함수:**    
    - 첫 번째 매개변수는 부수 효과를 수행하는 함수
    - 컴포넌트가 마운트될 때, 업데이트될 때, 언마운트될 때 등 특정 시점에 실행됨
    - 비동기 코드, 데이터 가져오기, 구독 설정 등을 처리

```jsx
useEffect(() => {
	console.log('컴포넌트가 마운트됨');
	// 부수 효과 수행   
	return () => {     
		console.log('컴포넌트가 언마운트되거나 업데이트되기 전에 호출됨');
    // 정리 작업   
    }; 
}, []); // 빈 배열은 의존성이 없으므로 컴포넌트가 마운트될 때만 실행`
```

2. **의존성 배열 (Dependencies):**
    - 두 번째 매개변수는 의존성 배열로, 특정 값이 변경될 때에만 `useEffect`가 실행되도록 조절
    - 의존성 배열이 빈 배열이면 컴포넌트가 마운트될 때 한 번만 실행됨
    - 의존성 배열이 없으면 매 렌더링마다 실행됨
    
```jsx
useEffect(() => {   
	console.log('의존성이 있는 값이 변경됨');   
	// 부수 효과 수행   
	return () => {     
		console.log('컴포넌트가 언마운트되거나 업데이트되기 전에 호출됨');     
	// 정리 작업   
}; }, [dependency1, dependency2]);
```

- `useEffect`를 사용할 때 의존성 배열을 지정함으로써 원하는 타이밍에 효과를 발생시킬 수 있고, 해당 효과에 대한 정리 작업도 수행할 수 있음
- 또한, 클래스형 컴포넌트의 `componentDidMount`, `componentDidUpdate`, `componentWillUnmount`와 유사한 동작을 구현할 수 있음
