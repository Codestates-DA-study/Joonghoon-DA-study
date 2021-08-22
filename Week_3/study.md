## 서브쿼리
- 쿼리 안에 쿼리
- 실행 순서는 하위 쿼리가 먼저 실행되고, 이후 그 결과가 상위 쿼리에 전달되고, 상위 쿼리가 실행되는 식
- rental_rate의 평균보다 큰 film_id, title, rental_rate 를 불러오는 서브쿼리문
~~~SQL
SELECT
	film_id,
	title,
	rental_rate
FROM film
WHERE rental_rate > (
	SELECT AVG (rental_rate)
	FROM film
	);
~~~
- WHERE절 안에 IN연산자 안으로 서브쿼리를 날릴 수도 있음
~~~SQL
SELECT film_id, title
FROM film
WHERE film_id IN (
SELECT i.film_id
FROM rental r
INNER JOIN inventory i ON i.inventory_id = r.inventory_id
WHERE return_date BETWEEN '2005-05-29' AND '2005-05-30'
);
~~~
- EXISTS 결과는 TRUE 또는 FALSE를 반환
- 즉 SELECT 뒤에 1, *, 'abc' 어떤 것을 설정하든 결과는 같음
~~~SQL
SELECT
	first_name,
	last_name
FROM
	customer
WHERE
	EXISTS (
		SELECT
			1
		FROM
			payment
		WHERE
			payment.customer_id = customer.customer_id
	);
~~~


## 집합연산자와 서브쿼리 = ANY연산
- ANY( ) 안에 만족하는 값이 하나만 있으면 됨
- 아래 쿼리는 카테고리 별 MAX(length)를 서브쿼리로 불러오고, 해당 조건에 아무거나 성립(ANY)하는 SELECT title FROM film 을 불러오는 것
~~~SQL
SELECT title
FROM film
WHERE length >= ANY(
    SELECT MAX(length)
    FROM film
    INNER JOIN film_category USING (film_id)
    GROUP BY  category_id );
~~~


## 집합연산자와 서브쿼리 = ALL연산
- ALL( ) 안에 모든 값을 만족해야 함
- 아래 쿼리는 rating으로 묶인 length 평균(소수점둘째자리까지)을 불러오고, 해당 조건을 모두 만족(ALL)하는 SELECT film_id, title, length FROM film 을 불러오는 것
~~~SQL
SELECT
    film_id,
    title,
    length
FROM
    film
WHERE
    length > ALL (
            SELECT
                ROUND(AVG (length),2)
            FROM
                film
            GROUP BY
                rating
    )
ORDER BY
    length;
~~~

## 집합연산자와 서브쿼리 = EXISTS연산
- 하위 쿼리에 행이 있는지 테스트하는 boolean operator
- 하위 쿼리가 NULL을 반환하면 EXIST는 true로 인식함
- 아래 쿼리는 11보다 큰 amount로 최소 1개의 rental 지불 항목이 있는 customers를 불러오는 것
~~~ SQL
SELECT first_name,
       last_name
FROM customer c
WHERE EXISTS
    (SELECT 1
     FROM payment p
     WHERE p.customer_id = c.customer_id
       AND amount > 11)
ORDER BY first_name,
         last_name;
~~~

## 조인과 집계데이터 = Grouping Set절
- 그룹핑에 대해서 개별적인 합계를 내어주고, 이를 UNION 해준 것과 같은 결과
- 특정 하나의 컬럼을 기준으로 묶어 Aggregation(sum avg count 등)함수를 사용할 수 있음
~~~SQL
--GROUPING( column_name | expression)
SELECT
	GROUPING(brand) grouping_brand,
	GROUPING(segment) grouping_segment,
	brand,
	segment,
	SUM (quantity)
FROM
	sales
GROUP BY
	GROUPING SETS (
		(brand), --brand 그룹핑
		(segment), --sengment 그룹핑
		() --NULL
	)
ORDER BY
	brand,
	segment;
~~~

## 조인과 집계데이터 = Rollup 절
- 그룹핑에 대해 소계를 내어줌
- 그룹핑 순서가 변경되면 subtotal 값 생성 기준도 변경됨
~~~SQL
--brand 별 소계 및 합계
--brand >> segment
SELECT
    brand,
    segment,
    SUM (quantity)
FROM
    sales
GROUP BY
    ROLLUP (brand, segment)
ORDER BY
    brand,
    segment;

--segment 별 소계 및 합계
--segment >> brand
SELECT
    segment,
    brand,
    SUM (quantity)
FROM
    sales
GROUP BY
    ROLLUP (segment, brand)
ORDER BY
    segment,
    brand;

--segment 별 소계 (합계X)
SELECT
    segment,
    brand,
    SUM (quantity)
FROM
    sales
GROUP BY
    segment,
    ROLLUP (brand)
ORDER BY
    segment,
    brand;

~~~

## 조인과 집계데이터 = Cube 절
- CUBE 는 그룹화된 모든 경우에 합을 구함
- 그룹핑 된 모든 칼럼마다 소계/합계를 구해주는 것
- ROLLUP 과 다르게 그룹핑 순서가 중요하지 않음
~~~SQL
SELECT
    brand,
    segment,
    SUM (quantity)
FROM
    sales
GROUP BY
    CUBE (brand, segment)
ORDER BY
    brand,
    segment;
~~~

## 조인과 집계데이터 = 분석 함수란
- 테이블에 있는 데이터를 특정 용도로 분석하여 결과를 반환하는 함수
- 복잡한 계산을 단순하게 처리해주는 함수
- 쿼리 결과 set을 대상으로 계산을 수행하는 함수
- SELECT 절에서 수행됨
- FROM, WHERE, GROUP BY 절에서 사용 불가
- ORDER BY 구문에서는 사용 가능
~~~SQL
분석함수(매개변수) OVER
    (PARTITION BY col1, col2,...  --분석함수로 계산될 대상 로우의 그룹(파티션)을 지정
     ORDER BY col3, col4,...  --파티션 안에서의 순서
     window 절)  --더 상세한 그룹으로 분할
~~~

### 조인과 집계데이터 = AVG함수
- Average

### 조인과 집계데이터 = Row Number , Rank , Dense_Rank 함수
- Row Number: 중복 관계없이 순차적으로 순위를 반환
- Rank: 동일한 값이면 중복 순위를 부여하고, 다음 순위는 해당 개수만큼 건너뛰고 반환
- Dense_Rank: 동일한 값이면 중복 순위를 부여하고, 다음 순위는 중복 순위와 상관없이 순차적으로 반환

### 조인과 집계데이터 = First_Value , Last_Value함수
~~~sql
FIRST_VALUE(Column_Name) OVER (ORDER BY Col1, Col2, ...)
~~~

### 조인과 집계데이터 = Lag, Lead 함수
- LAG: 이전 행의 값을 리턴
- LEAD: 다음 행의 값을 리턴
~~~sql
LAG(Column_Name, steps) OVER (ORDER BY Col1, Col2, ...) AS previous
LEAD(Column_Name, steps ) OVER (ORDER BY Col1, Col2, ...) AS next
~~~

## WINDOW FUNCTION 
- 행과 행간의 관계를 쉽게 정의하기 위해 만든 함수
- OVER 문구가 필수적으로 포함됨
- 크게 순위관련함수, 집계관련함수, 행순서관련함수, 비율관련함수, 통계분석관련함수 가 있음
~~~sql
SELECT WINDOW_FUCTION (ARGUMENTS) OVER ([PARTITION BY 칼럼명] [ORDER BY 절] [WINDOWING 절])
FROM table;
--PARTITION BY: 전체 집합을 기준의 의해 소그룹으로 나눔
--WINDOWING: 함수의 대상이 되는 행 기준의 범위를 지정할 수 있음
~~~

## PARTITION BY
- 그룹 내 순위 및 그룹 별 집계를 구할 때 사용
~~~SQL
SELECT 순위함수() 
OVER(PARTITION BY 컬럼명 ORDER BY 컬럼명) 
FROM 테이블명 

SELECT 집계함수(컬럼명) 
OVER (PARTITION BY 컬럼명) 
FROM 테이블명
~~~

## 조건연산자

### 산술연산자
- SELECT로 칼럼을 선택할 때, 칼럼의 값에 (+, -, *, /, %) 산술 연산자를 이용한 값을 반환할 수 있음
~~~sql
SELECT column1, column1 + 10
FROM table_name;
~~~

### 비교연산자
- =, <, >, <=, >=, <>(!=)
- IN, BETWEEN

### 논리연산자
- AND, OR , NOT

## CTE
- CTE(Common Table Expression)
- 비재귀적 CTE, 재귀적 CTE 가 있음
- SELECT문을 미리 정의해 이름을 붙인 후, 이어지는 쿼리에서 테이블처럼 사용하는 기능
- 바로 다음에 오는 SELECT절에만 해당 CTE를 이용할 수 있음
```sql
--일반적인 비재귀적 CTE 기본문법
WITH CTE_table (column_name) 
AS (
    CTE_SELECT 쿼리문
)
SELECT column_name
FROM CTE_table


--재귀적 CTE 예시
WITH CTE_table (column_name) 
AS(
    <쿼리문1: SELECT * FROM 테이블A> --Anchor Member
    
    UNION ALL
    
    <쿼리문2: SELECT * FROM 테이블B JOIN CTE_테이블이름> --Recursive Member
)
SELECT * FROM CTE_테이블이름;
```

### WITH
- 이름을 가진 서브쿼리를 먼저 정의한 후 메인쿼리를 사용하는 구문
- 쿼리의 전체적인 가독성을 높이고, 재사용할 수 있는 장점이 있음
- 대부분의 DBMS에서 지원
- 계층형 쿼리 구현 : WITH순환절, 오라클은 11g부터 계층형쿼리를 위한 CONNECT BY 절 지원
~~~sql
WITH [별명1] (컬럼명1, 컬럼명2) AS (
    SUB QUERY
)
MAIN QUERY
~~~

## SQL Transaction
- 원자성(Atomicity), 일관성(Consistency), 격리성(Isolation), 영속성(Durability)
- 트랜잭션에는 내가 적은 쿼리문과 데이터를 최종적으로 DB에 반영하는 COMMIT과 실패했을 때 COMMINT 시점으로 다시 되돌아가는 ROLLBACK이 있음
- COMMIT: DB에 영구 저장
- ROLLBACK: 변경사항을 취소
- SAVEPOINT: 임시저장, 특정 부분에서 트랜잭션을 취소
