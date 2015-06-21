---
layout: post
title:  "[CUBRID] 날짜 데이터 다루기 "
date:   2015-06-21 18:00:00
categories: jekyll update
---

## CUBRID 날짜 데이터 다루기 
최근 날짜 데이터를 사용하는 쿼리를 자주 작성하면서 자주 사용하는 함수와 주의 할 점에 대해서 알아 보았다. 


## 날짜 데이터 타입의 종류 
- DATE : 일 단위 
- DATETIME: 밀리초 단위 
- TIMESTAMP: 초 단위
- TIME: 초 단위 

# TIMESTAMP 와 DATETIME 차이

- TIMESTAMP 데이터 타입은 날짜와 시간을 저장 할 수 있는 데이터 타입 이다. 
- TIMESTAMP 데이터 타입은 GMT 1970-01-01 00:00:01 ~ 2038-01-19 03:14:07 까지의 범위를 저장 할 수 있다. 
- DATETIME 데이터타입은 TIMESTAMP의 범위를 초과 하거나 밀리초 단위까지의 시간을 저장 해야 하는 경우 사용한다. 


## 날짜 변환 

# 날짜 문자열 → 날짜 데이터 타입

- TO_CHAR(date, formatter)
날짜 데이터 형 타입을 포맷 기준으로 문자열로 변경한다. 

{% highlight sql %} 
SELECT TO_CHAR(SYSDATETIME, 'yyyy-mm-dd')
-- Result: 2015-06-21 
{% endhighlight %}

# 날짜 데이터 타입 → 날짜 문자열

- TO_DATE(date, formatter)
날짜 문자열을 포맷 기준으로 Date 데이터 타입으로 변경한다.

{% highlight sql %} 
SELECT TO_DATE('2015-06-01',  'yyyy-mm-dd')
--  Result: 2015-06-01
{% endhighlight %}



- TO_DATETIME()
날자 문자열을 포맷 기준으로 DateTime 데이터 타입으로 변경한다. 

{% highlight sql %} 
SELECT TO_DATETIME('2015-06-01 15:30:10',  'yyyy-mm-dd hh24:mi:ss')
--  Result: - 2015-06-01 15:30:10.000
{% endhighlight %}




## 날짜 연산하기 
 
**년도 연산**

- ADDDATE( SYSDATETIME , INTERVAL ± YEAR)

{% highlight sql %} 
SELECT  SYSDATETIME,ADDDATE( SYSDATETIME , INTERVAL +3 YEAR)
-- Result: 2015-06-21 20:00:37.342 		2018-06-21 20:00:37.342
{% endhighlight %}


**월 연산**

- ADDDATE( SYSDATETIME , INTERVAL ± MONTH)

{% highlight sql %} 
SELECT  SYSDATETIME,ADDDATE( SYSDATETIME , INTERVAL +3 MONTH)
-- Result: 2015-06-21 20:00:37.342	2018-06-21 20:00:37.342
{% endhighlight %}


**일 연산**

- ADDDATE( SYSDATETIME , INTERVAL ± DAY)

{% highlight sql %} 
SELECT  SYSDATETIME,ADDDATE( SYSDATETIME , INTERVAL -3 DAY)
-- Result: 2015-06-21 20:01:45.638	2015-06-18 20:01:45.638
{% endhighlight %}


**시간 연산** 

- ADDDATE( SYSDATETIME , INTERVAL ± HOUR)

{% highlight sql %} 
SELECT  SYSDATETIME,ADDDATE( SYSDATETIME , INTERVAL -3 HOUR)
-- Result: 2015-06-21 20:02:14.367	2015-06-21 17:02:14.367
{% endhighlight %}


** DATETIME 데이터타입의 연산 **

- DATETIME 데이터 타입간에 - 연산을 할 수 있다. (뺄셈만 허용)
- 연산 결과는 BIGINT 데이터 타입의 정수 값으로 결과값이 나온다. 
- 결과값에서 1000ms = 1초 이므로, 시간으로 계산하려면 60 * 60 * 1000 으로 나누면 시간 값을 계산 할 수 있다. 

## 날짜 차이 구하기 
두 날짜 간의 일(day) 차이를 구할 때는 datediff를 이용하면 편리하다. 

{% highlight sql %} 
SELECT  SYSDATETIME as current, DATEDIFF(SYSDATETIME, TO_DATE('2015-06-20')) as diff
-- Result:  2015-06-21 20:19:17.459	   1
{% endhighlight %}

## 인덱스 제대로 이용하기

- 비교 연산시 
# 부정형 비교 연산은 인덱스를 타지 않는 것에 주의 한다. 
- INDEX가 =이나 <, > , BETWEEN 조건에서만 인덱스를 탄다. 


# 인덱스 컬럼은 형 변환을 하지 않는다. 

{% highlight sql %} 
-- (1) 
SELECT * FROM sample WHERE TO_DATE(inputDate, 'yyyymmdd') = '20150601'

-- (2) 
SELECT * FROM sample WHERE inputDate = TO_DATE('20150601')
{% endhighlight %}

date 데이터 타입의 inputDate가 인덱스로 걸려 있다면 (2)의 쿼리 처럼 비교 대상의 날짜 데이터를 DATE 타입으로 변형해야 인덱스를 사용하게 된다. 


# 인덱스와 비교대상의 데이터 형을 일치 한다. 

{% highlight sql %}

--(1)
SELECT * FROM sample 
WHERE inputDateTime = TO_DATE('20150601', 'yyyymmdd') 

--(2)
SELECT * FROM sample 
WHERE inputDateTime >= TO_DATE('20150601', 'yyyymmdd') 
  AND inputDateTime < TO_DATE('20150630', 'yyyymmdd') 

--(3)
SELECT * FROM sample 
WHERE inputDateTime >= TO_DATETIME('20150601', 'yyyymmdd') 
  AND inputDateTime < TO_DATETIME('20150630', 'yyyymmdd') 
{% endhighlight %}

inputDateTime의 데이터 타입이 DATETIME 일 때, 
(1) 처럼 비교 하면 20150601의 문자열이 DATE 데이터 타입으로 변환하고, 비교 대상이 DATETIME 형식이기 때문에 DATETIME 데이터 타입으로 변형한다.  


하지만, inputDateTime 과 비교하면서 2015-06-01 DATE가 DATETIME 데이터 타입으로 형환 된다면 
inputDateTime = 2015-06-01 00:00:00 와 비교하므로 2015-06-01 00:00:01 ~ 2015-06:01 23:59:59 데이터는 조회가 되지 않을 수 있음에 주의 해야한다. 


(2) 처럼 비교 대상의 날짜가 DATE 데이터 타입으로 비교 하면 DATETIME 데이터 타입으로 형 변환 한다. 
비교 하는 데이터 타입을 일치 해줘야 형 변환이 일어 나지 않고 인덱스를 제대로 탈 수 있다. 

> 참고 자료   
> http://www.iamcorean.net/159  
> http://www.cubrid.org/manual/91/ko/sql/function/arithmetic_op.html  
> http://www.cubrid.org/manual/91/ko/sql/function/datetime_fn.html  


