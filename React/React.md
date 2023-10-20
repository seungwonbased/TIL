\# JSX

- A Syntax eXtension to JavaScript
- 자바스크립트 확장 문법

```
<img src="/images/data/test.png" width="100" height="100" style="border: 1px solid red;" onclick="..." />
```
- Tag
	- 시작 태그와 종료 태그로 구성
	- 종료 태그가 생략되거나 자기 종료되는 경우가 있음
		- e.g., \<input type="text"> \<img src="..." />
- Content
	- 시작 태그와 종료 태그 사이의 내용
- Attribute
	- 태그를 처리하는 데 필요한 부가적인 정보
	- e.g., 태그에 특화된 속성, 스타일과 관련된 속성, 이벤트 핸들러, ...
- Element = Tag + Content = Attribute

## JSX의 역할

- 내부적으로 XML / HTML 코드를 JavaScript로 변환
	- createElement() 함수가 담당
- 코드가 간결해지고 생산성과 가독성이 향상됨
- 보안 향상
	- 태그 안의 Content를 무조건 문자열로 처리하기 때문에 XSS 공격 방어 가능

### createElement()

```javascript
React.createLement(
	type,    // 엘리먼트의 유형, HTML 태그 또는 다른 리액트 엘리먼트
	[props],    // 부모 엘리먼트에서 자식 엘리먼트에게 전달하는 값
	[...children]    // 현재 엘리컨트가 포함하고 있는 자식 엘리먼트
)
```

## JSX 사용법
### 1. 반드시 부모 요소 하나로 감싸야 함

#### \<div>와 같은 태그로 감싸는 방법
- Root 엘리먼트를 추가하는 것임
- 태그가 너무 많아질 수 있음

#### \<Fragment>\</Fragment>와 같은 Fragment 컴포넌트를 이용해 감싸는 방법
- 불필요하게 태그가 추가되는 것을 방지

#### <>\</>로 Fragment를 축약해서 감싸는 방법

### 2. JSX 안에서는 JavaScript 표현식을 {}로 감싸서 작성

```javascript
function App() {
  const name = '배승원';
  const age = 25;
  return (
    <>
      <h1>안녕하세요, {name}.</h1>      
      <h2>내년에 나이는 {age + 1}입니다.</h2>
    </>
  );
}

export default App;
```

### 3. JSX 내부에서는 JavaScript의 if statement를 사용할 수 없으므로, 삼항 연산자를 사용해야 함

- 조건식 ? 참인 경우 : 거짓인 경우
	- = if (조건식) { 참인 경우 } else { 거짓인 경우 }

### 4. 조건을 만족할 때만 내용을 보여줄 경우에는 조건부 렌더링

#### 삼항 연산자 사용

```Javascript
function App() {
  const name = '배승원';
  return (
    <>
      <h1>{name}님, </h1>      
      <h2>{ name === '배승원' ? '환영합니다.' : null }</h2>
    </>
  );
}

export default App;
```

- 조건을 만족하지 않는 경우 null 반환
	- 화면에 내용이 출력되지 않음

#### && 연산자 사용

```Javascript
function App() {
  const name = '배승원';
  return (
    <>
      <h1>{name}님, </h1>      
      { name === '배승원' && <h2>환영합니다.</h2> }
    </>
  );
}

export default App;
```

### 5. 함수에서 undefined를 반환하지 않도록 해야함

```javascript
function App() {
  const name = undefined;
  return name || 'undefined 입니다.';
}

export default App;
```

-  || 연산자를 이용해서 undefined이 경우 사용할 기본값을 지정할 수 있음

### 6. Inline Styling

```javascript
function App() {
  const name = '리액트';
  const nameStyle = {
    backgroundColor: 'black', 
    color: 'yellow', 
    fontSize: 48,    // 단위를 생략하면 'px'이 기본
    padding: '16px'
  };
  return (
    <div style={nameStyle}>
      {name}
    </div>
  );
}

export default App;


function App() {
  const name = '리액트';
  return (
    <div style={{ backgroundColor: 'black', color: 'yellow', fontSize: 48, padding: '16px' }}>
      {name}
    </div>
  );
}

export default App;
```

- DOM 요소에 스타일을 적용할 때는 객체 형태로 적용해야 하며, 스타일 이름은 카멜 표현식을 사용
- div style의 바깥쪽 {}는 표현식을 뜻하고, 안쪽 {}는 객체를 뜻함

### 7. CSS 클래스를 사용할 때 class가 아닌 className으로 설정

```javascript
import './App.css';

function App() {
  const name = '리액트';
  return (
    <div className="react">
      {name}
    </div>
  );
}

export default App;
```

### 8. 모든 태그는 반드시 닫는 태그를 사용해야 함

```javascript
import './App.css';

function App() {
  const name = '리액트';
  return (
    <>
      <div className="react">{name}</div>

      <input type="text"></input>
      <input type="text" />
      <input type="text">    // 이렇게 닫는 태그를 사용하지 않으면 오류가 발생
    </>
  );
}

export default App;
```

- 내용이 없는 태그는 Self-closing 태그 사용

### 9. Comment

```javascript
import './App.css';

function App() {
  // 인라인 주석
  const name = '리액트';    // 인라인 주석 
  /*
      블럭 단위 주석
  */

  return (
    // 인라인 주석

    /*
       블럭 단위 주석
    */
    <>
      // 텍스트로 인식됨
      
      /* 
        텍스트로 인식됨
      */

      { /*
          이렇게 해야 주석 처리가 가능 
        */ }
        
      { // 이렇게 하는 것도 가능
      }
      
      {
        // 물론 이렇게 하는 것도 가능
      }
      
      { // 이렇게 하면 닫는 중괄호가 해석도지 않을 수도 있음}
      }
      <div className="react">{name}</div>
    </>
  );
}

export default App;
```

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
