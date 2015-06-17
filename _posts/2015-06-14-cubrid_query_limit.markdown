---
layout: post
title:  "[CUBRID] rounum limit절"
date:   2015-06-14 18:00:00
categories: jekyll update
---

일정 기간이 지난 데이터를 삭제하는 작업을 하게되었는데, 데이터가 많은 테이블이었기 때문에 나누어서 삭제를 해야했다.  
HA구조에서 데이터를 대량으로 삭제를 할 경우, 5000 QPS(Query Per Seconde)을 넘는 경우에는 복제지연이 발생 할 수 있기 때문이다.
복제지연은 Master / Slave 간 동기화가 지연되는 현상을 말한다.   

데이터를 나누어서 삭제하기 위해 레코드를 출력을 제한할 수 있는 쿼리를 작성하기 위해 ROWNUM 함수와 LIMIT 절에 대해서 알아보았다.   

# ROWNUM

ROWNUM함수는 결과 레코드에 대한 순서의 번호를 반환한다.  
SELECT 절에서는 ROWNUM, INST_NUM() 함수를 통해 레코드 순서를 출력 할 수 있으며,
GROUP BY절을 포함한 SELECT 에서는 GROUPBY_NUM() 함수로 레코드의 순서를 알 수 있다.   

SELECT 문에 ORDER BY 절이 포함된 경우 WHERE절에 사용되는 **ROWNUM은 정렬이 되기 전 생성된 순서 값**이므로 주의를 해야한다.   

{% highlight sql %}
--Unexpected results : ROWNUM operated before ORDER BY
SELECT ROWNUM, nation_code FROM participant
WHERE host_year = 1988 AND ROWNUM < 5
ORDER BY gold DESC;
       rownum  nation_code
===================================
            1  'ZIM'
            2  'ZAM'
            3  'ZAI'
            4  'YMD'
{% endhighlight %}           

위의 예제를 보면 4개의 데이터를 gold로 내림차순 정렬 하여 출력하고 있지만, 정렬되지 않은 상태에서의 ROWNUM이 5보다 작은 데이터를 조건으로 출력하기 때문에
예상하지 못한 결과를 받을 수 있다.  

따라서, 정렬된 데이터에서 레코드 순서로 데이터를 가져 오려면 아래의 예제 처럼 ORDERBY_NUM() 함수를 이용해야 한다.   

{% highlight sql %}
--Limiting 4 rows using FOR ORDERBY_NUM()
SELECT ROWNUM, nation_code FROM participant WHERE host_year = 1988
ORDER BY gold DESC
FOR ORDERBY_NUM() < 5;
       rownum  nation_code
===================================
          156  'URS'
          155  'GDR'
          154  'USA'
          153  'KOR'
{% endhighlight %} 

#LIMIT절 

LIMIT 절은 출력되는 레코드의 개수를 제한 할 때 사용한다.   
LIMIT 절과 ROWNUM의 차이는 LIMIT은 정렬 연산을 완료 한 후 최종 레코드 출력의 개수를 제한하고, 
ROWNUM은 정렬 연산 이전에 레코드 순서를 기준으로 출력을 제한하는 차이점이 있다.   
결과적으로 위의 예제에서 ORDERBY_NUM()을 사용하여 출력을 제한한 것과 LIMIT절의 연산과 같다.  

{% highlight sql %}
--Limiting 4 rows using LIMIT
SELECT ROWNUM, nation_code FROM participant WHERE host_year = 1988
ORDER BY gold DESC
LIMIT 4;
       rownum  nation_code
===================================
          156  'URS'
          155  'GDR'
          154  'USA'
          153  'KOR'
{% endhighlight %} 

따라서 아래의 예제는 위의 ORDERBY_NUM()을 사용한 쿼리 결과값이 같음을 확인 할 수 있다.  

> 참고자료   
> http://www.cubrid.org/manual/93/ko/sql/function/rownum_fn.html
> http://www.gurubee.net/m/lecture/2021


