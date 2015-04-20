---
layout: post
title:  "Thread Dump 생성하기"
date:   2015-04-19 18:00:00
categories: jekyll update
---
해당 포스트는 이상민 강사님의 트러블슈팅 가이드 강의를 듣고 작성하였습니다.  
덤프란 메모리상의 내용을 프린트나 파일로 출력하여 내용을 확인 할 수 있는 것을 말한다. 주로 오류 수정을 위해 원인을 파악하는데 이용이 된다. 
장애가 발생하였을 때 원인을 파악하기 위해 덤프파일을 생성하는 것이 중요하다.  


## 쓰레드 덤프 

쓰레드의 스택과 상태 정보를 파일로 출력한다.  
쓰레드 덤프를 생성하는 시점의 쓰레드의 스택과 상태를 저장한다는 점에 유의 해야 한다.  
따라서 쓰레드의 상태변화를 확인하기 위해서는 10~20초 간격으로 적어도 5~10번 정도로 덤프를 생성해야 한다.  


## 쓰레드 덤프 생성 명령어 

# kill 명령어 

{% highlight bash %}
kill -3 pid 또는 kill -quit pid 
{% endhighlight %}

덤프파일은 해당 프로세스가 실행되는 콘솔이나 로그파일에 출력된다.  
WAS(Tomcat)의 경우에는 catalina.out 파일에 덤프가 출력되는 것을 확인 할 수 있다.

맥용 터미널 프로그램인 iTerm을 사용하면 쉽게 덤프파일을 로컬 환경에 저장 할 수 있다. 
	1. Shell > log > start
	2. 덤프 로그파일이 저장 될 경로와 파일명을 지정 
	3. 덤프 생성 명령어 수행 
	4. Sheell > log > stop 
	5. (2)에서 지정한 덤프 파일 경로에 덤프파일 확인 

## pid 확인 하는 방법 

# ps 명령어 

{% highlight bash %}
ps -ef | grep java
{% endhighlight %}

ps 명령어는 현재 실행되고 있는 프로세스의 상태를 표시하는 명령어이다. 
파이프로 grep 사용하여 "java" 문자열을 포함하는 프로세스를 찾는다. 
- e: 모든 계쩡의 프로세스를 출력
- f: 프로세스의 자세한 정보를 출력 

UID/pid/PPID/CSTIME/TTY/TIME/CMD 포맷으로 출력되므로 두번째 탭의 숫자값이 pid이다. 

# jps 명령어 

{% highlight bash %}
jps pid 
{% endhighlight %}

 -v : 자세한 정보를 함께 출력
 jsp 명령어를 쳤을 때 process 명이 출력되지 않는 경우에는  
 temp 에 저장되는 프로세스 정보가 없을 때 그렇게 나오게 된다.   
 /tmp/hsperfdata_계정명/pid 파일에 프로세스의 정보가 계속해서 업데이트 되어 저장 된다.  
 해당 파일이 삭제가 되거나 없다면 jps, jstat, jstack 과 같은 명령어을 사용 할 수 없다.


쓰레드 덤프가 아래와 같이 출력된다.  

{% highlight bash %}
2015-04-19 20:34:53 
Full thread dump Java HotSpot(TM) 64-Bit Server VM (23.25-b01 mixed mode):

"ajp-bio-8001-exec-16" daemon prio=10 tid=0x00007fbded760000 nid=0x7fab waiting on condition [0x00007fbe0c4b8000]
   java.lang.Thread.State: WAITING (parking)
        at sun.misc.Unsafe.park(Native Method)
        - parking to wait for  <0x00000000e35ebca8> (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
        at java.util.concurrent.locks.LockSupport.park(LockSupport.java:186)
        at java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.await(

        
 ... (중간 생략 )

"VM Thread" prio=10 tid=0x00007fbe100b0000 nid=0x61be runnable 

"VM Periodic Task Thread" prio=10 tid=0x00007fbe10520000 nid=0x61d7 waiting on condition 

JNI global references: 386

Heap
 def new generation   total 184320K, used 110783K [0x00000000d5600000, 0x00000000e1e00000, 0x00000000e1e00000)
  eden space 163840K,  64% used [0x00000000d5600000, 0x00000000dbc7b588, 0x00000000df600000)
  from space 20480K,  28% used [0x00000000df600000, 0x00000000dfbb48d8, 0x00000000e0a00000)
  to   space 20480K,   0% used [0x00000000e0a00000, 0x00000000e0a00000, 0x00000000e1e00000)
 tenured generation   total 409600K, used 33182K [0x00000000e1e00000, 0x00000000fae00000, 0x00000000fae00000)
   the space 409600K,   8% used [0x00000000e1e00000, 0x00000000e3e67938, 0x00000000e3e67a00, 0x00000000fae00000)
 compacting perm gen  total 52288K, used 52123K [0x00000000fae00000, 0x00000000fe110000, 0x0000000100000000)
   the space 52288K,  99% used [0x00000000fae00000, 0x00000000fe0e6f80, 0x00000000fe0e7000, 0x00000000fe110000)
No shared spaces configured.
{% endhighlight %}
쓰레드 덤프 생성 시간 / JVM 정보가 출력이 되고 공백 라인으로 구분하여 각 쓰레드의 정보가 출력이 된다. 
마지막에는 Heap 영역 메모리의 사용현황이 출력된다. 

쓰레드 정보는 스페이스로 각 내용이 구분되어 진다.    


1. 쓰레드 이름   
2. 쓰레드 우선순위  
3. 쓰레드 ID  
4. Natvie 쓰레드 ID  
5. 쓰레드 상황  
6. 쓰레드 스택 주소 범위 (from..to)  
7. 쓰레드 스택 정보 (stack 이므로 가장 최근에 수행된 메소드가 상위에 위치한다. 가장 상위에 메소드가 수행 중인 상태에서 덤프가 시행된것.)  


Heap 영역의 정보에는 Eden, Tenured, Perm 영역의 사용량을 확인 할 수 있다. 
Eden 영역은 Young area 영역이라고도 하며, 객체가 생성되었을때 저장되는 heap 영역 중 일부이다. 
해당 영역이 꽉차게 되면 Young GC가 발생하며 살아 남은 객체는 Tenured 영역 (Old Area)로 옮겨진다.
Perm 영역은 로딩된 클래스, 메소드, meta 정보가 저장된다. 

Tenured, Perm 영역이 90% 이상으로 너무 높으면 out of memory error가 발생할 수 있다. 

## 쓰레드 덤프 분석 

thread logic (https://java.net/projects/threadlogic) 을 이용하면 덤프 파일 분석을 편리하게 할 수 있다.  
thread logic 을 다운로드 받으면 jar 파일을 다운로드 받을 수 있다.  
해당 jar 파일을 아래의 명령어로 실행하면 thread logic을 실행 할 수 있다.  
{% highlight bash %}
java -jar threadlogic.jar 
{% endhighlight %}


> 참고자료
> [스레드 덤프 분석하기][스레드 덤프 분석하기]
> [ThreadLogic Documentation][ThreadLogic Documentation]
> 자바 트러블 슈팅 가이드 (이상민님)

[ThreadLogic Documentation]: https://java.net/downloads/threadlogic/ThreadLogic-v0.9.pdf
[스레드 덤프 분석하기]: http://helloworld.naver.com/helloworld/textyle/10963
