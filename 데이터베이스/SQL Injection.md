# SQL Injection

악의적인 사용자가 응용 프로그램 보안 상의 허점을 의도적으로 이용해, 악의적인 SQL문을 주입하고 실행되게 하여 데이터베이스가 비정상적인 동작을 하도록 조작하는 행위이다.

# 방법

## 1. Error based SQL Injection

> 가장 대중적인 기법으로 SQL 쿼리에 고의적으로 오류를 발생시키고 이때 출력되는 에러의 내용으로 필요한 정보를 찾아내는 공격 기법.

로그인 페이지가 있고, 로그인을 할 때 USER_ID와 INPUT_PW를 입력받아 로그인이 진행된다고 했을 때

```sql
SELECT user FROM Users WHERE uid = 'USRE_ID' AND upw = 'INPUT_PW';
```

공격 예시 : 로그인 창의 ID 부분에

```sql
'OR 1 = 1 --
```

를 입력.

```sql
SELECT user FROM Users WHERE uid = '' OR 1 = 1 --USRE_ID' AND upw = 'INPUT_PW';
```

WHERE절에 있는 싱글 쿼터를 닫아주게 되고, `OR 1 = 1`로 모두 참을 만들어 준 후, `--`를 이용해 그 뒤의 모든 쿼리문을 주석처리해주게 된다.

결국, 테이블의 모든 행을 반환하게 되므로, 특정 사용자 계정이 아닌 첫 번째로 반환되는 사용자(Admin 계정)로 로그인하게 될 가능성이 크다.

## 2. Union base SQL Injection

UNION 키워드를 사용하여 원래의 요청에 추가 정보를 얻는 공격 기법으로 UNION 하려는 두 테이블의 컬럼 수와 데이터 형식이 같아야 한다. ORDER BY 절이나 HAVING을 이용한 오류 메시지를 통해 컬럼의 수를 유추할 수 있다.

게시판이 있고, 게시글을 검색할 때 INPUT을 받아 검색이 진행된다고 했을 때

```sql
SELECT * FROM Board WHERE title LIKE '%INPUT%' OR contents '%INPUT%'
```

공격 예시 : 검색 창에

```sql
UNION SELECT null, id, passwd FROM User --
```

입력.

```sql
SELECT * FROM Board WHERE title LIKE '%' UNION SELECT null, id, passwd FROM Users --%' OR contents '%INPUT%'
```

사전 공격을 통해 컬럼명과 테이블명을 얻은 후 사용자의 ID와 PW를 요청하는 쿼리문을 함께 입력하게 되면 사용자의 개인 정보가 게시글과 함께 보이게 된다.

## 3. Blind SQL Injection

> 에러가 발생되지 않는 사이트에서 데이터 베이스로부터 특정한 값이나 데이터를 전달받지 않고, 단순히 참과 거짓의 정보만 알 수 있을 때 사용하는 공격 기법. limit, SUBSTR, ASCII를 사용해서 조건이 참이면 페이지가 정상적으로 출력되고 그렇지 않을 경우 출력되지 않음으로 구분이 가능.

최근에는 에러 메시지를 출력하지 않는 웹 서버를 구축하고 있어 주로 사용하는 방식.

Boolean 기반 공격, Time 기반 공격 등이 있음.

```sql
SELECT user FROM Users WHERE uid = 'USRE_ID' AND upw = 'INPUT_PW'
```

공격 예시: 로그인 폼에 DB 테이블 명을 알아내기 위한 쿼리문을 주입, 이때 임의로 가입한 idd3이라는 아이디와 함께 구문을 주입.

```sql
SELECT user FROM Users WHERE uid = 'idd3' OR (LENGTH(DATABASE())=1 AND SLEEP(2)) -- USRE_ID' AND upw = 'INPUT_PW';
```

- 숫자 1을 조작해 현재 사용하고 있는 데이터 베이스의 길이를 알아낼 수 있다.
- LENGTH를 사용해 문자열 길이를 반환하도록 한다.
- DATABASE()를 사용해 데이터베이스의 이름을 반환한다.
- SLEEP단어가 치환 처리되었을 경우, BENCHMARK나 WAIT함수를 사용할 수 있다.

## 4. Stored Procedure based SQL Injection

저장 프로시저(Stored Procedure)는 쿼리들을 모아 하나의 함수처럼 사용하기 위한 것이다.

웹에서 저장 프로시저에 대한 접근 권한을 가짐으로써 실행이 가능해진다. 공격 난도가 높으나 성공 시 직접적인 피해를 입힐 수 있는 공격 기법.

## 5. Mass SQL Injection

한 번의 공격으로 다량의 DB가 조작해 큰 피해를 입히는 공격 기법.

# 예방 방법

1. Prepared Statements 사용: 사용자 입력을 직접 SQL 쿼리에 포함시키지 않고, Prepared Statement를 사용하여 SQL 인젝션을 방지해야 한다.

```java
String query = "SELECT * FROM Users WHERE username = ? AND password = ?";
PreparedStatement pstmt = connection.prepareStatement(query);
pstmt.setString(1, username);
pstmt.setString(2, password);
```

2. 입력값 검증: 사용자로부터 받은 모든 입력값을 철저히 검증하고, 특수 문자를 필터링한다.
3. ORM 사용: Hibernate 같은 ORM을 사용하면 SQL 인젝션 위험을 크게 줄일 수 있다.
4. 웹 애플리케이션 방화벽(WAF) 사용: SQL 인젝션 공격 패턴을 탐지하고 차단할 수 있는 WAF를 사용하는 것도 효과적이다.
5. 에러 메시지 숨기기: 서버가 에러 메시지를 노출하지 않도록 하고, 기본적인 오류 처리 방식을 개선하여 정보를 공격자에게 제공하지 않도록 해야 한다.

---

# 기타

![alt text](images/sql%20injection%201.png)
![alt text](images/sql%20injection%202.png)

https://youtu.be/FoZ2cucLiDs?si=EytyotLptGDo-FyB
https://www.wired.com/story/null-license-plate-landed-one-hacker-ticket-hell/

## 참고자료

https://velog.io/@33bini/DB-SQL-Injection
https://do-ha-computer.tistory.com/entry/SQL-Injection-%EC%9D%B4%EB%9E%80

# 질문

1. SQL Injection을 막기위한 방법 중 가장 효율적이고 대중적인 방법은 무엇인가요?
2. SQL Injection에서 Error-based 공격 기법이 어떻게 작동하나요?
