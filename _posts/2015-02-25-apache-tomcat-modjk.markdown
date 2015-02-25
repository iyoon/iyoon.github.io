---
layout: post
title:  "(OSX) Apache Tomcat 연동하기"
date:   2015-02-25 18:00:00
categories: jekyll update
---

개발환경에서 웹 프로젝트를 실행 할 때 보통 Tomcat Server만 설치하여 사용하는 경우가 많다.  
하지만, 개발을 하다보면 개발환경에서 Apache가 필요한 경우가 있다.  

대부분의 OS에는 Apache를 별도로 설치해야하지만 OSX에는 기본으로 Apache가 깔려있다.
(이하 내용은 OSX Yosemite 10.10.1 버전 기준으로 작성 되었습니다. )

**Apache 설치 경로**  
/etc/apache2

├── extra  
│   ├── httpd-autoindex.conf  
│   ├── httpd-dav.conf  
│   ├── httpd-default.conf  
│   ├── httpd-info.conf  
│   ├── httpd-languages.conf  
│   ├── httpd-manual.conf  
│   ├── httpd-mpm.conf  
│   ├── httpd-multilang-errordoc.conf  
│   ├── httpd-ssl.conf  
│   ├── httpd-ssl.conf~orig  
│   ├── httpd-userdir.conf  
│   ├── httpd-vhosts.conf  
│   ├── mod_hgc.conf  
│   └── proxy-html.conf  
├── httpd.conf  
└── workers.properties  
... (생략)

**Apache Log**  
/private/var/log/apache2

먼저 Apache와 Tomcat 연동을 위해서는 mod_jk(tomcat connector)를 설치 해야 한다.   
http://tomcat.apache.org/connectors-doc/ 에서 다운로드 한다. 

받은 파일은 source 파일이므로 컴파일을 하여서 mod_jk.so 파일을 만들어야한다. 

다운로드 받은 파일을 압축을 풀고 아래 내용을 실행한다. 
{% highlight bash %}
./configure
sudo make  
sudo make install  
{% endhighlight %}

mod_jk.so 파일을 apache module이 저장되어 있는 경로로 /usr/libexec/apache2 이동 시킨다. 

/etc/apache2/httpd.conf 에 아래 내용을 추가한다. 


{% highlight bash %}
LoadModule jk_module libexec/apache2/mod_jk.so

<IfModule mod_jk.c>
JkMount /* tomcat
JkWorkersFile "/etc/apache2/workers.properties"
JkLogFile "/private/var/log/apache2/mod_jk.log"
JkLogLevel error
</IfModule>
{% endhighlight %}


/* 모든 하위 실행에 대해서 tomcat이라는 worker로 보내겠다는 의미이다.  
tomcat 은 workers.properties에 정의 되어있다. 

/etc/apache2/workers.properties 위치에 workers.properties 파일을 생성한다. 

**workers.properties**

{% highlight bash %}
worker.list=tomcat
worker.tomcat.type=ajp13
worker.tomcat.port=8009
worker.tomcat.socket_timeout=10
worker.tomcat.connection_pool_timeout=10
{% endhighlight %}


Apache를 재시작하기 위해 먼저 Apache를 stop 한다.
{% highlight bash %}  
sudo apachectl stop
{% endhighlight %}  

Apache를 시작하기 전에 config 파일이 제대로 작성되었는지 검사를 한다. 
{% highlight bash %} 
apachectl -t  
{% endhighlight %}

config 파일에 문제가 없으면 apache를 시작한다.  
{% highlight bash %}
sudo apachectl start 
{% endhighlight %} 

그럼 끝! 














