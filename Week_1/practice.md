## #1
문제1번) dvd 렌탈 업체의  dvd 대여가 있었던 날짜를 확인해주세요.
~~~SQL
SELECT DISTINCT DATE(rental_date) AS date
FROM rental
ORDER BY date;
~~~

문제2번) 영화길이가 120분 이상이면서, 대여기간이 4일 이상이 가능한, 영화제목을 알려주세요.
~~~SQL
SELECT title
FROM film
WHERE rental_duration >= 4 AND length >= 120;
~~~

문제3번) 직원의 id 가 2 번인  직원의  id, 이름, 성을 알려주세요
~~~SQL
SELECT employee_id, employee_name
FROM employees
WHERE employee_id = 2;
~~~

문제4번) 지불 내역 중에서,   지불 내역 번호가 17510 에 해당하는  ,  고객의 지출 내역 (amount ) 는 얼마인가요?
~~~SQL
SELECT amount
FROM payment
WHERE payment_id = 17510;
~~~

문제5번) 영화 카테고리 중에서 ,Sci-Fi  카테고리의  카테고리 번호는 몇번인가요?
~~~SQL
SELECT category_id
FROM category
WHERE name = 'Sci-Fi';
~~~

문제6번) film 테이블을 활용하여, rating  등급(?) 에 대해서, 몇개의 등급이 있는지 확인해보세요.
~~~SQL
SELECT COUNT(DISTINCT rating)
FROM film;
~~~

문제7번) 대여 기간이 (회수 - 대여일) 10일 이상이였던 rental 테이블에 대한 모든 정보를 알려주세요.
단 , 대여기간은  대여일자부터 대여기간으로 포함하여 계산합니다.
~~~SQL
SELECT *
FROM rental
WHERE DATE(return_date) - DATE(rental_date) + 1 >= 10;
~~~

문제8번) 고객의 id 가  50,100,150 ..등 50번의 배수에 해당하는 고객들에 대해서,
회원 가입 감사 이벤트를 진행하려고합니다.
고객 아이디가 50번 배수인 아이디와, 고객의 이름 (성, 이름)과 이메일에 대해서
확인해주세요.
~~~SQL
SELECT first_name, last_name, email
FROM customer
WHERE customer_id % 50 = 0;
~~~

문제9번) 영화 제목의 길이가 8글자인, 영화 제목 리스트를 나열해주세요.
~~~SQL
SELECT title
FROM film
WHERE LENGTH(title) = 8;
~~~

문제10번)	city 테이블의 city 갯수는 몇개인가요?
~~~SQL
SELECT COUNT (DISTINCT city)
FROM city;
~~~

문제11번)	영화배우의 이름 (이름+' '+성) 에 대해서,  대문자로 이름을 보여주세요.  단 고객의 이름이 동일한 사람이 있다면,  중복 제거하고, 알려주세요.
~~~SQL
SELECT DISTINCT CONCAT(UPPER(first_name), ' ', UPPER(last_name))
FROM actor;
~~~

문제12번)	고객 중에서,  active 상태가 0 인 즉 현재 사용하지 않고 있는 고객의 수를 알려주세요.
~~~SQL
SELECT COUNT(*)
FROM customer
WHERE active = 0;
~~~

문제13번)	Customer 테이블을 활용하여,  store_id = 1 에 매핑된  고객의 수는 몇명인지 확인해보세요.
~~~SQL
SELECT COUNT(*)
FROM customer
WHERE store_id = 1;
~~~

문제14번)	rental 테이블을 활용하여,  고객이 return 했던 날짜가 2005년6월20일에 해당했던 rental 의 갯수가 몇개였는지 확인해보세요.
~~~SQL
SELECT COUNT(*)
FROM rental
WHERE DATE(return_date) = DATE('2005-06-20');
~~~

문제15번)	film 테이블을 활용하여, 2006년에 출시가 되고 rating 이 'G' 등급에 해당하며, 대여기간이 3일에 해당하는  것에 대한 film 테이블의 모든 컬럼을 알려주세요.
~~~SQL
SELECT *
FROM film
WHERE release_year = 2006
AND rating = 'G'
AND rental_duration = 3;
~~~

문제16번)	langugage 테이블에 있는 id, name 컬럼을 확인해보세요 .
~~~SQL
SELECT language_id, name
FROM language;
~~~

문제17번)	film 테이블을 활용하여,  rental_duration 이  7일 이상 대여가 가능한  film 에 대해서  film_id,   title,  description 컬럼을 확인해보세요.
~~~SQL
SELECT film_id, title, description
FROM film
WHERE rental_duration >= 7;
~~~

문제18번)	film 테이블을 활용하여,  rental_duration   대여가 가능한 일자가 3일 또는 5일에 해당하는  film_id,  title, desciption 을 확인해주세요.
~~~SQL
SELECT film_id, title, description
FROM film
WHERE rental_duration  = 3
OR rental_duration = 5;
~~~

문제19번)	Actor 테이블을 이용하여,  이름이 Nick 이거나  성이 Hunt 인  배우의  id 와  이름, 성을 확인해주세요.
~~~SQL
SELECT actor_id, first_name, last_name
FROM actor
WHERE first_name = 'Nick'
OR last_name = 'Hunt';
~~~

문제20번)	Actor 테이블을 이용하여, Actor 테이블의  first_name 컬럼과 last_name 컬럼을 , firstname, lastname 으로 컬럼명을 바꿔서 보여주세요
~~~SQL
SELECT  actor_id,
		first_name AS firstname,
		last_name AS lastname,
		last_update
FROM actor;
~~~

<br>

## #2
문제1번) film 테이블을 활용하여,  film 테이블의  100개의 row 만 확인해보세요.
~~~SQL
SELECT *
FROM film
LIMIT 100;
~~~

문제2번) actor 의 성(last_name) 이  Jo 로 시작하는 사람의 id 값이 가장 낮은 사람 한사람에 대하여, 사람의  id 값과  이름, 성 을 알려주세요.
~~~SQL
SELECT actor_id, first_name, last_name
FROM actor
WHERE last_name LIKE 'Jo%'
ORDER BY actor_id ASC
LIMIT 1;
~~~

문제3번)film 테이블을 이용하여, film 테이블의 아이디값이 1~10 사이에 있는 모든 컬럼을 확인해주세요.
~~~SQL
SELECT *
FROM film
ORDER BY film_id ASC
LIMIT 10;
~~~

문제4번) country 테이블을 이용하여, country 이름이 A 로 시작하는 country 를 확인해주세요.
~~~SQL
SELECT DISTINCT country
FROM country
WHERE country LIKE 'A%'
ORDER BY country;
~~~

문제5번) country 테이블을 이용하여, country 이름이 s 로 끝나는 country 를 확인해주세요.
~~~SQL
SELECT DISTINCT country
FROM country
WHERE country LIKE '%s'
ORDER BY country;
~~~

문제6번) address 테이블을 이용하여, 우편번호(postal_code) 값이 77로 시작하는  주소에 대하여, address_id, address, district ,postal_code  컬럼을 확인해주세요.
~~~SQL
SELECT address_id, address, district, postal_code
FROM address
WHERE postal_code LIKE '77%';
~~~

문제7번) address 테이블을 이용하여, 우편번호(postal_code) 값이  두번째글자가 1인 우편번호의  address_id, address, district ,postal_code  컬럼을 확인해주세요.
~~~SQL
SELECT address_id, address, district, postal_code
FROM address
WHERE postal_code LIKE '_1%';
~~~

문제8번) payment 테이블을 이용하여,  고객번호가 341에 해당 하는 사람이 결제를 2007년 2월 15~16일 사이에 한 모든 결제내역을 확인해주세요.
~~~SQL
SELECT *
FROM payment
WHERE customer_id = 341
AND DATE(payment_date) >= DATE('2007-2-15')
AND DATE(payment_date) <= DATE('2007-2-16');
~~~

문제9번) payment 테이블을 이용하여, 고객번호가 355에 해당 하는 사람의 결제 금액이 1~3원 사이에 해당하는 모든 결제 내역을 확인해주세요.
~~~SQL
SELECT *
FROM payment
WHERE customer_id = 335
AND amount >= 1
AND amount <= 3;

SELECT *
FROM payment
WHERE customer_id = 335
AND amount BETWEEN 1 AND 3;
~~~

문제10번) customer 테이블을 이용하여, 고객의 이름이 Maria, Lisa, Mike 에 해당하는 사람의 id, 이름, 성을 확인해주세요.
~~~SQL
SELECT customer_id, first_name, last_name
FROM customer
WHERE first_name in ('Maria', 'Lisa', 'Mike');
~~~

문제11번) film 테이블을 이용하여,  film의 길이가  100-120 에 해당하거나 또는 rental 대여기간이 3-5일에 해당하는 film 의 모든 정보를 확인해주세요.
~~~SQL
SELECT *
FROM film
WHERE length BETWEEN 100 AND 120
OR rental_duration BETWEEN 3 AND 5
ORDER BY film_id;
~~~

문제12번) address 테이블을 이용하여, postal_code 값이  공백('') 이거나 35200, 17886 에 해당하는 address 에 모든 정보를 확인해주세요.
~~~SQL
SELECT *
FROM address
WHERE postal_code IN ('', '35200', '17886');
~~~

문제13번) address 테이블을 이용하여,  address 의 상세주소(=address2) 값이  존재하지 않는 모든 데이터를 확인하여 주세요.
~~~SQL
SELECT *
FROM address
WHERE address2 IS NULL
OR address2 = '';
~~~

문제14번) staff 테이블을 이용하여, staff 의  picture  사진의 값이 있는  직원의  id, 이름,성을 확인해주세요.  단 이름과 성을  하나의 컬럼으로 이름, 성의형태로  새로운 컬럼 name 컬럼으로 도출해주세요.
~~~SQL
SELECT staff_id, CONCAT(first_name, ' ', last_name) AS name
FROM staff
WHERE picture IS NULL;
~~~

문제15번) rental 테이블을 이용하여,  대여는했으나 아직 반납 기록이 없는 대여건의 모든 정보를 확인해주세요.
~~~SQL
SELECT * 
FROM rental
WHERE rental_date is NOT NULL
AND return_date IS NULL;
~~~

문제16번) address 테이블을 이용하여, postal_code 값이  빈 값(NULL) 이거나 35200, 17886 에 해당하는 address 에 모든 정보를 확인해주세요.
~~~SQL
SELECT *
FROM address
WHERE postal_code IS NULL
OR postal_code IN ('35200', '17886');
~~~

문제17번) 고객의 성에 John 이라는 단어가 들어가는, 고객의 이름과 성을 모두 찾아주세요.
~~~SQL
SELECT first_name, last_name
from customer
WHERE last_name LIKE '%John%';
~~~

문제18번) 주소 테이블에서, address2 값이 null 값인 row 전체를 확인해볼까요?
~~~SQL
SELECT *
FROM address
WHERE address2 IS NULL;
~~~
