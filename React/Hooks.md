# Hooks

- 함수형 컴포넌트에서 클래스형 컴포넌트가 제공하는 기능을 지원하기 위해 도입된 함수
	- State, Ref, Lifecycle function 등

## useState()

- 함수형 컴포넌트에서 State를 관리하기 위해 사용
- [상태변수 초기값, 상태 변수를 변경하는 함수] 배열을 반환

```JSX
const [ name, setName ] = useState('');
```

## useEffect()

- 클래스형 컴포넌트의 componentDidMount, componentDidUpdate, componentWillUnmount를 합친 형태
- 사용법
	- useEffect(이펙트 함수, 의존성 배열);
	- 의존성 배열
		- 이펙트가 의존하고 있는 배열
		- 배열 안에 있는 변수 중 하나라도 값이 변경되었을 때 이펙트 함수가 실행됨

## useRef

- 함수형 컴포넌트에서 DOM 요소를 직접 제어하기 위해 사용
