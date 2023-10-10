# YAML

- 시스템 간에 데이터를 주고 받을 필요가 있을 때, 데이터의 연동과 호환성을 위한 **포맷에 대한 규칙**
- 기존에는 웹에서는 XML과 JSON으로 그리고 자바 프로젝트에서는 properties 파일을 이용하여 어떤 값들을 정의하고 저장하고 사용해 옴
	- XML은 사용하기 매우 까다롭고 가독성도 좋지 않음
	- JSON은 **주석을 달수 없는** 등 약간의 제한이 있고 쓸데없는 **중괄호와 대괄호의 남발**로 코드 길이가 강제적으로 길어지게 된다는 단점이 있음
- YAML은 고급 컴퓨터 언어에 친화적
- 주로 Doker Compose, Kubernetes, Flutter, Spring boot 프로젝트에서 설정파일을 정의할 때 사용됨

## YAML 문법 정리

### 데이터 정의
#### Key: Value
- 콜론을 기준으로 구분
- 콜론 뒤에는 띄어쓰기 한 번 해야함

```yaml
name: 배승원
age: 25
major: 컴퓨터
```

#### Indent
- 들여쓰기를 통해 계층 구조를 표현
- 객체를 표현할 때 이러한 계층 구조를 활용 

```yaml
person:
  name: 배승원
  age: 25
  major: 컴퓨터
```

#### 따옴표
- 큰 따옴표, 작은 따옴표를 쓰거나 아예 쓰지 않아도 자동으로 문자열로 인식됨
- 만약 콜론이 포함된 문자열일 경우 무조건 따옴표료 둘러싸야 인식됨

```yaml
name: 배승원
age: 25
major: "컴퓨터"
ratio1: "1:2"
ratio2: '1:2'
ratio3: 1:2  # 제대로 인식되지 않음
```

- 작은 따옴표와 큰 따옴표의 차이점
	- 큰 따옴표는 Escape Sequence를 처리
	- 작은 따옴표는 처리하지 않음

```yaml
string1: "\t tab \n NL"  # 이스케이프 처리
string2: '\t tab \n NL'
```

### 배열 & 리스트
#### 단순 배열
- 하이픈(-)으로 시작하는 하위 엘리먼트를 표현
- 하이픈 뒤에 띄어쓰기를 해야 리스트로 인식됨
- 하이픈 대신 대괄호로도 표현 가능

```yaml
ingrediens:
  - 닭
  - 튀김가루
  - 소스

ingredients: [닭, 튀김가루, 소스]
```

#### 객체 배열
- 단순한 값 리스트의 나열이 아닌 복잡한 구조체의 리스트를 표현하고자 할 때 하이픈과 Key: Value 구조를 이용해 표현
- 하나의 객체 쌍인 것을 표현하기 위해 하나의 객체 원소면 하이픈을 쓰지 않고 나열
- 하이픈 뒤에 Key: Value 구조가 온다면 이것은 객체를 포함한 배열, 그냥 데이터가 바로 온다면 단순 원소 배열이라고 인식

```yaml
students:
  - name: Mark
    major: Math
    age: 20
  - name: Julie
    major: Arts
    age: 23
  - name: Tommy
    major: Music
    age: 25
```

### Boolean

- 데이터 부분에 yes, no, true, false를 지정하면 Boolean 값으로 인식됨
- 대소문자를 가리지 않음

```yaml
booleans:
  - yes
  - Yes
  - YES
  - true
  - True
  - FALSE
  - "YES"
```

### 주석

- 샵(#)으로 주석을 표기

```yaml
# students
name: John
major: Math
```

### Text Multi Line

- 개행이 있는 문자열을 표현할 때는 > 문자나 | 문자를 사용

#### Folded Block Scalar
- > 기호는 개행 문자를 화이트 스페이스로 치환
- 빈 줄 하나가 단독으로 있는 경우만 줄바꿈으로 치환
- 맨 마지막은 자동으로 개행 처리를 해주지만, > 기호 옆에 - 기호를 붙이면 맨 끝의 줄바꿈 문자를 포함시키지 않음

```yaml
# 중간에 개행 문자가 있음
# 끝에도 개행 문자가 있음
paragraph1: >
  abc
  def
  ghi

  aab
  ccc
# 맨끝의 줄바꿈 문자를 포함시키지 않음
paragraph2: >-
  abc
  def
  ghi

  aab
  ccc
```

#### Literal Block Scalar
- | 기호는 개행 문자 그대로 줄을 바꿔줌
- | 기호 옆에 -를 붙여주면 맨 끝의 개행 문자를 포함시키지 않음

YAML

```yaml
# 그대로 줄바꿈 + 맨 끝 개행 문자 포함
paragraph3: |
  abc
  def
  ghi

  aab
  ccc

# 그대로 줄바꿈 + 맨 끝 개행 문자 제외
paragraph4: |-
  abc
  def
  ghi

  aab
  ccc
```

### Alias

- 재사용하는 값 또는 구조체가 있을 경우 변수와 비슷한 태그 기능을 사용할 수 있음
- & 기호로 변수를 선언하고 \* 기호로 변수를 참조할 수 있음

```yaml
# default_school 라는 변수를 선언
# 내용은 group 과 description 데이터를 지니고 있음
default: &default_school
   group: '아주대학교'
   description: |
      수원에 있는 대한민국의 사립 대학교

student:
   - name: '배승원'
     <<: *default_school # default_school 변수 내용물을 대입
   - name: '홍길동'
     <<: *default_school # default_school 변수 내용물을 대입
```


