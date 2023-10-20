# Basic
## JavaScript를 실행하는 방법

1. 브라우저 개발자 도구의 콘솔 창 이용
2. HTML 문서 안에 내장
	- \<script> 태그 이용
	- .js 파일 생성

## JavaScript에서 함수를 정의하는 방법
### 1. 함수 선언문

```Javascript
function add(x, y) {
	return x + y;
}

console.log(add(10, 20));
```

### 2. 함수 표현식

```Javascript
// 익명 함수 표현식
let add = function(x, y) { return x + y; }

// 기명 함수 표현식
let sum = function add(x, y) { return x + y; }
```

- 함수 표현식을 이용하면 함수를 변수처럼 사용 가능
- 익명 함수 표현식
- 기명 함수 표현식
	- 함수 외부에서 함수 이름으로 사용할 수 없으나, 함수 내부에서 재귀 호출 시 사용 가능
- 함수 선언문으로 정의한 함수는 자바스크립트 내부에서 함수 이름과 함수 변수 이름이 동일한 함수 표현식으로 자동 변경

### 3. Function() 생성자 함수 사용

```Javascript
new Function([arg1[, arg2[, ... argN]]], functionBody)

let add = new Function('x', 'y', 'return x + y');
```

## Function Hoisting

- 함수 선언문으로 함수를 선언했을 때 함수 호이스팅이 발생
	- 함수 선언이 최상단으로 이동
	- 함수를 선언하기 전에 호출하는 것이 가능

```javascript
console.log("20 + 30 = ", add(20, 30));
// 20 + 30 = 50
// 함수 선언이 호이스팅하기 때문에 가능

function add(x, y) {
	return x + y;
}

console.log("30 + 40 = ", add(30, 40));
// 20 + 30 = 50
```

- 함수 표현식으로 함수를 선언하면 함수 선언 이전에 함수를 호출할 수 없음

```javascript
console.log("20 + 30 = ", add(20, 30));
// Uncaught ReferenceError: Cannot access 'add' before initialization

let add = function(x, y) {
	return x + y;
}
```

- 함수 표현식으로 함수를 선언할 때는 반드시 함수 선언 후 함수를 호출해야 함

```javascript
let add = function(x, y) {
	return x + y;
}

console.log("30 + 40 = ", add(30, 40));
// 30 + 40 = 70
```

## 변수 선언 (var, let, const)

```javascript
// undefined
console.log(i);

// 변수의 선언이 호이스팅되어 위치와 관계 없이 사용이 가능
var i;              
// undefined
console.log(i);     
i = 10;
// 10
console.log(i);     

// undefined
console.log(x);
// 변수의 선언과 초기화가 분리되어 변수의 선언이 호이스팅됨
var x = 10;
// 10
console.log(x);

// Uncaught ReferenceError: Cannot access 'j' before initialization
console.log("j = ", j);
let j;
// j = undefined
console.log("j = ", j);
j = 10;
// j = 10
console.log("j = ", j);

// Uncaught ReferenceError: Cannot access 'y' before initialization
console.log("y = ", y); 
let y = 10;
// y = 10
console.log("y = " , y);

// Uncaught TypeError: sum is not a function
console.log(sum(10, 20));
var sum = function(x, y) {
	return x + y;
};
// 30
console.log(sum(10, 20));

// Uncaught ReferenceError: Cannot access 'sum' before initialization
console.log(sum(10, 20));
let sum = function(x, y) {
	return x + y;
};
// 30
console.log(sum(10, 20));

const a = 100;
// Uncaught TypeError: Assignment to constant variable.
a = 200;

// 'const' declarations must be initialized.
const a;
a = 200;    

// 함수 표현식은 const로 많이 씀
const add = function(x, y) {
	return x + y;
};
console.log(add(10, 20));
```

## 함수 객체

```javascript
// 객체 선언
let obj = {
	"key": "value", 
	"first name": "hong"
};

// 객체의 요소를 참조
console.log(obj.key);
console.log(obj["key"]);
console.log(obj["first name"]);

// 새로운 요소를 추가
obj.email = "test@test.com";
obj["last name"] = "gil dong";
console.log(obj);

// 기존 요소를 수정
obj.key = "NEW VALUE";
console.log(obj);

// 함수를 정의
function add(x, y) {
	return x + y;
}
console.log(add(10, 20));
add.result = add(10, 20);
add.status = "OK";
console.log(add.result);
console.log(add.status);
console.dir(add);
// 함수 자체가 객체
```

## 함수를 다른 함수의 인자로 전달

```javascript
let click = function(fname) {
	fname();
};

let myfunc = function() {
	console.log('my function');
};

let yourfunc = function() {
	console.log('your function');
};

click(myfunc);
click(yourfunc);
```

## 함수를 다른 함수의 리턴값으로 활용

```javascript
let foo = function() {
	return function() {
		console.log("return function")
	};
};

let bar = foo();
// return function
bar();
```

## Callback Function

- 개발자가 코드를 이용해서 명시적으로 호출하는 함수가 아니고, 개발자는 단지 함수를 등록하기만 하고, 어떤 이벤트가 발생하거나 특정 시점에 도달했을 때 시스템에서 호출하는 함수
- e.g., Event Handler

## 즉시 실행 함수 = 자기 호출 함수

- 함수 정의와 동시에 바로 실행하는 함수
- 함수 표현식을 괄호로 둘러싼 후 바로 호출(실행)할 수 있도록 괄호 쌍을 추가

```javascript
(function(name) { 
	console.log(name + '는 즉시 실행됩니다.'; 
})("본 함수");
```

## Array

```javascript
// 배열 선언
let values = ['빨강', "노랑", `파랑`, true, 123];

// 배열 길이는 배열의 length 속성을 이용해 확인이 가능
console.log(values.length);
console.log(values["length"]);

// 배열 요소에 접근할 때는 일반적으로 배열의 인덱스를 사용
console.log('첫번째', values[0]);
console.log('마지막', values[values.length - 1]);

// 배열 요소에 순차적으로 접근하는 방법
console.log('방법 1');
console.log(values[0]);
console.log(values[1]);
console.log(values[2]);
console.log(values[3]);
console.log(values[4]);

console.log('방법 2');
for (let i = 0; i < values.length; i++) {
	console.log(values[i]);
}


console.log('방법 3');
for (let index in values) {
	console.log(values[index]);
}

console.log('방법 4')
for (let value of values) {
	console.log(value)
}

console.log('방법 5-1')
let print = function(color) {
	console.log(color);
};
values.forEach(print);

console.log('방법 5-2')
values.forEach(function(color) {
	console.log(color);
});

console.log('방법 5-3')
values.forEach((color) => {
	console.log(color);
});

// 이렇게 처리해야 함
console.log('방법 5-4')
values.forEach(color => console.log(color));
```

## 객체

```javascript
// 객체 선언
let person = {
	"name": '배승원',
	age: 25,
	isMarried: false,
	'favorite colors': ["빨강", "파랑"],
	hello: function() {
		return '안녕하세요. ' + this.name + `입니다.`;
	}
};

// 객체 속성 참조 -> 객체_변수.속성_이름 or 객체_변수["속성_이름"]
console.log(person.name);
console.log(person["name"]);
console.log(person["favorite colors"]);
console.log(person.hello());

// 객체 속성 값 변경
person.name = "배찬우";

// 객체의 모든 속성을 순회하면서 출력 for-of는 사용 불가능 (Objects are not iterable)
for (let key in person) {
	console.log(key, ' 속성의 값은 ', person[key]);
}

// in 연산자: 해당 속성이 객체 안에 존재하는지 여부를 확인
console.log("name" in person); // true
console.log("email" in person); // false

// with 연산자: 사용할 객체를 지정, 값을 참조할 때만 가능
console.log(`name: ${person.name}`);
console.log(`age: ${person.age}`);
with(person) {
	console.log(`name: ${name}`);
	console.log(`age: ${age}`);
}

// 객체가 가지고 있는 모든 속성과 속성의 값을 출력하는 함수를 객체에 추가
person.print = function() {
	for (let key in person) {
		if (key !== 'print') {
			console.log(key, person[key]);
		}
	}
};

// 객체 속성 추가
let person = {};

person.name = "배승원";
person.age = "33";
person["favorite colors"] = ["빨강", "파랑"];

// 객체 속성 삭제
delete person.age;
```

### 객체와 배열을 이용한 데이터 처리

```javascript
let scores = [
	{name: '홍길동', korean: 80, math: 90, english: 90},
	{name: '고길동', korean: 90, math: 80, english: 80},
	{name: '신길동', korean: 70, math: 80, english: 70}
];

console.log(`--------\t------\t------\t------\t------\t------`);
console.log(`학생이름\t국어\t영어\t수학\t합계\t평균`);
console.log(`--------\t------\t------\t------\t------\t------`);
scores.forEach(score => {
	score.sum = () => this.korean + this.math + this.english;
	score.average = function() {
		let result = this.sum() / 3;
		return Math.round(result * 10) / 10;
	};
	
	console.log(`${score.name}\t${score.korean}\t${score.math}\t${score.english}\t${score.sum()}\t${score.average()}`)
});
```

## 단축 속성명 (Shorthand Property Names)

```javascript
let name = "Bae";

const obj1 = {
	age: 21,
	name: name,
	getName: function getName() {
	return this.name
	}
};

console.log(obj1);
console.log(obj1.getName()); // Bae

const obj2 = {
	age: 21,
	name,  // 속성 이름 생략: 속성 이름과 속성 값을 가지고 있는 변수 이름이 같은 경우
	getName() { // 속성 이름과 함수 이름이 동일한 경우 function 키워드와 함께 생략
		return this.name
	}
};

console.log(obj2);
console.log(obj2.getName());  // Bae
```

### Use Cases
#### 1. 매개변수를 객체로 반환하는 함수를 정의하는 경우

```javascript
function returnObject1(age, name) {
	return {
		age: age,
		name: name
	};
}

function returnObject2(age, name) {
	return {age, name}
}

// 동일한 결과
```

#### 2. Log를 출력할 때

```javascript
const age = 20;
const name = "승원";

// 방법 1
console.log("age", age);
console.log("name", name);

// 방법 2
console.log(`age=${age}, name=${name}`);

// 방법 3
console.log({age: age, name: name});

// 방법 4
console.log({age, name});

// 동일한 결과
```

## 계산된 속성명 (Computed Property Names)

```javascript
// 속성 이름(key)와 속성 값(value)를 전달받아 객체를 반환하는 함수
function returnObject1(key, value) {
	return {[key]: value}
}

console.log(returnObject1('name', 'seungwon'));
// {name: 'seungwon'}

// 속성 이름이 일련번호 형태를 가지는 객체를 반환하는 함수
function returnObject2(key, value, no) {
	return {[key+no]: value};
}

console.log(returnObject2('name', 'seungwon', 1));
// {name1: 'seungwon'}
console.log(returnObject2('name', 'chanwoo', 2));
// {name2: 'chanwoo'}
```

## 전개 연산자 (Spread Operator)

- 배열 또는 객체의 값을 반환하는 연산자

```javascript
console.log(Math.max(10, 20, 1, 14, 3, 130, 8)); // 130
console.log(Math.max([10, 20, 1, 14, 3, 130, 8])); // NaN

const numbers = [10, 20, 1, 14, 3, 130, 8];
console.log(Math.max(numbers)); // NaN
console.log(Math.max(...numbers)); // 130

console.log(numbers) // 배열
console.log(...numbers) // 순열 (숫자의 나열), 배열 요소의 나열
```

### Use Cases
#### 1. 배열의 값을 복사할 때

```javascript
let arr1 = [1, 2, 3];
let arr2 = arr1;
// arr1을 수정하면 arr2도 바뀜

let arr3 = [1, 2, 3];
let arr4 = [...arr3];
// arr3의 값만 arr4로 옮겨감
```

#### 2. 객체의 값을 복사할 때

```javascript
let obj1 = {
	age: 25, 
	name: "seungwon"
};
let obj2 = obj1;
// obj1의 값을 수정하면 obj2의 값도 바뀜

let obj3 = {
	age: 25,
	name: "seungwon"
};
let obj4 = {...obj3};
// obj3의 값만 obj4로 옮겨감
```

#### 3. 객체를 복사하는 과정에서 새로운 속성을 추가하거나 속성의 값을 변경하는 경우

```javascript
let obj1 = {
	age: 25,
	name: "seungwon"
};

let obj2 = {...obj1, name: "chanwoo"};
// 복사하면서 값도 수정

let obj3 = {...obj2, email: "test@test.com"};
// 복사하면서 값도 추가
```

#### 4. 배열 또는 객체를 결합할 경우

```javascript
// 배열의 결합
const arr1 = [1, 2, 3];
const arr2 = [7, 8, 7];

const arr3 = [...arr1, ...arr2];
const arr4 = [...arr2, ...arr1];
// 순서가 달라지면 결과도 달라짐

// 객체의 결합
const obj1 = {
	age: 25,
	name: "seungwon"
};
const obj2 = {
	hobby: "swimming"
	name: "chanwoo"
};

const obj3 = {...obj1, ...obj2};
const obj4 = {...obj2, ...obj1};
// 뒤에 나오는 객체와 속성이 겹칠 경우 뒤에 있는 객체의 속성 값으로 덮어씌워짐
```

## 배열 비구조화 (Array Destructuring)

- 배열 데이터를 변수에 나눠서 할당

```javascript
const arr = [1, 2, 3, 4, 5];
let a = arr[0];
let b = arr[1];

// arr 배열의 0번째부터 3번째 인덱스에 해당하는 값을 c, d, e, f 변수에 차례로 할당
let [c, d, e, f] = arr;
console.log(c, d, e, f);
// 1 2 3 4
```

### Use Cases
#### 1. Swaping

```javascript
let x = 10;
let y = 20;

[x, y] = [y, x];
// [비구조화] = [배열]
```

#### 2. 배열의 일부값을 변수의 할당할 경우

```javascript
const arr = [ 1, 2, 3 ];
let a = 10, b = 20, c = 30;

console.log(a, b, c);
// 10 20 30

**

// 변수 a에 1을, 변수 c에 3을 재할당하고 변수 b는 20을 그대로 유지
[ a, , c ] = arr;

console.log(a, b, c);   
// 1 20 3
```

#### 3. 배열의 값을 할당하고 남은 나머지를 새로운 배열로 만드는 경우

```javascript
const arr = [ 1, 2, 3, 4, 5 ];

// arr 배열의 첫번째 값을 first 변수에 넣고, 나머지 값을 rest라는 이름의 배열에 추가
const [first, ...rest] = arr;

console.log(first); // 1
console.log(rest); // [2, 3, 4, 5]
```

## 객체 비구조화 (Object Destructuring)

- 객체 비구조화 할 때는 속성명(Key)이 중요

```javascript
const obj1 = { age: 21, name: 'mike' };
const obj2 = { age: 40, name: 'john' };

// obj1의 age와 name 속성의 값을 age와 name 변수에 할당
const { age, name } = obj1;
console.log(age, name); // 21 'mike'
const { ageNew, nameNew } = obj1;
console.log(ageNew, nameNew); // undefined undefined
console.log(obj1.ageNew); // undefined

// 객체 비구조화를 통해 가져온 값을 새로운 변수 이름으로 사용할 경우 => 별칭을 부여
const { age: ageNew, name: nameNew } = obj1;
console.log(ageNew, nameNew);

// 객체에 존재하지 않는 요소를 변수에 할당할 때 적용할 기본값 설정도 가능
const { age, name: nameNew, email='default' } = obj1;
console.log(age, nameNew, email); // 21 'mike' 'default'       

// 객체 비구조화 시 객체의 일부 속성을 변수에 할당하고, 나머지를 새로운 객체로 저장
const obj3 = { age: 32, name: "John", grade: "c" };

// obj3의 age 속성의 값을 johnAge 변수에 할당하고, 나머지를 rest 이름의 객체에 할당
const { age: johnAge, ...rest } = obj3;
console.log(johnAge, rest); // 32 {name:'John', grade:'c'}
```

## 화살표 함수 (Arrow Function)

```javascript
// 익명 함수 표현식을 이용한 함수 정의 
let add = function (a, b) { return a + b; };

// 화살표 함수
// function 키워드를 제거하고, 함수 파라미터와 본문 사이에 => 를 추가
let add2 = (a, b) => { return a + b; };

// 화살표 함수 본문의 중괄호를 제거하면 화살표 오른쪽의 결과를 반환
let add3 = (a, b) => a + b;
console.log(add3(10, 20)); // 30

// 파라미터가 하나이면 파라미터를 감싸고 있는 소괄호도 생략 가능
let add4 = a => a + 4;
console.log(add4(10)); // 14

// 객체를 반환하는 경우에는 소괄호를 감싸야 함
let add5 = (a, b) => { return { result: a+b }; };
console.log(add5(10, 20)); // {result: 30}

let add6 = (a, b) => ( { result: a+b } );
console.log(add6(10, 20)); // {result: 30}
```

## map() 함수

```javascript
const source = [ 1, 4, 9, 16 ];

// source 배열의 값을 두 배수한 결과 배열(twoTimes)을 만들어서 출력하시오. 
const twoTimes = [];
for (let i = 0; i < source.length; i ++) {
	twoTimes[i] = source[i] * 2;
}
console.log(twoTimes);  // [2, 8, 18, 32]

const twoTimes = source.map(value => value * 2);
console.log(twoTimes);  // [2, 8, 18, 32]

const f = value => value * 2;
const twoTimes = source.map(f);
console.log(twoTimes);  // [2, 8, 18, 32]

const f1 = v => v * 2;
const f2 = v => v * 10;
const twoTimes = source.map(f1).map(f2);
console.log(twoTimes);  // [20, 80, 180, 320]
```

## filter() 함수

```javascript
const words = [ 'srpay', 'limit', 'elite', 'destruction', 'present', 'exuberant' ];

// 길이가 여섯 글자 이상인 단어만 추출
const newWords = [];
for (let i = 0; i < words.length; i ++) {
	if (words[i].length > 6) {
		newWords.push(words[i]);
	}
}

console.log(newWords);  // ['destruction', 'present', 'exuberant']

const newWords = words.filter(word => word.length > 6);
console.log(newWords);  // ['destruction', 'present', 'exuberant']

const numbers = [ 1, 3, 4, 6, 11, 14 ];
// 짝수만 추출해서 10배수한 결과를 출력
console.log(numbers
			.filter(num => num % 2 === 0)
			.map(num => num * 10));     // [40, 60, 140]
```

## reduce() 함수

```javascript
const numbers = [ 1, 2, 3, 4, 5 ];

// 배열 데이터의 합계
let sum = 0;
for (let i = 0; i < numbers.length; i ++) {
	sum = sum + numbers[i];
}
console.log(sum);   // 15

let sum = numbers.reduce((previous, current) => previous + current);
console.log(sum);

// numbers 배열의 각 항목의 값에 13을 곱한 결과 중 짝수의 합을 구하시오. 
const f1 = n => n * 13;
const f2 = n => n % 2 === 0;
const f3 = (p, c) => p + c;

const result1 = numbers.map(f1);
console.log(result1);
const result2 = result1.filter(f2);
console.log(result2);
const result3 = result2.reduce(f3);
console.log(result3);
console.log(numbers.map(f1).filter(f2).reduce(f3));
console.log(numbers
			.map(n => n * 13)
			.filter(n => n % 2 === 0)
			.reduce((p, c) => p + c));

```