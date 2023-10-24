# Event Handling
## React에서 이벤트를 사용할 때 주의사항
### 1. 이벤트 이름은 카멜 케이스를 사용

- onclick (x)
- OnClick (x)
- onClick (o)

- onkeyup (x)
- OnKeyUp (x)
- onKeyUp (o)

### 2. 이벤트 핸들러로 자바스크립트 코드를 사용할 수 없고, 함수 형태로 전달

- HTML => \<button onClick="console.log('클릭되었습니다.')" />
- react => \<button onClick={ () => console.log('클릭되었습니다.') } />

### 3. 이벤트는 DOM 요소(Element)에만 사용이 가능

- HTML 태그
- 직접 만든 컴포넌트 태그에는 이벤트 설정 불가능

## React에서 처리할 수 있는 이벤트 종류

- To be continued...


