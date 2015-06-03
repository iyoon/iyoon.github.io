---
layout: post
title:  "Apache Tika로 MIME Type 알아내기"
date:   2015-05-31 18:00:00
categories: jekyll update
---



# MIME Type이란? 

MIME으로 인코딩한 파일은 Content-Type 정보를 가지고 있다.  
MIME Type은 "파일종류/파일포맷" 형태를 가지고 파일 형식에 따라 구동 할 프로그램을 구분지을 수 있도록 해준다.  

# Apache Tika 

Apache Tika는 다양한 문서에서 메타정보와 컨텐츠를 추출 하기 위해 사용할 수 있는 라이브러리 이다.  

**Apache Tika Download**  
Apache Tika는 https://tika.apache.org/download.html 에서 다운로드 받을 수 있다. 

# Maven에서 추가 하기 
maven을 이용한다면 아래의 내용을 pom.xml에 추가하면 된다. 
{% highlight xml %}
<dependency>
    <groupId>org.apache.tika</groupId>
    <artifactId>tika-core</artifactId>
    <version>...</version>
</dependency>

 <dependency>
    <groupId>org.apache.tika</groupId>
    <artifactId>tika-parsers</artifactId>
    <version>...</version>
  </dependency>
{% endhighlight %}


# Ant에서 추가하기 

ant를 사용하는 프로젝트인 경우 jar 파일을 다운로드 받아서 아래와 같이 추가 해준다. 
{% highlight xml %}
<classpath>
  ... <!-- your other classpath entries -->
  <!-- either: -->
  <pathelement location="path/to/tika-core-${tika.version}.jar"/>
  <!-- or: -->
  <pathelement location="path/to/tika-app-${tika.version}.jar"/>
</classpath>
{% endhighlight %}


자세한 내용은 Apache Tika의 Getting Started 문서를 참고한다.

> https://tika.apache.org/1.8/gettingstarted.html


# Tika Java 코드 

Tika 객체를 생성하여서 detect 메소드를 활용하면 File의 MIME Type을 String 문자열로 리턴 받을 수 있다.   
다른 라이브러리를 이용하면 확장자를 통해 MIME Type을 판별하거나 정확한 MIME Type을 얻기가 어려운데...   
Tika는 파일의 확장자를 변조하거나 하여도 정확한 MIME Type을 알려준다.   


{% highlight java %}
private static boolean isValidMimeType(File[] files) {
	// image file, pdf, excel, text, zip 파일에 해당하는 mime type이 아니면 업로드 불가
	Tika tika = new Tika();
	String mimeType;

	try {
		for (File file : files) {
			mimeType = tika.detect(file);
			if (!isPermittedMimeType(mimeType)) {
				return false;
			}
		}
	} catch (Exception e) {
		return false;
	}
	return true;

}


private static boolean isPermittedMimeType(String mimeType) {

	String[] validMimeTypes = {"image", "application/pdf", ...(생략) };

	for (String validMimeType : validMimeTypes) {
		if (StringUtils.startsWith(mimeType, validMimeType)) {
			return true;
		}
	}

	return false;
}
{% endhighlight %}

Tika에서 정의한 Mime Type의 목록을 확인 하려면 tika-mimetypes.xml 내용을 확인 하면 된다.   

> Tika MIME Type  
[tika-mimetypes.xml][tika-mimetypes.xml]

[tika-mimetypes.xml]: http://grepcode.com/file/repo1.maven.org/maven2/org.apache.tika/tika-core/0.6/org/apache/tika/mime/tika-mimetypes.xml 


# MIME Type 포맷 규칙 

Mime Type의 포맷은 일정한 규칙이 있다. 

1. "파일종류/파일포맷" 형식으로 이름이 지어진다. 
2. x- 로 시작하는 (prefix) MIME Type은 non-standard한 MIME 이다. 
3. vnd 로 시작하는 MIME Type은 Vendor specific 한 Type 으로 벤더사에서 제공하는 MIME Type 이다.   
예를 들어 마이크로 소프트사의 msword는 MIME Type이 "application/vnd.ms-word" 이다. 


> [MIME Type 설명과 포맷규칙][MIME Type 설명]  
  [MIME Type 리스트][MIME Type 리스트]]


[MIME Type 설명]:http://www.freeformatter.com/mime-types-list.html
[MIME Type 리스트]:http://grepcode.com/file/repo1.maven.org/maven2/org.apache.tika/tika-core/0.6/org/apache/tika/mime/tika-mimetypes.xml 






