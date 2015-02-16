---
layout: post
title:  Javascript Template Engine (jQuert tmpl)
date:   2015-02-15 20:00:00
categories: jekyll update
---


DOM에 데이터를 추가 할 때 아래와같이 append()로 데이터를 추가하게 된다. 

#list.html
	<table>
	    <caption>연락처</caption>
	    <thead>
	        <tr>
	            <th>이름</th>
	            <th>전화번호</th>
	        </tr>
	    </thead>
	    <tbody id="list" >
	    </tbody>
	</table> 

#list.js
	var data = [{'name':'yoon', 'tel': '010-0000-0000'}, {'name':'kim', 'tel':'010-1111-1111'}];
	var appendData = "";
	$.each(data, function(index, value) { 
	    appendData += "<tr><td>" + value.name + "</td> " +  " <td> " + value.tel + " </td></tr>";
	});
	$("#list").append(appendData);


간단한 리스트를 추가함에도 불구하고 많은 문자열 연결연산이 필요하고 복잡하다. 
반복적으로 추가 되는 부분이 복잡해 질 수록 해당 코드는 더 복잡해지기 때문에 관리하기도 어렵고, 개발자가 실수 하기도 쉽다.  

자바에서는 JSP에서 JSTL을 통해 템플릿 데이터를 한다.
자바스크립트에서도 이런 템플릿을 만들 수 있는 자바스크립 템플릿 엔진이 있다. 
이렇게 템플릿을 사용하게되면 데이터모델(json) 과 뷰(template)을 분리 할 수 있는 장점이 있다. 

#Javscript Template Engine의 종류 
1. pure (http://beebole.com/pure/) 
2. jTemplates(http://jtemplates.tpython.com/)
3. trimpath JavaScriptTemplates (http://code.google.com/p/trimpath/wiki/JavaScriptTemplates)
4. jQuery tmpl (https://github.com/BorisMoore/jquery-tmpl)

다양한 자바스크립트 템플릿 엔진 라이브러리들이 많다.
템플릿 엔진별로 제공하는 기능이나 문법이 다르기 때문에 Documents를 읽어보고 사용하기 편한 엔진을 사용하는게 좋을것 같다.

javascript template engine 중 하나인 jquery tmpl을 사용해보았다. 

list.html 
	<script id="template-tel" type="text/j-query-tmpl">
	    <tr>
	        <td>${name}</td>
	        <td>${tel}</td>
	    <tr>
	</script>

	<table>
	    <caption>연락처(template)</caption>
	    <thead>
	        <tr>
	            <th>이름</th>
	            <th>전화번호</th>
	        </tr>
	    </thead>
	    <tbody id="template-list">
	    </tbody>
	</table> 

#list.js
	var data = [{'name':'yoon', 'tel': '010-0000-0000'}, {'name':'kim', 'tel':'010-1111-1111'}];
	$("#template-tel").tmpl(data).appendTo("#template-list");

반복적인 템플릿은 템플릿 코드로 분리하고, 태그를 사용하여 키 값으로 해당 데이터를 표시할 수 있다.  

#jQuery tmpl 태그 

jquery tmpl 에서는 다음과 같은 태그를 제공한다.

## if / else

if ~ else 구문을 사용 할 수 있다. 

{{if item.length > 0 }}
	<li> ${item} </li>
{{else}}
	empty item 
{{/if}}

## html 

html 태그를 문자열로 출력한다. 
{% highlight %}
{{html <li>item</li>}}
{% endhighlight %}

## each

컬렌션의 모든 요소에 대해 반복문을 수행한다.   
index는 each 내부에서 $index 로 사용하면 된다.    

{% highlight %}
{{each list}}
	<li>${index + 1} 번째 데이터</li>
	<li>${item}</li>
{{/each}}
{% endhighlight %}

index와 컬렉션 아이템을 변수명으로 지정하고자 하는 경우에는 아래와 같이 쓴다. 

{% highlight html linenos %}
{{each(i,item)  list}}
	<li>${i + 1} 번째 데이터</li>
	<li>${item}<li>
{{/each}}
{% endhighlight %}


> 더 자세한 내용은 http://borismoore.github.io/jquery-tmpl/demos/step-by-step.html 참고


