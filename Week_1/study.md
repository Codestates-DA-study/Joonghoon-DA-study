# SQL Basic
## SELECT
~~~SQL
SELECT *
FROM table;
~~~

~~~SQL
SELECT field_1 AS one,
       field_2 AS two,
FROM table
~~~
## ORDER BY
~~~SQL
SELECT * 
FROM table
ORDER BY field ASC 혹은 DESC
~~~
- ASC: 오름차순 정렬 (default)
- DESC: 내림차순 정렬

## DISTINCT
~~~SQL
SELECT DISTINCT field
FROM table;
~~~
- 칼럼 내 고유값 조회

## COUNT
~~~SQL
SELECT COUNT(*)
FROM table;
~~~
~~~SQL
SELECT COUNT(field)
FROM table;
~~~
- non-missing value 카운트
<br>

~~~SQL
SELECT COUNT(DISTINCT field)
FROM table;
~~~
- unique value 카운트

## WHERE, Comparison operators
~~~SQL
SELECT *
FROM table
WHERE field = 1
~~~
- WHERE: condition filtering
- SELECT >> FROM >> WHERE 순으로 작성
- SQL 비교 연산자: =, <> or !=, >, <, >=, <=

## LIMIT, FETCH, OFFSET
~~~SQL
SELECT *
FROM table
LIMIT 10;
~~~

~~~SQL
SELECT *
FROM table
ORDER BY field
FETCH FIRST 10 ONLY;
~~~
- FETCH 절은 LIMIT 절과 기능적으로 동일함
<br>

~~~SQL
SELECT *
FROM table
OFFSET 10 ROWS;
~~~
- OFFSET: ~까지 조회하지 않음, 위의 경우 10번째 행까지는 조회하지 않고 11번째 행부터 조회

## Logical operators(LIKE, BETWEEN, IS NULL, IN, AND, OR, NOT)
~~~SQL
SELECT *
FROM table
WHERE field LIKE 'A%' 혹은 WHERE field ILIKE 'a%'
~~~
- 와일드카드 %
- 대소문자를 무시하는 ILIKE
- _ 를 사용해서 단일 문자도 대체 가능 ex. A_ple
<br>

~~~SQL
SELECT *
FROM table
WHERE field BETWEEN 5 AND 10 혹은 WHERE field >= 5 AND field <= 10
~~~

~~~SQL
SELECT *
FROM table
WHERE field IS NULL 혹은 WHERE field IS NOT NULL
~~~
- 칼럼 내 결측치 혹은 결측치가 아닌 값만 필터링해서 조회
<br>

~~~SQL
SELECT *
FROM table
WHERE field IN (1, 2, 3)
~~~
- 1,2,3 리스트 값 중에 일치하는게 있는 데이터만 조회

~~~SQL
AND
OR
NOT
~~~
