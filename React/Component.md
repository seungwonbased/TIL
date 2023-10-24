# Component
## Class형 Component
## Function형 Component
## props 활용

- Properties 줄임말
- 컴포넌트의 속성을 설정할 때 사용
- props 변수의 값은 해당 컴포넌트를 사용하는 부모 컴포넌트에서 설정
- 컴포넌트 자신은 해당 props 값을 읽기 전용으로 사용만 가능

```
<Parent>
<Child 변수_이름=변수_값 변수_이름=변수_값></Child>
</Parent>
```

## State

- 상태 변수
- 컴포넌트 내부에서 읽고 업데이트할 수 있는 값
- 상태 변수의 값이 변경되면 리렌더링이 발생
- 클래스형 컴포넌트는 setState() 메서드를 이용해서, 함수형 컴포넌트는 useState() 훅 함수가 반환하는 Setter 함수를 이용해 값을 변경해야 함

### 상태 변수를 사용해야 하는 이유

- 변수의 값 변화가 화면에 반영되어야 하는 경우 상태변수를 사용
- 이때 상태변수의 값 변경은 setState() 메서드를 이용해야 함

### setState(): 클래스형 컴포넌트

- 방법1: updater 함수를 이용
	- setState(updater[, callback])
	- 첫번째 인자 updater는 (state, props) => stateChange 형태를 가지는 함수
	- state는 변경 사항이 적용되는 시점에 컴포넌트가 가지는 state에 대한 참조
	- updater 함수로 전달된 state와 props는 최신값임이 보장
- 방법2: 변경객체를 전달하는 방법
	- setState(stateChange[, callback])
	- setState()에 전달하는 첫번째 인자로 아래와 같이 객체를 전달하는 것도 가능
		- this.setState({ number: number + 1 })
	- 비동기적으로 수행되며, 같은 주기 동안 여러번 호출된다면 일괄적으로 처리될 수 있음

### useState(): 함수형 컴포넌트

- const [ 상태 변수, Setter 함수 ] = useState(상태_변수_초기값);
- 상태 변수의 초기값과 상태 변수의 값을 변경할 수 있는 함수(Setter 함수)를 배열로 반환
	- [ 상태변수의 초기값 , 상태변수의 값을 변경할 수 있는 함수 ]

## Component Lifecycle 컴포넌트 생명주기
### 함수형 컴포넌트 생명주기

|**분류**|**클래스형 컴포넌트**|**함수형 컴포넌트**|
|---|---|---|
|Mounting|constructor()|**함수형 컴포넌트 내부**|
|Mounting|render()|**return()**|
|Mounting|ComponenDidMount()|**useEffect()**|
|Updating|componentDidUpdate()|**useEffect()**|
|UnMounting|componentWillUnmount()|**useEffect()**|

#### **순서**
1. 컴포넌트가 호출되어 로드됨
2. 컴포넌트 내부 함수 수행
	- 데이터의 초기화를 수행
3. return() 함수 실행
	- 화면의 렌더링을 수행
4. useEffect() 실행

**💡 조건부 수행  
- useEffect() 실행
	- 컴포넌트의 '변화'가 발생하는 경우 수행
- **useEffect() 실행
	- 컴포넌트의 '소멸'이 발생하는 경우 수행

#### 렌더링
- React 내에서는 컴포넌트 렌더링이라고도 하며, 컴포넌트 내에 엘리먼트 요소들(HTML, React 사용자 정의 태그)을 화면상에 그리는 동작을 의미

#### 리-렌더링
- 컴포넌트 내에 엘리먼트 요소들을 코드를 기반으로 화면의 그리는 작업을 다시 수행하는 것을 의미