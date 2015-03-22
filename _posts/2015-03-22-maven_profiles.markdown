---
layout: post
title:  "Maven Build Profiles"
date:   2015-03-22 18:00:00
categories: jekyll update
---


# Maven Profile 

배포환경에 따라 설정파일이나 리소스파일을 다르게 패키징 해야하는 경우가 있다.  
maven에서는 Profile 설정 만으로 간편하게 각 profile별로 패키징 할 수 있다. 


배포환경마다 달라질 수 있는 정보로는 다음과 같은 경우가 보편적이다.  

1. DB property : 각 환경별로 사용하는 DB가 다른 경우   
2. Logging level: 개발환경은 logging level이 debug라면 운영환경은 error 와 같이 상위 level로 하는 경우   
3. resource : 각 환경 별로 resource 값이나 경로가 다른 경우   


# Maven Profile 설정으로 배포환경에 따른 패키징 하기 

dev(개발환경), test(테스트환경), release(운영환경)으로 나누었다.   
src/main/java/{dev,test,release}에 각 환경별 db.properties 파일을 추가하였다. 

profile-sample  
├── pom.xml  
├── src  
│   ├── main  
│   │   ├── java  
│   │   ├── resources  
│   │   │   ├── dev  
│   │   │   │   └── db.properties  
│   │   │   ├── release  
│   │   │   │   └── db.properties  
│   │   │   └── test  
│   │   │       └── db.properties  


pom.xml에 profiles에 각 환경 별 profile을 추가하였다. 

{% highlight xml %}
<profiles>
    <!-- Development server -->
    <profile>
      <id>dev</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <properties>
        <env>dev</env>
      </properties>
    </profile>

    <!-- test server -->
    <profile>
      <id>beta</id>
      <properties>
        <env>beta</env>
      </properties>
    </profile>
    
    <!-- release server -->
    <profile>
      <id>release</id>
      <properties>
        <env>release</env>
      </properties>
    </profile>
</profiles>
{% endhighlight %}


각 환경별로 resource의 경로를 설정해준다. 
${env}는 profile에 설정한 properties의 <env> 이다. 

{% highlight xml %}
<build>
  <resources>
    <resource>
      <directory>src/main/resources/${env}</directory>
    </resource>
  </resources>
  <testResources>
    <testResource>
      <directory>src/test/resources/resources/${env}</directory>
    </testResource>
  </testResources>
</build>
{% endhighlight %}


각 환경별로 패키징을 하려면 mvn clean package -P "프로파일id" 로 실행한다. 


mvn clean package dev   
mvn clean package test  
mvn clean package release   


(만일 -P 옵션을 제거하고 패키징한다면, profile에 activeByDefault로 설정된 dev profile로 패키징된다.)  

각 profile별로 db.properties가 배포 파일에 추가된 것을 확인 할 수 있다. 

> [Introduction to Build Profiles][Introduction to Build Profiles]

[Introduction to Build Profiles]:http://maven.apache.org/guides/introduction/introduction-to-profiles.html

