# 2025.03.18
DCL (Date Control LANGUAGE) : 데이터 제어어
데이터베이스에 접근하고 객체들을 사용하도록 권한을 주거나 회수하는 명령어
GRANT, REVOKE

권한주기 : GRANT 권한명,권한명 TO 계정명;

개별적인 권한
CREATE USER
CREATE SESSION
CREATE TABLE

역할(ROLES)
여러 권한을 그룹화한 집함
CONNECT : 기본적인 데이터베이스 접속 권한을 제공
RESOURCE : 테이블, 인덱스 등 객체를 생성할 수 있는 구너한 포함
DBA : 데이터베이스의 모든권한을 포함하는 최고수준의 역할

테이블 스페이스
테이블들이 논리적으로 저장되는 공간을 의미
테이블스페이스는 저장장치에 파일 형태로 존재한다.

CREATE TABLESPACE 테이블 스페이스 명
DATAFILE '경로와 이름' .ddf
SIZE 크기M
(AUTOEXTEND ON NEXT 크기M)
(MAX 크기M)

DBA_DATA_FILES
오라클에서 사용되는 데이터 파일들의 세부정보를 제공하는 뷰
데이터 베이스의 물리적 저장소 구성 요소에 대한 다양한 정보를 포함하고 있다.

FILE_NAME
데이터 파일의 전체 경로와 파일명을 나타낸다.
실제 디스크상에 존재하는 파일의 위치를 확인할 수 있다.

TABLESPACE_NAME
해당 데이터 파일이 소속된 테이블스페이스의 이름을 보여준다.
AUTOEXTENSIBLE
데이터파일이 자동확장 기능으로 설정되어있는지 여부를 나타낸다.
'YES' 또는 'NO' 로 표시된다.

INDEX
자동으로 만들어지는 인데스
ㄴPRIMARY KEY, UNIQUE에 의해 자동으로 만들어진다.
수동인덱스
ㄴ사용자가 직접 생성한 INDEX를 의미

INDEX조회
SELECT *FROM ALL_INDEXS WHERE TABLE_NAME = '테이블 명'
인덱스를 사용하면 좋은곳
1. 데이터가 많은 곳
2. 삽입, 수정, 석제가 잘 일어나지 않는곳

인덱스의 삭제
공간을 너무많이 차지한다면 지우는것도 방법
DROP INDEX 인덱스명;

언제 인덱스가 사용되는가?
1. WHERE 절
-조건에 사용된 컬럼에 인덱스가 있으면 DBMS가 인덱스를 통해서 빠르게 해당행의 위치를 찾는다.
2. JOIN, ORDER BY, GROUP BY
-두 테이블을 조인하거나, 결과를 정렬 또는 그룹화 할 때도 인덱스가 사용된다.

인덱스가 제대로 작동하는지 확인하는 방법


EXPLAIN PLAN 사용하기
1. 실행할 쿼리 앞에 EXPLAIN PLAN FOR 를 추가한다.
ㄴ쿼리의 실행 경로흫 분석
ㄴ실행 계획 정보는 PLAN_TABLE 이라는 테이블에 저장된다
3. 그후 SELECT*FROM TABLE(DBMS_XPPLAN.DISPLAY); 명령을 실행하여 계획을 확인한다.

서브쿼리
-쿼리문 안에 쿼리문에 있는것
-DB에 여러번 접속할 거를 한번에 접속할 수 있다.
-가독성이 조금 떨어질 수 있다.

FROM정(IN LINE VIEW) : 쿼리문으로 출력되는 결과를 테이블 처럼 사용
SELECR정(SCALAR) : 하나의 컬럼처럼 사용되는 서브쿼리
WHERE(SUB QUERY) : 하나의 변수처럼 사용

UPDATE문에 SET에서 사용
DELETE문에서 WHERE에서 사용

CONCATENATION(연결)
-두개의 컬럼을 마치 하나의 컬럼처럼 연결

SELECR FIRST_NAME //' '//LAST_NAME FROM EMPLOYEES;
SELECR FIRST_NAME //'의 급여는'//SALART'//이다 FROM EMPLOYEES;

별칭(ALIAS)
SELECT절의 컬럼 뒤에 주는법
1. AS별칭 ("두 단어 이상의 별칭")
2. 별칭("두 단어 이상의 별칭")

FROM 절
ㄴ테이블 이름 대신 별칭을 사용하는게 가능

JOIN
ANSI 조인 : 표준JOIN 방법

INNER JOIN
-두 테이블간에 조인 조건에 일치되는 데이터만 가져온다.

SELECT e.FIRST_NAME, E.DEPARTMENT_ID, D.DEPARTMENT_NAME
FROM EMPLOYEES (INNER) JOIN DPARTMENTS D
ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
(INNER) 는 생략해도됨.

on과 where 의 차이
on : 조인시 두 테이블의 행위 결합 조건을 지정
특히 외부 조인에서는 어떤 행을 결합할지 결정하는 중요한 역할을 한다

where : 결합된 결과 집합에 대해 추가적인 필터링을 적용한다.


--CROSS INNER JOIN
--두개이상의 테이블에서 '모든 가능한 조합' 을만들어 결과를 반환하는 조인방법
이를통해 두 개 이상의 테이블을 조합하여 새로운 테이블을 만들 수 있다.
두테이블이 서로관현이 없어도 조인이 가능하다.
--outer join
두 테이블에서 '공톤된 값을 가지지 않는 행들' 도 반환한다
--LEFT OUTER JOIN
'왼쪽 테이블의 모든 행 ' 과 '오른쪽 테이블과 왼쪽 테이블이 공통적으로 가지는값을 반환한다.
교집합과 차집합의 연산 결과를 합친것과 같다.
만약 오른쪽 테이블에서 공통된 값을 가지고 있는 행이 없다면 NULL을 반환한다.

--사원 테이블과 부서테이블의 LEFT OUTER JOIN 을 이용하여 
사원이 어느 부서에 있는지 조회하기
SELECT
FROM EMPLOYEES e
LEFT OUTER JOIN DEPARTMENTS d
ON e.department_id = d.DEPARTMENT_ID;

LEFT 와 RIGHT 중에 뭘 많이 쓸까?
상황에 따라 다르다...
대부분 왼쪽 테이블의 데이터를 중심으로 분석하고자 하는경우가 많기 때문에 LEFT 를 더많이 사용하는것 같다.

--FULL OUTER JOIN
두 테이블에서 '모든값' 을 반환한다.
서로 공통되지않은 부분은 NULL 로 채운다
합집합의 연산과 같다.

--부서번호, 사원명,  직업, 위치를 EMP와 DEPT테이블을 통해
INNER JOIN 하여 조회하기

--PLAYER 테이블, TEAM 테이블에서 송종국 선수사 속한 팀의 전화번호 조회하기
--팀 아이디, 선수 이름, 전화번호 조회

JOBS테이블과 EMPLOYEES테이블 에서
직종번호, 직종이름, 이메일, 이름과 성(연걸) 별칭을 이름으로 하고 조회하기
--다양한 비교 연산자를 통해 조인조건을 지정하는방식
--특정값이 일정 범위내에 속하거나, 두값사이의 관계를 비교하여 행을 결합할떄 유용하다.

--DEPT 테이블의 LOC별 평균 SAL 를 반올림한 값과, SAL 의 총합을 조회해주세요

--자동매칭
두 테이블에서 이름이 동일한 컬럼을 찾아서, 해당 컬럼들이 값이 일치하는 행끼리 자동으로 결합한다.
--중복컬럼제거
--조인결과에서 공통컬럼은 한 번만 표시된다.
--명시적 조건 생략
--ON 절이나 USING절 없이 간단하게 조인할 수 있다.
--의도하지 않은 결과가 나올 수 있다.
--자동으로 공통 컬럼을 기준으로 조인하기 떄문에, 개발자가 어떤 컬럼을 기준으로 조인하는지 명확히 알기 어려울 수 있다.

--집합 연산자
-JOIN과는 별개로 두개의 테이블을 합치는 방법

1. UNION ALL
--두 개의 테이블에서 '중복을 제거하고 합친 모든 행' 을 반환
SELECR FIRST_NAME FROM EMPLOYEES
UNION ALL
SELECT DEPARTMENT_NAME FROM DEPARTMENTS;

--VIEW
--하나이상의 테이블이나 다른뷰의 데이터를 볼 수 있게 해주는
--데이터베이스 객체이다.
--쿼리문을 작성하면 -> 1회성
--VIEW형태로 저장해서
--쿼리 결과를 마치 하나의 테이블처럼 사용할 수 있도록 하는 가상테이블
--실제 데이터는 저장하지 않고, 뷰를 참조할 때마다 미리정의된 
--쿼리문이 살행되어 결과가 생성된다.
--테이블 뿐만 아니라 다른뷰를 참조해 새로운 뷰를 만들 수도 있다.

--사용목적
--여러 테이블에서 필요한 정보를 사용할때가 많다.
--VIEW에 저장하면 간단하게 호출할 수 있다. 

--VIEW의 특징
--독립성
--테이블 구조가 변경되어도 뷰를 사용하는 응용프로그램은 변경하지 않아도 된다.

--편리성
--복잡한 쿼리문을 뷰로 생성함으로써 관련 쿼리를 단순하게 작성할 수 있다.
--자주 사용하는 SQL문이면 뷰에 저장하고 편리하게 사용할 수 있다.

--보안성
SELECT* EMPNO,ENAME,JOB,MGR,HIREDATE,COMM,DEPTNO FROM EMP;
--숨기고 싶은 정보가 존재한다면, 뷰를 생성할 때 해당 컬럼은 빼고
--생성함으로써 사용자에게 정보를 감출 수 있다. 

--VIEW의 생성
/*
*CREATE VIEW 뷰이름 AS(
*     쿼리문 
*)
**/

--OR REPLACE 옵션은 기존의 정의를 변경하는 데 사용할 수 있다.

/*
*CREATE OR REPLACE 뷰이름 AS(
*     쿼리문 
*)
**/


--VIEW의 삭제
--DROP VIEW 뷰이름[RESTRICT OR CASCADE]
--RESTRICT : 뷰를 다른곳에서 참조하고 있다면 삭제가 취소
--CASCADE : 뷰를 참조하는 다른뷰나 제약 조건까지 모두 삭제된다.

SELECT * FROM PLAYER;
--선수의 이름과 나이를 조회하세요
--EX) 홍길동 35 

--사원 이름과 상사이름을 조회하기
--king Steven의 부하직원이 몇명인지 조회하세요
SELECT count(*) FROM
employees_manager
WHERE mname='King Steven';

--player테이블에 team_name 컬럼을 추가한 view 만들기
--view 이름은 player_team_name
--HOMETEAM_ID, STADIUM_NAME, TEAM_NAME을 조회
--HOMETEAM이 없는 경기장 이름도 나와야함

--사원테이블에서 급여, 급여를 많이받는 순으로 순위를 조회
--DATA_PLUS라는 VIEW에 저장

--case문은 하나의 값이나 결과를 반환하는 표현식이기 때문에
--selecr, orderby, group by등의 구문 내에서 사용될 수 있다.
가독성향상
--복잡한 조건에 따른 값을 한눈에 파악할수 있게 도와주어, 쿼리의 가독성과 유지보수성이 높아진다.
null처리
조건에 해당하지 않는 경우 else절이 ㅇ벗으면null이반환

중첩사용가능
case문 안에 case문을 중첩해서 사용할 수 있다.
표준sql지원
대부분의 주요 데티어 베이스등에서 표준sql문법의 일부로 지원된다.
 --pl/sql문
 원래 sql문은 주로 데이터의 정의, 조작, 제어를 위한언어
 --pl/sal
 절차적 기능을 추가하여 복잡한 로직을 구현할 수 있게 해준다.

--PL/SQL문의 구조
--기본적으로 블록단위로 구성. 하나의 블록은 세 부분으로 이루어져 있다.
--선언부(DECLARE) : 변수, 상수, TKDYDWKWJDDML XKDLQDMF TJSDJS
--실행부(BEGIN...END): 실제 로직이 작성되는부분
--예외처리부(EXEPTION) : 실행 도중 발생하는 오류를 처리하는 구문 작성

--1부터 10까지
--X는 짝수입니다.
--X는 홀수 입니다.
--출력하기















