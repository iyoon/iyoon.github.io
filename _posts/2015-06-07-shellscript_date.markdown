---
layout: post
title:  "shell script에서 date 연산하기"
date:   2015-05-31 18:00:00
categories: jekyll update
---

특정 기간에 해당하는 데이터를 이전하는 배치작업이 있어 쉘 스크립트를 작성하였다.  
아래와 같이 3일 간격으로 배치를 수행하도록 스크립트를 작성하였다.  
2015-06-01 ~ 2015-06-04   
2015-06-05 ~ 2015-06-08   
...  


쉘 스크립트에서 날짜 연산을 하려면 date 함수를 이용하면 된다.   
자세한 사용방법은 date --help 를 확인하였다.  

**사용**  
Usage: date [OPTION]... [+FORMAT]  
  or:  date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]  

**옵션**   
-d, --date=STRING         display time described by STRING, not `now'  

**날짜 포맷**  

  %Y   year
  %m   month (01..12)
  %d   day of month (e.g, 01)
  %H   hour (00..23)
  %M   minute (00..59)


{% highlight bash %}
INPUT_FROM_DATE="`cat date.dat`"
INPUT_FROM_DATE=`date +%Y%m%d -d $INPUT_FROM_DATE' +1days'`
echo $INPUT_FROM_DATE
INPUT_TO_DATE=`date +%Y%m%d -d $INPUT_FROM_DATE' +3days'`
echo $INPUT_TO_DATE
echo $INPUT_TO_DATE > date.dat
{% endhighlight %}
 
배치가 수행되고나면 어느 날짜 까지 수행되었는지 저장하기 위해 date.dat 파일에 해당 날짜를 저장 하였다.  
그리고 배치가 수행 될 때마다 date.dat 파일의 날짜를 읽어 3일씩 더하는 연산을 하도록 하였다.  
날짜 연산은 + 3days 이런식으로 연산을 하는데 연산 단위에 따라 year(s), month, day, hour, minute 를 쓰면 된다.  
예를 들어 3일 5시간 후는  + 3days 5hour 로 연산하면 된다.    


