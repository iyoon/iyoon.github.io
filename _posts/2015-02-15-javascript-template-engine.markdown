---
layout: post
title:  Javascript Template Engine (jQuert tmpl)
date:   2015-02-15 20:00:00
categories: jekyll update
---


DOM에 데이터를 추가 할 때 아래와같이 append()로 데이터를 추가하게 된다. 

list.html

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
 


list.js 
	
	var data = [{'name':'yoon', 'tel': '010-0000-0000'}, {'name':'kim', 'tel':'010-1111-1111'}];
	var appendData = "";
	$.each(data, function(index, value) { 
		appendData += "<tr><td>" + value.name + "</td> " +  " <td> " + value.tel + " </td></tr>";
	});
	$("#list").append(appendData);

간단한 리스트를 추가함에도 불구하고 많은 문자열 연결연산이 필요하고 복잡하다. 
반복적으로 추가 되는 부분이 복잡해 질 수록 해당 코드는 더 복잡해지기 때문에 관리하기도 어렵고, 개발자가 실수 하기도 쉽다.  

자바에서는 JSP에서 JSTL을 통해 템플릿 데이터를 표시하거나 하는데 자바스크립트에서도 이런 템플릿을 만들 수 있는 자바스크립 템플릿 엔진이 있다. 


javscript template engine의 종류 

1. pure[http://beebole.com/pure/], 
2. jTemplates(http://jtemplates.tpython.com/), 
3. trimpath JavaScriptTemplates (http://code.google.com/p/trimpath/wiki/JavaScriptTemplates)
4. jQuery tmpl 

다양한 자바스크립트 템플릿 엔진이 있는데, 템플릿 엔진별로 제공하는 기능이나 문법이 다르기 때문에 Documents를 읽어보고 사용하기 편한 엔진을 사용하는게 좋을것 같다.
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


list.js

	var data = [{'name':'yoon', 'tel': '010-0000-0000'}, {'name':'kim', 'tel':'010-1111-1111'}];
	$("#template-tel").tmpl(data).appendTo("#template-list");


반복적으로 추가되는 부분은 템플릿을 분리하고, script에서는 해당 템플릿을 사용하여 데이터만 추가해주면된다. 
모델과 뷰를 분리 함으로써 코드도 간결해지고 이해하기 쉬워졌다. 


jquery tmpl 에서는 다음과 같은 태그를 제공한다.
1. {{if}} / {{else}}

2. {{html}}

3. {{each}}

