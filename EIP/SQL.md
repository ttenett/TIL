# 관계대수 & 관계해석
## 관계대수 연산자
    - 순수관계연산자 : 행을 선택, 수평적 부분집합 δ σ아톰(시그마) / 열 선택, 수직적 부분집합 프로젝트 π(파이기호)/ 두테이블 조인 **⋈**(보타이기호): 공통인거만 살아남고, 공통아닌건 빠짐. / 분리는 ÷(나누기)
    - 집합 연산자 : 합집합 교집합 차집 카데시
    - ≥ ≤ 이렇게 써야함. >= 이건 SQL
    - ∪합집합  ∩ 교집합
    - and = ^ , 컬럼구분시 콤마(,)

### DDL(Data Definition Language)

DDL 정의어 : 구조를 만듦. 데이터 저장할 박스.

M 조작어 : 그 박스를 가져오고 , 업데이트 삭제 등

C제어어: 컨트롤. 권한주거나 뺏거나 커밋, 롤백

1. DDL - CREATE DROP ALTER(수정) , TURNCATE(안의내용만 지움) RENAME

여러 속성들을  

- 무결성 제약조건 ( 무조건 앞에 CONSTRAINTS)를 줘야함.
    - 개체 무결성 : 기본키에 NOT NULL, 중복값 올수없음
    - 참조 무결성 : 참조하려는 테이블에 기본키와 일치해야 함.
    - 도메인 무결성 : 허용하는, 해당하는 도메인의 범위에 있어야 함. ex) 초등학교면 1~6까지 있어야함.
- 기본키PRIMARY KEY, 고유값 UNIQUE 주민번호 등 고유한 것. CHECK 도메인무결성제약조건(특수설정한 조건)
- 참조 FOREIGN KEY
- 연쇄삭제 CASCADE , 삭제안됨 RESTRICT .
- 어떤 테이블에 대해서 ON , 오름차순 ASC, 내림차순 DESC

VIEW

외부스키마: 뷰와 연관됨. 사용자가 보는것, 내용, 여러가지로 만들어질 수 있음.

- 논리적 독립성 존재 - 개념스키마가 바뀌어도 외부스키마에 영향을 미치지 않음.

개념스키마: 데이터베이스의 전체구조 제약조건, 한번만 만들어짐.

- 물리적 독립성 존재 - 내부스키마가 바뀌어도 개념스키마에 영향을 미치지 않음.

내부스키마: 저장장치 입장에서 본 제약조건. 글자몇개, 숫자 등등

- 트리거 : 어떤이벤트 발생시키면 연쇄적으로 다른테이블에도 내용이 바뀔수있는것.
    - stored function, sotred 프로시져 - 데이터베이스에 저장된 하나의 일처리를 하는 함수
    - sotred 패키지 - 비슷한걸 모아두는 폴더

**개**념설계 ERD 머리속에있는거 끄집어내서 ER다이어그램

**논**리설계 목표DBMS에 맞춰서 설계(계층형,망형,관계형, 객체지향형,NoSQL, USQL인지) / 트랜잭션 인터페이스 설계, 관계형이면 정규화작업 (잘게쪼개는)

**물**리설계 실제 저장장치 입장에서 설계 , 성능 , 용량, 정규화를 시켜놧으면 성능에 영향줄수있음. Join 연산이 발생하니까. → 파티션, 클러스터링(비슷한거 모아둠), 인덱스, 뷰, 프로시저 만들다던지. 하다가 안되면 역정규화(반정규화)로 성능을 높이게 됨.

삭제

DROP - 건물 부셔버림. 구조 아예삭제

Truncate - 구조는 그대로두고 초기화. 이사, 새집처럼 만듬

Delete - 그 안의 내용을 삭제, 안에있는거 버림. (설정값이나 뭐 이런건 남아있음)

### DML(Data Management Language)

1. DML - INSTER UPDATE DELETE SELECT

INSERT INTO 테이블명 ( 컬럼명 a, b, c)

values (집어넣을 내용’a’, ‘b’, ‘c’); 값은 홑따옴표 꼭 붙여주기 - 문자는 꼭 쓰고, 숫자는 있없상관무

PRIMARY KEY : 기본값

UPDATE 테이블명 

SET a속성=’바꿀값’ ,  b속성=’2’ 

WHERE 조건절

DELETE FROM 테이블명 

WHERE 삭제할조건

- DROP 전부폭파
- Truncate 집 초기화
- delete 집안에 물건들만 지우는것

BETWEEN  1 AND 2 : 범위 지정할때

WHERE 잔고 >= 1 AND 잔고 <= 2 도 가능

LIKE 연산 : 문자열 연

% 와일드문자가 있으면 LIKE를 써야함. 아무값이나 쓸 수 있음.

% : 있어도 되고 없어도되고 많이와도 되고 조금와도 되고 상관없음. 

_  언더바 : 무조건 한글자 들어가야함.

1) SELECT 필드, * 

DISTINCT ~~

FROM 테이블명

2) WHERE

AND/OR

3) GROUP BY 속성

HAVING 조건 집계함수-sum, avg 평균, count갯수 min max / sum(영어) >= 80

집계함수는 SELECT 절이나 having절에서만 나옴.

ORDER BY 정렬 ASC 오름차순 DESC 내림차순

SELECT DISTINCT 검색어 FROM 테이블명  : 중복제거

COUNT는 NULL값을 세지 않음. *을 한다면 NULL도 셈.

집합함수 UNION :  중복제거

UNION ALL: 중복 신경안쓰고 테이블 다 합침

INTERSECT : 교집합, 둘 다 공통으로 들고있는거.

외부조인 Left Outer Join : 왼쪽기준 정렬 Right Outer Join 오른쪽 기준 정렬

기준쪽에 있는건 다나오고, 아닌쪽꺼에만 있는건 NULL

Full Outer Join은 모두 다 넣는거 = 레프트,라이트조인한 후 유니온한거

 INNER Join 필드가 일치하는 레코드만 결합하는 조인일때

합친후 테이블명.필드명

- SELECT * FROM A, B 보통 INNER 조인인데, 아랫줄에 ON 어떤필드 어떤필드 같아야한다 = 이너조인, / 그냥 콤마를 쓰면 카티시안 곱이 됨.
- CROSS JOIN = 카티시안 곱 = 필드(열)수는 더하기, 레코드(행)수는 곱하기

AS = Alias  열이름을 변경함. 필드.

WHERE 필드명 IN (’찾을값’,’찾을값’) : 괄호안에 포함 된 것 갖고와라

NOT IN 포함안된거 

기본급의 최댓값 구하기 : MAX(기본급) AS 최댓값

다중행연산자 IN NOT ANY : 하나라도 포함, ALL : 모두 포함 

WHERE 조건 안의 () 내용이 단일값이면 단일행연산자 > < 가능, 두개이상이면 IN 같은 다중행연산자 사용.

- WHERE EXISTS = 행이 있는지만 확인.  IN은 (값들) 하나하나 다 맞는지 비교.
- WHERE 다음 속성명이 없을때 사용. 속성명 있으면 IN 사용가능., NOT EXISTS도 있음.  EXISTS는 키값 참조로 엮어줘야함. 테이블1.필드명=테이블2.필드명

GROUP BY는 해당 필드값만 합칠 수 있음. 문자는 못합치니까 그래서 avg 같은 집계함수만 올수있음.

### DCL (Data Control Language)

1. DCL - GRANT TO권한줌 REVOKE FROM권한뺏음 COMMIT, ROLLBACK, SAVEPOINT

커밋(반영) 롤백(원복)은 트랜잭션 특성에 더 자주나오는 개념

GRANT SELECT, INSERT, DELECT ON 테이블명 TO 사용자

모든 사용자 PUBLIC

INSERT 삽입, UPDATE 갱신, DELETE 삭제, SELECT 검색,조회

WITH GRANT OPTION :   GRANT 권한을 나눠줄수있음