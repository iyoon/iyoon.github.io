---
layout: post
title:  "quartz cron expression "
date:   2015-05-17 18:00:00
categories: jekyll update
---
## quartz cron expression 


cron expression 은 6필드 또는 7필드로 구성이되고 각 글자는 공백으로 구분짓는다.   
표현식은 숫자와 특수문자 (special charaters)로 구성된다. 마지막 년도 필드는 필수 입력이 아니다.  

| 필드이름  | 필수 | 범위  | 사용할 수있는 특수문자  |
|---------|-----|---|---|
|초|YES|0-59|, - * /|
|분|YES|0-59|, - * /|
|시간|YES|0-23|, - * /|
|일|YES|1-31|, - * ? / L W|
|월|YES|1-12 또는 JAN-DEC|, - * /|
|요일|1-7 또는 SUN-SAT|0-59|, - * ? / L W|
|년도|NO|empty, 1970-2099|, - * /|  


# 특수문자  

- \* : 모든값에 매칭 매 분/매 시 마다 수행할 때 사용한다. 
- ? : 값을 정의 하지 않을 때 사용한다.  
- \- : 범위를 지정할 때 사용한다. 시간 필드에 사용한다면 1-5 로 지정하면 1시에서 5시에 수행된다.
- , : 나열하여 지정할 때 사용한다. 시간 필으데 사용한다면 1,5,7 로 지정하면 1시,5시,7시에 수행된다. 
- / : 증가값을 정의할 때 사용된다. 예를 들어 분 필드에 0/10 으로 지정하면 0분에 시작하여 10분마다 수행된다.
- L(Last): 달의 마지막날짜, 주의 마지막날짜를 지정하여 수행 할 때 사용한다.
- W (Weekday): 주어진 날짜의 가장 가까운 평일(월~금)을 지정하여 수행할 때 사용한다. 


# 특정 시간대에 n 분마다 수행되도록 설정하기 
{% highlight text %}
0 0/2 22-23,0-4 * * ?
{% endhighlight %}

매일 오후10시 부터 새벽 5시까지 2분마다 수행이 되도록 설정하였다.  해당 정의를 하면서 실수 했던 점은 아래와 같다. 


1. 새벽 5시까지 수행되도록 할려면 0 - 5 가 아니라 0 - 4 이다.  
2. 오후 10시 부터 새벽 5시는 22-4로 범위를 지정하면 원하는 대로 수행이 되지 않는다.  
따라서 시간 필드에서 자정을 중간에 포함하는 범위를 지정할 경우에는 나누어서 범위를 표현해야 한다. 

# cron expression TEST 와 Generator 사이트 
http://www.cronmaker.com/ 사이트에서 테스트와 생성이 가능하다. 
테스트는 입력한 표현식의 수행되는 시간을 출력해준다. 




> 참고자료  
[quartz cron expression][quartz cron expression]

[quartz cron expression]: http://www.quartz-scheduler.org/documentation/quartz-1.x/tutorials/crontrigger





