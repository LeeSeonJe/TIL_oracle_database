# DML
+
  + 데이터 조작 언어로 테이블에 값을 **삽입**(**INSERT**), **수정**(**UPDATE**), **삭제**(**DELETE**)하는 구문을 말함
  
### INSERT
+ 
  + 테이블에 새로운 행을 추가하여 테이블의 행 개수를 증가시키는 구문

### INSERT ALL
+
  + INSERT시 서브쿼리가 사용하는 테이블 같은 경우, 두 개 이상의 테이블에 INSERT ALL을 이용하여 한 번에 삽입 가능
  + 단. 각 서브쿼리의 조건절이 같아야 함
### UPDATE
+
  + 테이블에 기록된 **컬럼 값**을 수정하는 구문으로 테이블 전체 행 개수는 변화 없음
### DELETE
+
  + 테이블의 행을 삭제하는 구문으로 테이블의 행 개수가 줄어듦
  
### TRUNCATE
+
  + 테이블 전체 행 삭제 시 사용하여 DELETE보다 수행 속도가 빠르나 ROLLBACK을 통해 복구 불가능