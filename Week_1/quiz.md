![image](https://user-images.githubusercontent.com/74339882/128601768-3e293340-5b44-4308-a0e2-76ed0155be17.png)

문제1번)  각 제품 가격을 5 % 줄이면 어떻게 될까요?

```sql
--현재가의 95%를 확인하란건가?

SELECT *, retailprice * 0.95 AS "95% of retailprice"
FROM products;
```

문제2번)  고객이 주문한 목록을 주문 일자로 내림차순 정렬해서 보여주세요.

```sql
SELECT *
FROM orders
ORDER BY DATE(orderdate) DESC;
```

문제3번)  employees 테이블을 이용하여, 705 아이디를 가진 직원의 , 이름, 성과  해당 직원의  태어난 해를 확인해주세요.

```sql
SELECT empfirstname, emplastname, empbirthdate
FROM employees
WHERE employeeid = 705;
```

문제4번)  customers 테이블을 이용하여,  고객의 이름과 성을 하나의 컬럼으로 전체 이름을 보여주세요 (이름, 성 의 형태로  full_name 이라는 이름으로 보여주세요)

```sql
SELECT CONCAT(custfirstname, ' ', custlastname) AS full_name
FROM customers;
```

문제5번) orders 테이블을 활용하여, 고객번호가 1001 에 해당하는 사람이 employeeid 가 707인 직원으로부터  산 주문의 id 와 주문 날짜를 알려주세요.
* 주문일자 빠른순으로 정렬하여, 보여주세요.

```sql
SELECT ordernumber, orderdate
FROM orders
WHERE employeeid = 707;
```

문제6번)  vendors 테이블을 이용하여, 벤더가 위치한 state 주가 어떻게 되는지, 확인해보세요.  중복된 주가 있다면, 중복제거 후에 알려주세요.

```sql
SELECT DISTINCT vendstate
FROM vendors;
```

문제7번) 주문일자가  2017-09-02~ 09-03일 사이에 해당하는 주문 번호를 알려주세요.

```sql
SELECT *
FROM orders
WHERE orderdate BETWEEN DATE('2017-09-02') AND DATE('2017-09-03');
```

문제8번) products 테이블을 활용하여, productdescription에 상품 상세 설명 값이 없는  상품 데이터를 모두 알려주세요.

```sql
-- 결측치가 null이 아닌 empty로 되어 있을 수 있음
-- 그래서 IS NULL과 함께 = '' 조건으로 필터링을 같이 걸어줌
-- 근데 전체 데이터에 productdescription은 없는 것으로 확인

SELECT *
FROM products
WHERE productdescription IS NULL
OR productdescription = '';
```

문제9 번) vendors 테이블을 이용하여, vendor의 State 지역이 NY 또는 WA 인 업체의 이름을 알려주세요.

```sql
SELECT vendname
FROM vendors
WHERE vendstate IN ('NY', 'WA')
```

문제10번)  customers 테이블을 이용하여, 고객의 id 별로,  custstate 지역 중 WA 지역에 사는 사람과  WA 가 아닌 지역에 사는 사람을 구분해서  보여주세요.

- customerid 와, newstate_flag 컬럼으로 구성해주세요 .
- newstate_flag 컬럼은 WA 와 OTHERS 로 노출해주시면 됩니다.

```sql
SELECT customerid,
CASE
  WHEN custstate = 'WA' THEN 'WA' 
  WHEN custstate != 'WA' THEN 'OTHERS' 
END AS newstate_flag
FROM customers;
```
