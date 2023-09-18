# SQL Injection

- SQL 삽입 공격
- 악의적인 사용자가 보안상의 취약점을 이용해 임의의 SQL을 주입하고 실행되게 하여 데이터베이스가 비정상적인 동작을 하도록 조작하는 행위
- 공격이 비교적 쉬운 편이고 성공할 경우 큰 피해를 입힐 수 있는 공격

## 공격 종류

- Error based SQL Injection
	- 논리적 에러를 이용
	- 가장 대중적인 기법
- Union based SQL Injection
	- Union 명령어를 이용
- Boolean based SQL Injection
	- True/False 정보만 알 수 있을 때 사용
- Time based SQL Injection
- 저장된 프로시저에서의 SQL Injection
	- 일련의 쿼리들을 모아 하나의 함수처럼 사용하기 위한 것
- 다량의 SQL Injection
	- 한 번의 공격으로 다량의 데이터베이스가 조작되는 것

## 대응 방안

- 입력 값에 대한 검증
- Prepared Statement 구문 사용
	- 사용하면 사용자의 입력 값이 데이터베이스의 파라미터로 들어가기 전에 DBMS가 미리 컴파일하여 실행하지 않고 대기
- Error Message 노출 금지
- 웹 방화벽 사용