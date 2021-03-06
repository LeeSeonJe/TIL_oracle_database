# DQL.md
### DQL - SELECT
+ 데이터를 조회한 결과를 Result Set이라고 하는데 SELECT구문에 의해 반환된 행들의 집합을 의미함
+ Result Set은 0개 이상의 행이 포함될 수 있고 특정 기준에 의해 정렬 가능
+ 한(여러) 테이블의 특정 컬럼, 행, 행/컬럼 조회 가능


  + 작성법
  ```sql
    SELECT 컬럼명[,컬럼명, ...]  // 조회하고자 하는 컬럼명 기술
    FROM 테이블명 // 조회 대상 컬럼이 포함된 테이블명
    WHERE 조건식; // 행을 선택하는 조건 기술
  ```
    + SELECT 예시
    ```sql
      -- 직원 전부의 모든 정보를 조회하는 구문
      SELECT * FROM EMPLOYEE
      
      -- 컬럼 값에 대해 산술 연산한 결과 조회 
      SELECT EMP_NAME, SALARY * 12, (SALARY + (SALARY*BONUS)) * 12
      FROM EMPLOYEE;
      
      -- *(아스트로)는 혼자사용해야 한다.
      SELECT *, SALARY * 12 
      -- 2개의 * 가 들어갔다. 허용되지 않아 에러가 발생된다. 
      -- 전체를 뽑거나 곱하기를 하거나 둘 중 하나만 해야한다.
      FROM EMPLOYEE;
      
      -- 반올림, 올림, 내림, 버림 사용하기
      SELECT SYSDATE - HIRE_DATE 근무일수, ROUND(SYSDATE-HIRE_DATE) 반올림, CEIL(SYSDATE-HIRE_DATE) 올림,
              FLOOR(SYSDATE - HIRE_DATE) 내림, TRUNC(SYSDATE - HIRE_DATE) 버림
      FROM EMPLOYEE;      
      
      -- 컬럼 별징
      -- 컬럼명 AS 별칭
      -- 컬럼명 "별칭" // 별칭에 띄어쓰기, 특수문자, 숫자가 포함될 경우 무조건 ""으로 묶는다.
      -- 컬럼명 AS "별칭" 
      -- 컬럼명 별칭
      
      -- EMPLOYEE테이블에서 직원의 직원명(별칭 : 이름), 연봉(별칭 : 연봉(원)), 보너스를 추가한 연봉(별칭 : 총소득(원)) 조회
      SELECT EMP_NAME 이름, SALARY * 12 "연봉(원)", (SALARY * (1+BONUS))*12 AS "총소득(원)"
      FROM EMPLOYEE;

      -- EMPLOYEE테이블에서 이름, 고용일, 근무일수(오늘날짜 - 고용일) 조회
      SELECT EMP_NAME AS "이름", HIRE_DATE AS "고용일", SYSDATE-HIRE_DATE "근무일수"
      FROM EMPLOYEE;
    ```
+
  + 컬럼명 "별칭" // 별칭에 띄어쓰기, 특수문자, 숫자가 포함될 경우 무조건 ""으로 묶는다.
  + 가급적 ""(더블 커텐션)으로 묶도록 연습하자.!
 
+ 리터럴
  + 임의로 정한 문자열을 SELECT절에 사용하면 테이블에 존재하는 데이터처럼 사용 가능  
  문자나 날짜 리터럴은 ' ' 기호가 사용되며 모든 행에 반복 표시됨
  
  ```SQL
    SELECT EMP_ID "직원번호", EMP_NAME "사원명", SALARY "급여",'원' "단위"
    FROM EMPLOYEE;
    
    -- EMPLOYEE테이블에서 퇴사 여부가 N인 직원을 조회하고
    -- 근무 여부를 재직중으로 표시하여 사번, 이름, 고용일, 근무 여부 조회
    SELECT EMP_ID, EMP_NAME, HIRE_DATE, '재직중' "근무여부"
    FROM EMPLOYEE
    WHERE ENT_YN = 'N';
  ```
  
+ DISTINCT
  + 컬럼에 포함된 중복 값을 한 번씩만 표기하고자 할 때 사용  
    **SELECT절에 딱 한 번만 쓸 수 있고 맨 앞에 써야한다.**
  ```SQL
    -- EMPLOYEE테이블에서 직원의 직급코드를 중복제거 하여 조회
    SELECT DISTINCT JOB_CODE
    FROM EMPLOYEE;
    
    -- EMPLOYEE테이블에서 부서코드와 직급코드를 중복제거 하여 조회
    SELECT DISTINCT DEPT_CODE, DISTINCT JOB_CODE
    FROM EMPLOYEE;
    -- 에러가 발생한다.!! DISTINCT는 하나만 써야한다.!!
    
    -- 올바른 사용법
    SELECT DISTINCT DEPT_CODE, JOB_CODE
    FROM EMPLOYEE;
  ```
>
### DQL - WHERE
+ WHERE절이란?
  + SELECT에 걸리는 조건문이 들어가는 절  
  조회할 행들에서 조건이 맞는 값만 가진 행을 골라냄
>
+
  + 연결연산자
    + '||'를 사용하여 여러 컬럼을 하나의 컬럼인 것 처럼 연결하거나 컬럼과 리터럴을 연결함.
    >
    ```SQL
      -- EMPLOYEEE테이블에서 사번, 이름, 급여를 연결하여 조회
      SELECT EMP_ID || EMP_NAME || SALARY
      FROM EMPLOYEE;
      
      -- EMPLOYEE테이블에서 '사원 명의 월급은 급여원입니다.' 형식으로 조회
      SELECT EMP_NAME || '의 월급은 ' || SALARY || '원입니다'
      FROM EMPLOYEE;
    ```
    >
    
  + 비교연산자  
  
      |연산자|설명|
      |:------:|:---:|
      |=|같다|
      |>,<|크다/작다|
      |>= , =< |크거나 같다 / 작거나 같다|
      |<> , != , ^=|같지 않다|
      |BETWEEN AND|특정 범위에 포함되는지 비교|
      |LIKE / NOT LIKE|문자 패턴 비교|
      |IS NULL / IS NOT NULL|NULL 여부 비교|
      |IN / NOT IN|비교 값 목록에 포함/미포함 되는지 여부 비교|
      + #### SQL EXAMPLE
      + '='
      ```SQL
        --EMPLOYEE테이블에서 부서코드가 'D9'인 직원의 이름, 부서코드 조회
        SELECT EMP_NAME, DEPT_CODE
        FROM EMPLOYEE
        WHERE DEPT_CODE = 'D9';
      ```
      + '>,<'
      ```SQL
        -- EMPLOYEE테이블에서 급여가 4000000 초과인 직원의 이름, 급여 조회
        SELECT EMP_NAME, SALARY
        FROM EMPLOYEE
        WHERE SALARY > 4000000;
      ```
      + '>= , =<'
      ```SQL
        -- EMPLOYEE테이블에서 급여가 4000000 이상인 직원의 이름, 급여 조회
        SELECT EMP_NAME, SALARY
        FROM EMPLOYEE
        WHERE SALARY >= 4000000;
      ```
      + '<> , != , ^='
      ```SQL
        -- EMPLOYEE테이블에서 부서코드가 D9이 아닌 사원의 사번, 이름, 부서코드 조회
        SELECT EMP_ID, EMP_NAME, DEPT_CODE
        FROM EMPLOYEE
        WHERE DEPT_CODE != 'D9';
        --WHERE DEPT_CODE ^= 'D9';
        --WHERE DEPT_CODE <> 'D9';
      ```
      + 'BETWEEN AND'
      ```SQL
        -- 급여를 3500000원 보다 많이 받고 6000000보다 적게 받는 사원의 이름, 급여 조회
        SELECT EMP_NAME, SALARY
        FROM EMPLOYEE
        WHERE SALARY BETWEEN 3500000 AND 6000000;
      ```
      + 'LIKE / NOT LIKE'
      ```SQL
        -- 비교하려는 값이 지정한 특정 패턴을 만족 시키는지 조회
        -- % : 0글자 이상
        -- _ : 1글자
        -- '글자%' : 글자로 시작하는 값
        -- '%글자%' : 글자가 포함된 값
        -- '%글자' : 글자로 끝나는 값 
        -- '_' : 한 글자
        -- '__' : 두 글자
        -- '박__' : 박으로 시작하는 세글자

        -- EMPLOYEE테이블에서 성이 전씨인 사원의 사번, 이름, 고용일 조회
        SELECT EMP_ID, EMP_NAME, HIRE_DATE
        FROM EMPLOYEE
        WHERE EMP_NAME LIKE '전%';
        --WHERE EMP_NAME LIKE '전__'; // 이름이 2글자 3글자가 넘어가면 적용되지 않음

        -- EMPLOYEE테이블에서 이름이 '하'가 포함된 직원의 이름, 주민번호, 부서코드 조회
        SELECT EMP_NAME, EMP_NO, DEPT_CODE
        FROM EMPLOYEE
        WHERE EMP_NAME LIKE '%하%';

        -- EMPLOYEE테이블에서 전화번호 4번째 자리가 9로 시작하는 사원의 사번, 이름, 전화번호 조회
        SELECT EMP_ID, EMP_NAME, PHONE
        FROM EMPLOYEE
        WHERE PHONE LIKE '___9%';

        -- 이메일 중 _ 앞글자가 3자리인 이메일 주소를 가진 사원의 사번, 이름, 이메일 주소 조회
        SELECT EMP_ID, EMP_NAME, EMAIL
        FROM EMPLOYEE
        WHERE EMAIL LIKE '____%';
        -- 와일드카드인 _가 검색하고자 하는 조건 안에 들어가는 문자와 같기 때문에
        -- 문자 자체가 아닌 와일드 카드로 인식

        -- ESCAFE OPTION
        SELECT EMP_ID, EMP_NAME, EMAIL
        FROM EMPLOYEE
        WHERE EMAIL LIKE '___\_%' ESCAPE '\';
      ```
      + 'IS NULL / IS NOT NULL'
      ```SQL
        -- EMPLOYEE테이블에서 보너스를 받지 않는 사원의 사번, 이름, 급여, 보너스 조회
        SELECT EMP_ID, EMP_NAME, SALARY, BONUS
        FROM EMPLOYEE
        WHERE BONUS IS NULL;

        -- EMPLOYEE테이블에서 보너스를 받는 사원의 사번, 이름, 급여, 보너스 조회
        SELECT EMP_ID, EMP_NAME, SALARY, BONUS
        FROM EMPLOYEE
        WHERE NOT BONUS IS NULL;

        -- EMPLOYEE테이블에서 관리자도 없고 부서배치도 받지 않은 직원의 이름, 관리자, 부서코드 조회
        SELECT EMP_NAME, MANAGER_ID, DEPT_CODE
        FROM EMPLOYEE
        WHERE MANAGER_ID IS NULL AND DEPT_CODE IS NULL;

        -- EMPLOYEE테이블에서 부서 배치를 받지 않았지만 보너스를 받는 직원의 이름, 보너스, 부서코드 조회
        SELECT EMP_NAME, BONUS, DEPT_CODE
        FROM EMPLOYEE
        WHERE DEPT_CODE IS NULL AND BONUS IS NOT NULL;
      ```
      + 'IN / NOT IN'
      ```SQL
        -- 비교하려는 값 목록에 일차하는 값이 있으면 TRUE를 반환하는 연산자
        -- EMPLOYEE테이블에서 부소커도가 D6이거나 D9인 사원의 이름, 부서코드, 급여 조회
        SELECT EMP_NAME, DEPT_CODE, SALARY
        FROM EMPLOYEE
        WHERE DEPT_CODE = 'D9' OR DEPT_CODE = 'D6';

        SELECT EMP_NAME, DEPT_CODE, SALARY
        FROM EMPLOYEE
        WHERE DEPT_CODE IN ('D6', 'D9');      
        
        -- 직급 코드가 J1, J2, J3, J4인 사람들의 이름, 직급 코드, 급여
        SELECT EMP_NAME, JOB_CODE, SALARY
        FROM EMPLOYEE
        WHERE JOB_CODE IN ('J1', 'J2', 'J3', 'J4');
      ```
    >
    
  + 논리 연산자
    + 여러 개의 제한 조건 결과를 하나의 논리 결과로 만들어줌
    >
    |연산자|설명|
    |:------:|:---:|
    |AND|여러 조건이 동시에 TRUE일 경우에만 TRUE값 반환|
    |OR|여러 조건들 중에 어느 하나의 조건만 TRUE이면 TRUE값 반환|
    |NOT|조건에 대한 반대 값으로 반환(NULL 제외)|
    + #### SQL EXAMPLE
    + 'AND'
    ```SQL
      -- EMPLOYEE테이블에서 부서코드가 'D6'이고 급여를 이백만보다 많은 직원의 이름, 부서코드, 급여 조회
      SELECT EMP_NAME, DEPT_CODE, SALARY
      FROM EMPLOYEE
      WHERE DEPT_CODE = 'D6' AND SALARY >2000000;
    ```
    + 'OR'
    ```SQL
      -- EMPLOYEE테이블에서 부서코드가 'D6' 이거나 급여를 이백만보다 많은 직원의 이름, 부서코드, 급여 조회
      SELECT EMP_NAME, DEPT_CODE, SALARY
      FROM EMPLOYEE
      WHERE DEPT_CODE = 'D6' OR SALARY >2000000;
    ```
    + 'NOT'
    ```SQL
      -- EMPLOYEE테이블에서 부서코드가 'D6' 이 아닌 직원의 이름, 부서코드, 급여 조회
      SELECT EMP_NAME, DEPT_CODE, SALARY
      FROM EMPLOYEE
      WHERE NOT DEPT_CODE = 'D6';
    ```
 

  

