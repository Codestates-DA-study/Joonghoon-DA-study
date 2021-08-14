# SQL Joins 
### 2개 이상의 테이블을 1개의 테이블로 생각하고 join하여 쿼리를 날리는 것
<br>

## Inner join
![image](https://user-images.githubusercontent.com/74339882/129435437-9f24c455-a47a-4040-a8a6-42dacdf4c08d.png)
~~~SQL
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name = table2.column_name;
~~~
<br>

## Outer join
### Left Outer join
![image](https://user-images.githubusercontent.com/74339882/129435541-b3120d72-bdaf-46c3-94e7-075ee8fdd03f.png)
~~~SQL
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name = table2.column_name;
~~~
<br>

### Right Outer join
![image](https://user-images.githubusercontent.com/74339882/129435546-1f9c67e1-72a4-4771-a09a-1bf558197e47.png)
~~~SQL
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name = table2.column_name;
~~~~
<br>

### Full Outer join (== Full join)
![image](https://user-images.githubusercontent.com/74339882/129435610-82f8037b-c6f0-4b4c-b3d9-3b3473faafeb.png)
~~~SQL
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name = table2.column_name
WHERE condition;
~~~
<br>

## Self join
- 하나의 테이블을 여러번 복사해서 join (자기 자신 테이블과 join)
- from 뒤에 테이블이 두 개가 온다는 것이 self join 의 특징
- 하나의 테이블 이름을 다르게 지정해줘야 함
~~~SQL
SELECT column_name(s)
FROM table1 T1, table1 T2
WHERE condition;
~~~
<br>

## Cross join
![image](https://user-images.githubusercontent.com/74339882/129435952-92ec14b0-0ea0-4ed0-a2a1-855c7af2c12d.png)<br>
- 모든 경우의 수를 표현할 때 사용
- A, B 테이블이 있을 때 A테이블의 한 행을 B테이블의 모든 행과 비교할 때
- 결과값이 엄청 많아짐 (n * m)
~~~SQL
--방법1--
SELECT A.name, B.age
FROM EX_TABLE A
CROSS JOIN JOIN_TABLE B
~~~
~~~SQL
--방법2--
SELECT
A.name, --A테이블의 NAME조회
B.age --B테이블의 AGE조회
FROM EX_TABLE A, JOIN_TABLE B
~~~
<br>

## Natural join
- 두 테이블 사이 동일한 이름의 칼럼이 모두 join
- USING 절을 사용해서 특정 칼럼을 설정해서 사용 가능
- 사용할 일 없을 것 같은데..?
~~~SQL
SELECT name, age
FROM EX_TABLE A
NATURAL JOIN JOIN_TABLE B
USING (column);
~~~
<br>

# Group by
- 국가, 성별 같이 같은 값을 가진 카테고리별로 묶어서 조회
- aggregate functions ( COUNT(), MAX(), MIN(), SUM(), AVG() )
~~~SQL
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
ORDER BY column_name(s);
~~~
~~~SQL
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
ORDER BY COUNT(CustomerID) DESC;
~~~
<br>

# Having
- WHERE 절이 aggregate function과 같이 사용될 수 없어서 추가됨
- GROUP BY 하위에 조건을 걸 때 사용
~~~SQL
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5
ORDER BY COUNT(CustomerID) DESC;
~~~
<br>

# 집합연산자와 서브쿼리
![image](https://user-images.githubusercontent.com/74339882/129438309-43d3eace-cd0a-42a2-882e-ff408f93609c.png)

## UNION
- 중복을 제거한 결과의 합
- 두 개 이상의 SELECT문과 결과값을 결합하는데 사용
- JOIN과 유사하지만 SELECT 문으로 만들어진 field가 동일한 데이터 유형에 사용되어야 함
- 중복데이터는 출력되지 않음
- UNION 내 SELECT문의 결과값은 같은 수의 열을 가지고 동일한 순서로 있어야 함
- 일바적으로 첫번째 selectㅁ
~~~SQL
SELECT column_name(s) FROM table1

UNION

SELECT column_name(s) FROM table2;
~~~
~~~SQL
SELECT City, Country FROM Customers
WHERE Country='Germany'

UNION

SELECT City, Country FROM Suppliers
WHERE Country='Germany'
ORDER BY City;
~~~
<br>

## UNION ALL
- 중복을 포함한 결과의 합
- UNION 은 중복값을 출력하지 않기 때문에 중복값까지 모두 출력할 때 사용
~~~SQL
SELECT column_name(s) FROM table1

UNION ALL

SELECT column_name(s) FROM table2;
~~~
<br>

## INTERSECT
- 양쪽 모두에서 포함된 행
- INTERSECT는 두 SELECT문을 결합하는 데 사용되지만 두 번째 SELECT문의 행과 동일한 첫 번째 SELECT 문의 행만 반환
- 두 개의 SELECT 문에서 반환된 공통 행만 반환
~~~SQL
SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition]

INTERSECT

SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition]
~~~
<br>

## EXCEPT
- MINUS와 동일?
- 첫 번째 검색 결과에서 두 번째 검색 결과를 제외한 나머지
- 보통 EXCEPT 집합연산자를 이용한 방법보다 두 개의 조건을 이용한 SELECT문이 훨씬 단순하고 가독성이 좋으며 성능 또한 우수
- 경우에 따라 EXCEPT 연산자를 이용하면 성능이 훨씬 좋을 때가 있음 (ex. 조건에 부정연산이 들어간 경우)
~~~SQL
SELECT employee_id, last_name, job_id
FROM employees
WHERE last_name LIKE 'K%'
AND job_id != 'SA_REP';
~~~
~~~SQL
SELECT employee_id, last_name, job_id
FROM employees
WHERE last_name LIKE 'K%'

EXCEPT

SELECT employee_id, last_name, job_id
FROM employees
WHERE job_id = 'SA_REP';
~~~
