## #3
문제1번) 고객의 기본 정보인, 고객 id, 이름, 성, 이메일과 함께 고객의 주소 address, district, postal_code, phone 번호를 함께 보여주세요.
~~~SQL
SELECT c.customer_id, c.first_name, c.last_name, c.email, a.address, a.district, a.postal_code, a.phone
FROM customer AS c
INNER JOIN address AS a ON c.address_id = a.address_id;
~~~
<br>

문제2번) 고객의  기본 정보인, 고객 id, 이름, 성, 이메일과 함께 고객의 주소 address, district, postal_code, phone , city 를 함께 알려주세요.
~~~SQL
SELECT c.customer_id, c.first_name, c.last_name, c.email, a.address, a.district, a.postal_code, a.phone, city.city
FROM customer AS c
INNER JOIN address AS a ON c.address_id = a.address_id
INNER JOIN city ON a.city_id = city.city_id;
--INNER JOIN을 사용했는데, 여기서 LEFT OUTER JOIN을 사용하는 것과 다른게 없는 것 같음
~~~
<br>

문제3번) Lima City에 사는 고객의 이름과, 성, 이메일, phonenumber에 대해서 알려주세요.
~~~SQL
SELECT c.first_name, c.last_name, c.email
FROM customer AS c
INNER JOIN address AS a ON c.address_id = a.address_id
INNER JOIN city ON a.city_id = city.city_id
WHERE city.city = 'Lima';
~~~
<br>

문제4번) rental 정보에 추가로, 고객의 이름과, 직원의 이름을 함께 보여주세요.
- 고객의 이름, 직원 이름은 이름과 성을 fullname 컬럼으로 만들어서 직원이름/고객이름 2개의 컬럼으로 확인해주세요.
~~~SQL
SELECT rental.*, CONCAT(customer.first_name, ' ', customer.last_name) AS customer_fullname, CONCAT(staff.first_name, ' ', staff.last_name) AS staff_fullname
FROM rental
INNER JOIN customer ON rental.customer_id = customer.customer_id
INNER JOIN staff ON rental.staff_id = staff.staff_id
ORDER BY rental_id;
~~~
<br>

문제5번) [seth.hannon@sakilacustomer.org](mailto:seth.hannon@sakilacustomer.org) 이메일 주소를 가진 고객의  주소 address, address2, postal_code, phone, city 주소를 알려주세요.
~~~SQL
SELECT a.address, a.address2, a.postal_code, a.phone, city.city
FROM address a
INNER JOIN city ON a.city_id = city.city_id
INNER JOIN customer c ON a.address_id = c.address_id
WHERE c.email = 'seth.hannon@sakilacustomer.org';
~~~
<br>

문제6번) Jon Stephens 직원을 통해 dvd대여를 한 payment 기록 정보를  확인하려고 합니다.
- payment_id,  고객 이름 과 성,  rental_id, amount, staff 이름과 성을 알려주세요.
~~~SQL
SELECT p.payment_id, c.first_name, c.last_name, p.rental_id, p.amount, s.first_name, s.last_name
FROM payment p
INNER JOIN customer c ON p.customer_id = c.customer_id
INNER JOIN staff s ON p.staff_id = s.staff_id
WHERE s.first_name = 'Jon'
AND s.last_name = 'Stephens';
~~~
<br>

문제7번) 배우가 출연하지 않는 영화의 film_id, title, release_year, rental_rate, length 를 알려주세요.
~~~SQL
SELECT f.film_id, f.title, f.release_year, f.rental_rate, f.length
FROM film f
LEFT OUTER JOIN film_actor fa ON f.film_id = fa.film_id
WHERE fa.actor_id IS NULL;
-- "배우가 출연하지 않는" film이 있으므로 INNER JOIN하면 결측치 부분은 제외되고 조회됨
-- 그러므로 film_actor.actor_id가 결측치인 정보가 조회되지 않음
-- outer join을 사용해서 모든 값을 조회 후 WHERE 조건절을 추가해야 함
~~~
<br>

문제8번) store 상점 id별 주소 (address, address2, distict) 와 해당 상점이 위치한 city 주소를 알려주세요.
~~~SQL
-- 내 풀이
-- store_id가 결측치인 값들이 모두 조회됨 >> 잘못된 방법
SELECT s.store_id, a.address, a.address2, a.district, c.city
FROM address a
LEFT OUTER JOIN store s ON a.address_id = s.address_id
LEFT OUTER JOIN city c ON a.city_id = c.city_id;
~~~
~~~SQL
-- 답안풀이
-- store 테이블에 SELECT문을 날려서 store_id가 결측치인 값들은 조회되지 않음
SELECT s.store_id, a.address, a.address2, a.district, c.city
FROM store s
LEFT OUTER JOIN address a ON s.address_id = a.address_id
LEFT OUTER JOIN city c ON a.city_id = c.city_id;
~~~
<br>

문제9번) 고객의 id 별로 고객의 이름 (first_name, last_name), 이메일, 고객의 주소 (address, district), phone번호, city, country 를 알려주세요.
~~~SQL
SELECT DISTINCT c.first_name, c.last_name, c.email, a.address, a.district, country.country
FROM customer c
LEFT OUTER JOIN address a ON c.address_id = a.address_id
LEFT OUTER JOIN city ON a.city_id = city.city_id
LEFT OUTER JOIN country ON city.country_id = country.country_id;
~~~
<br>

문제10번) country 가 china 가 아닌 지역에 사는, 고객의 이름(first_name, last_name)과 , email, phonenumber, country, city 를 알려주세요
~~~SQL
SELECT DISTINCT c.first_name, c.last_name, c.email, a.address, a.district, country.country
FROM customer c
LEFT OUTER JOIN address a ON c.address_id = a.address_id
LEFT OUTER JOIN city ON a.city_id = city.city_id
LEFT OUTER JOIN country ON city.country_id = country.country_id
WHERE country.country != 'China';
~~~
<br>

문제11번) Horror 카테고리 장르에 해당하는 영화의 이름과 description 에 대해서 알려주세요
~~~SQL
SELECT f.title, f.description
FROM film f
LEFT OUTER JOIN film_category fc ON f.film_id = fc.film_id
LEFT OUTER JOIN category c ON fc.category_id = c.category_id
WHERE c.name = 'Horror';
~~~
<br>

문제12번) Music 장르이면서, 영화길이가 60~180분 사이에 해당하는 영화의 title, description, length 를 알려주세요.
- 영화 길이가 짧은 순으로 정렬해서 알려주세요.
~~~SQL
SELECT f.title, f.description, f.length
FROM film f
LEFT OUTER JOIN film_category fc ON f.film_id = fc.film_id
LEFT OUTER JOIN category c ON fc.category_id = c.category_id
WHERE c.name = 'Music'
AND f.length >= 60 AND f.length <= 180
ORDER BY f.length;
~~~
<br>

문제13번) actor 테이블을 이용하여,  배우의 ID, 이름, 성 컬럼에 추가로    'Angels Life' 영화에 나온 영화 배우 여부를 Y , N 으로 컬럼을 추가 표기해주세요.  해당 컬럼은 angelslife_flag로 만들어주세요.
~~~SQL
SELECT a.actor_id, a.first_name, a.last_name, CASE WHEN a.actor_id IN (
	SELECT fa.actor_id
	FROM film_actor fa
	INNER JOIN film f ON fa.film_id = f.film_id
	WHERE f.title = 'Angels Life') THEN 'Y'
	ELSE 'N'
	END AS angelslife_flag
FROM actor a;
-- 헷갈린다!!! 복잡해복잡해 복복잡잡해
~~~
<br>

문제14번) 대여일자가 2005-06-01~ 14일에 해당하는 주문 중에서 , 직원의 이름(이름 성) = 'Mike Hillyer' 이거나  고객의 이름이 (이름 성) ='Gloria Cook'  에 해당 하는 rental 의 모든 정보를 알려주세요.

- 추가로 직원이름과, 고객이름에 대해서도 fullname 으로 구성해서 알려주세요.
~~~SQL
SELECT r.*, 
	CONCAT(s.first_name, ' ', s.last_name) AS staff_name,
	CONCAT(c.first_name, ' ', c.last_name) AS customer_name
FROM rental r
LEFT OUTER JOIN staff s ON r.staff_id = s.staff_id
LEFT OUTER JOIN customer c ON r.customer_id = c.customer_id
WHERE DATE(rental_date) BETWEEN '2005-06-01' AND '2005-06-14'
AND (s.first_name || ' ' || s.last_name = 'Mike Hillyer'
	OR c.first_name || ' ' || c.last_name = 'Gloria Cook')
~~~
<br>

문제15번) 대여일자가 2005-06-01~ 14일에 해당하는 주문 중에서 , 직원의 이름(이름 성) = 'Mike Hillyer' 에 해당 하는 직원에게  구매하지 않은  rental 의 모든 정보를 알려주세요.

- 추가로 직원이름과, 고객이름에 대해서도 fullname 으로 구성해서 알려주세요.
~~~SQL
SELECT r.*, 
	CONCAT(s.first_name, ' ', s.last_name) AS staff_name,
	CONCAT(c.first_name, ' ', c.last_name) AS customer_name
FROM rental r
LEFT OUTER JOIN staff s ON r.staff_id = s.staff_id
LEFT OUTER JOIN customer c ON r.customer_id = c.customer_id
WHERE DATE(rental_date) BETWEEN '2005-06-01' AND '2005-06-14'
AND s.first_name || ' ' || s.last_name != 'Mike Hillyer'
~~~
<br>

## #4
문제1번) store 별로 staff는 몇명이 있는지 확인해주세요.

문제2번) 영화등급(rating) 별로 몇개 영화film을 가지고 있는지 확인해주세요.

문제3번) 출현한 영화배우(actor)가  10명 초과한 영화명은 무엇인가요?

문제4번) 영화 배우(actor)들이 출연한 영화는 각각 몇 편인가요?

- 영화 배우의 이름 , 성 과 함께 출연 영화 수를 알려주세요.

문제5번) 국가(country)별 고객(customer) 는 몇명인가요?

문제6번) 영화 재고 (inventory) 수량이 3개 이상인 영화(film) 는?

- store는 상관 없이 확인해주세요.

문제7번) dvd 대여를 제일 많이한 고객 이름은?

문제8번) rental 테이블을  기준으로,   2005년 5월26일에 대여를 기록한 고객 중, 하루에 2번 이상 대여를 한 고객의 ID 값을 확인해주세요.

문제9번) film_actor 테이블을 기준으로, 출현한 영화의 수가 많은  5명의 actor_id 와 , 출현한 영화 수 를 알려주세요.

문제10번) payment 테이블을 기준으로,  결제일자가 2007년2월15일에 해당 하는 주문 중에서  ,  하루에 2건 이상 주문한 고객의  총 결제 금액이 10달러 이상인 고객에 대해서 알려주세요.
(고객의 id,  주문건수 , 총 결제 금액까지 알려주세요)

문제11번) 사용되는 언어별 영화 수는?

문제12번) 40편 이상 출연한 영화 배우(actor) 는 누구인가요?

문제13번) 고객 등급별 고객 수를 구하세요. (대여 금액 혹은 매출액  에 따라 고객 등급을 나누고 조건은 아래와 같습니다.)
/*
A 등급은 151 이상
B 등급은 101 이상 150 이하
C 등급은   51 이상 100 이하
D 등급은   50 이하

- 대여 금액의 소수점은 반올림 하세요.

HINT
반올림 하는 함수는 ROUND 입니다.	
*/

## #5
문제1번) 영화 배우가,  영화 180분 이상의 길이 의 영화에 출연하거나, 영화의 rating 이 R 인 등급에 해당하는 영화에 출연한  영화 배우에 대해서,  영화 배우 ID 와 (180분이상 / R등급영화)에 대한 Flag 컬럼을 알려주세요.

- film_actor 테이블와 film 테이블을 이용하세요.
- union, unionall, intersect, except 중 상황에 맞게 사용해주세요.
- actor_id 가 동일한 flag 값 이 여러개 나오지 않도록 해주세요.

문제2번) R등급의 영화에 출연했던 배우이면서, 동시에, Alone Trip의 영화에 출연한  영화배우의 ID 를 확인해주세요.

- film_actor 테이블와 film 테이블을 이용하세요.
- union, unionall, intersect, except 중 상황에 맞게 사용해주세요.

문제3번) G 등급에 해당하는 필름을 찍었으나,   영화를 20편이상 찍지 않은 영화배우의 ID 를 확인해주세요.

- film_actor 테이블와 film 테이블을 이용하세요.
- union, unionall, intersect, except 중 상황에 맞게 사용해주세요.

문제4번) 필름 중에서,  필름 카테고리가 Action, Animation, Horror 에 해당하지 않는 필름 아이디를 알려주세요.

- category 테이블을 이용해서 알려주세요.

문제5번) Staff  의  id , 이름, 성 에 대한 데이터와 , Customer 의 id, 이름 , 성에 대한 데이터를  하나의  데이터셋의 형태로 보여주세요.

- 컬럼 구성 : id, 이름 , 성, flag (직원/고객여부) 로 구성해주세요.

문제6번) 직원과  고객의 이름이 동일한 사람이 혹시 있나요? 있다면, 해당 사람의 이름과 성을 알려주세요.

문제7번) 반납이 되지 않은 대여점(store)별 영화 재고 (inventory)와 전체 영화 재고를 같이 구하세요. (union all)

문제8번) 국가(country)별 도시(city)별 매출액, 국가(country)매출액 소계 그리고 전체 매출액을 구하세요. (union all)
