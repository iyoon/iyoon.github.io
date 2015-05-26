---
layout: post
title:  "Javascript 정복하기1"
date:   2015-05-25 18:00:00
categories: jekyll update
---
> 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 JavaScript for Web Developers (저자: 니콜라스 자카스)을 읽고 공부한 내용입니다. 

## 1장. 자바스크립트란 무엇인가 

# 자바스크립트의 탄생  
- 모뎀 시절 서버를 통한 유효성 검사는 네트워크 성능이 너무 안좋았기 때문에 오랜 시간이 걸리는 문제가 있었다
	- 넷스케이프와 선 마이크로시스템즈의 협력으로 자바스크립트 1.0 탄생  
	- 넷스케이프의 자바스크립트 vs 마이크로소프트의 JScript  

# ECMAScript
- 두 가지 버전의 자바스크립트는 업계에 큰 혼란을 가져왔고, 자바스크립트의 표준화의 필요성을 대두시켰다. 
- ECMAScript의 탄생
	- 문법과 의미를 표준화 하여 플랫폼/제조사에 중립인 스크립트 언어
	- ECMAScript는 문법/타입/선언문/키워드와 같은 언어의 저수준 부분을 정의
	- '판' 으로 버전을 관리 ```ECMA-262 3판``` 부터 프로그래밍 언어로서의 면모를 갖추기 시작하였다. 
	- 최신판은 ECMAScript 5판 

# Javascript
- Javascript는 ECMAScript를 기반으로 구현한 스크립트 언어
- Javascript = ```ECMAScript``` + ```DOM``` + ```BOM``` 
	- ```DOM``` 은 XML을 HTML로 사용 할 수 있도록 확장하여 노드를 조작할 수 있도록 한다.
	- ```BOM``` 은 브라우저 창의 접근과 제어를 할 수 있도록 한다.

## 2장. HTML 속의 스크립트 

# 자바 스크립트 추가하기  

**페이지에 직접 작성하는 방법**

{% highlight html %}
 <script language="JavaScript"> ... </script> 
{% endhighlight %}
- language는 최근 브라우저에서는 아예 값을 무시한다.(폐기됨)

{% highlight html %}
<script type="text/javascript">  ... </script>
{% endhighlight %}
- "text/javascript"가 기본값이므로 생략해도 무방하다.

**외부파일로 추가하는 방법**

{% highlight html %}
<script src="../common.js" charset="UTF-8">  ... </script>
{% endhighlight %}

- 다른 도메인의 스크립트 파일도 사용이 가능하다.
- src 속성을 추가하면 스크립트 내부에는 주석으로 사용이 가능하다.

# \<script\> 요소

- ```<script>``` 요소 내부에서는 위에서 아래로 차례로 해석 
- ```<script>``` 요소 내부의 코드를 전체 평가하기 전에는 페이지의 나머지 콘텐츠는 불러오거나 표시하지 않는다.
- 외부파일의 경우 외부 파일을 다운로드를 받고 파일 전체를 해석하는 동안 페이지 렌더링이 시작되지 않는다.
- ```<body>``` 요소 만나면서 페이지 렌더링이 시작되므로 페이지가 로드 이후에 사용될 스크립트는 ```<body>``` 요소의 페이지 콘텐츠 마지막에 쓴다. 

# \<script\> 속성 

***defer***  
{% highlight html %}
<script type="text/javascript" async src="example.js"></script>
{% endhighlight %}

- 코드는 즉시 내려받지만 실행은 </html> 을 만날 때 까지 지연 한다.

***async***  
{% highlight html %}
<script type="text/javascript" async src="example.js"></script>
{% endhighlight %}

- 코드는 즉시 내려받지만 실행은 </html> 을 만날 때 까지 지연 한다.
- 스크립트가 마크업 순서대로 실행됨을 보장하지 않는다. 

__위의 2개 속성은 ```src``` 속성이 있는 경우에만 유효합니다.__ 


# 자바스크립트를 외부 파일로 관리하면 얻는 이점?
1. **관리하기가 쉽다.**   
	외부파일로 분리하지 않으면 html 페이지에 자바스크립트 코드와 마크업이 섞이므로 관리가 어렵다.

2. **캐싱 활용**   
	브라우저는 외부에서 연결된 자바스크립트 파일을 모두 캐시한다. 서로 다른 페이지에서 같은 자바스크립트 파일을 사용할 때 캐시에서 불러오므로 
	페이지를 불러오는 시간이 짧아진다.

3. **xhtml에서 CDATA 섹션이나 주석 편법을 쓰지 않아도 된다.**  


# 주석 편법 

```<script>``` 요소가 처음 도입이 되었을 때 ```<script>``` 요소를 지원하지 않는 브라우저는 ```<script>``` 요소의 콘텐츠를 페이지에 그대로 출력하는 문제가 있었다.  
```<script>``` 요소를 지원하지 않는 브라우저에서는 콘텐츠를 표시하지 않도록 하기 위해 아래와 같은 방법을 사용한다. 

{% highlight html %}
<script>
<!--
function foo(){
	alert('foo');
}
//-->
</script>
{% endhighlight %}

# \<noscript\>요소 
브라우저가 자바스크립트를 지원하지 않을 때 대체 콘텐츠를 제공하기 위해 만들어졌다.  

- 브라우저가 스크립트를 지원하지 않을 때  
- 브라우저의 스크립트 지원이 꺼져 있을 때 

{% highlight html %}
<body>
	<noscript>
		<h1> This page requires a javascript-enabled browser. </h1>
	</noscript>

</body>
{% endhighlight %}



## 4장. 변수와 스코프, 메모리

# ECMAScript 변수의 특징 
자바스크립트의 변수의 값과 데이터 타입은 느슨한 타입을 취하므로 실행 도중 언제든지 바뀔 수 있다. 

# 원시 값과 참조 값 

**원시값**  

- 5가지 원시 데이터 (undefined, null, boolean, number, string) 가 있다.
- Stack
- 값 복사: 저장된 값을 새로 생성 후 새로운 변수에 복사 
- ```typeof``` 을 통해 원시값의 타입을 알 수 있다.

{% highlight javascript %}
var s = "hyungwon";			// string 
var b = true;				// boolean
var i = 22;				// number
var u;					// undefined
var n = null;				// null
var o = new Object();			// object
var a = function(){ alert('test');}; 	// function
{% endhighlight %}
- 원시 값에는 동적 프로퍼티가 없다. 

{% highlight javascript %}
var person = "hyungwon";
person.age = 27; 
alert(person.age); // Undefined
{% endhighlight %}



**참조값**  

- 객체를 가르키는 참조를 저장하는 변수 
- Heap
- 값 복사: 힙에 저장된 객체를 가르키는 포인터를 복사한다. 참조만 저장 할 뿐 객체 자체를 복사하는 건 아니다!
{% highlight javascript %}
var obj1 = new Object();
var obj2 = obj1; 
obj1.name="hyungwon";
alert(obj1.name);	// hyungwon
alert(obj2.name);	// hyungwon 
{% endhighlight %}
- ```instanceof``` 을 통해 참조 값의 참조 데이터형을 알 수 있다. 

{% highlight javascript %}
alert(person instanceof Object);	// true 
alert(colors instanceof Array);		// false
alert(pattern instanceof RegExp);	// true
{% endhighlight %}

- 참조형에서는 ```동적 프로퍼티``` 추가/삭제/변경이 가능하다.
{% highlight javascript %}
var person = new Object();
person.name="hyungwon"; 
alert(person.name); // hyungwon
{% endhighlight %}


# 함수에서의 매개변수 전달 

- ECMAScript의 함수 매개변수는 모두 값으로 전달한다. 
	- 함수 외부에 있는 값은 함수 내부의 매개변수로 복사 된다. 



- 참조 값인 경우에는? 매개 변수가 참조 형태로 전달 된다?? ***No!!***

{% highlight javascript %}
function setName(obj){
	obj.name = "hyungwon";	// obj가 가르키는 것은 힙 메모리의 전역 객체 
}

var person = new Object();
setName(person);
alert(person.name);		// hyungwon
{% endhighlight %}	

얼핏 보면 참조 변수의 값을 변경하는 것 같다. 하지만 다음의 예제를 보자.

{% highlight javascript %}
function setName(obj){
	obj.name = "hyungwon";
	obj = new Object();
	obj.name = "yoon";
}

var person = new Object();
setName(person);
alert(person.name);	// hyungwon
{% endhighlight %}

***ECAMScript의 함수 매개변수는 지역변수와 다를 것이 없다고 생각하면 쉽다.***


# 실행 컨텍스트

- ```실행 컨텍스트``` 는 변수나 함수에 접근 할 수 있는지 어떻게 행동하는지를 규정한다.

- 가장 바깥쪽에 존재하는 ```실행 컨텍스트``` 는 ```전역 컨텍스트``` 이다. 웹 브라우저에서 ```window``` 라 부른다. 

# 실행 컨텍스트의 life 

- 함수를 호출하면 독자적인 ```실행 컨텍스트``` 가 생성 된다.   
- ```실행 컨텍스트``` 는 ```스택``` 에 쌓인다.
- 함수 실행이 끝나면 해당 컨텍스트는 스택에서 꺼내고 컨텍스트 내부에서 정의된 변수와 함수를 모두 파괴한다.
- 컨트롤을 이전 컨텍스트 반환한다. 
- ```전역 컨텍스트``` 의 경우 웹페이지에서 나가거나 브라우저가 닫힐 때 까지 유지된다.  

{% highlight javascript %}
function add(num1, num2){
	var sum = num1 + num2;
	return sum;
}
add(10,20);
{% endhighlight %}

# 컨텍스트의 내부 
컨텍스트에는 ```Variable Object```, ```Scope Chain```, ```this value``` 가 있다.

***Varialble Obejct***  
로컬 변수와 함수 매개변수(arguments) 정보를 포함한다.

- num1 : 10
- num2 : 20 
- arguments : {0:10, 1:20}
- sum : 30 


***Scope Chain***  

- 실행 컨텍스트가 접근 할 수 있는 모든 변수와 함수에 순서를 정의 한다. 
- 함수 실행 중 로컬변수를 만나면 아래와 같은 순서로 찾아간다. 
	- 현재 스코프 활성화 객체(activation object) -> 부모 컨텍스트 -> 부모의 부모 컨텍스트 -> ... -> 전역 컨텍스트   
- 컨텍스트는 스코프 체인을 따라 상위 컨테스트에서 변수나 함수를 검색 할 수 있다. 
- 그 반대로 상위 컨텍스트에서 하위 컨텍스트 참조는 불가능하다.

***this value***  

-  실행 컨텍스트를 가르키는 것. 일반 객체언어에서 객체의 this, self 와 비슷 

# 자바스크립트에서 주의 해야 할 스코프  

***자바스크립트에는 블록 레벨 스코프가 없다.*** 

{% highlight javascript %}
if(true){
	var color = "blue";
}

alert(color);	// blue
{% endhighlight %}

보통 C,Java와 같은 언어에는 블록마다 스코프가 생성 되어 블록이 끝나면 파괴된다.  
하지만, 자바스크립트에서는 블록 레벨 스코프가 없으므로 블록 내 지역변수가 파괴되지 않는다. 

{% highlight javascript %}
for(var i=0; i < 10; i++){
	sum += i;
}

alert(i); // 10 
{% endhighlight %}

***지역 변수는 ```var``` 키워드로 선언 하자.***  

```var``` 키워드로 변수를 선언하면 함수의 가장 가까운 로컬 컨텍스트에 변수가 추가 된다.  
```var``` 가 없다면 ```전역 컨텍스트``` 에 변수가 추가된다.  

{% highlight javascript %}
function add(num1, num2){
	var sum = num1 + num2;
	return sum;
}

var result = add(10,20);
alert(sum); // undefined
{% endhighlight %}


# 식별자 검색 
컨텍스트 안에서 식별자를 참조 하면 검색을 하게 되는데 검색은 현재 컨텍스트 부터 스코프 체인을 따라 검색한다. 
식별자를 찾으면 더 이상 검색을 하지 않기 때문에 부모 컨텍스트에 해당 식별자가 있더라도 참조 할 수 없다. 
전역 컨텍스트의 변수를 참조하려면 window.color 와 같이 명시해야 한다. 


# 가비지 콜렉션 
웹 브라우저에서 사용 할 수 있는 메모리는 일반적인 데스크톱 애플리케이션의 가용 메모리에 비해 매우 적다.  
자바스크립트가 시스템 메모리를 전부 사용해서 운영체제를 다운시키는 일을 방지 하기 위해서 이다.  
따라서 성능에 영향을 최소화하면서 주기적으로 메모리 정리를 하는 가비지콜레터가 중요하다.  

# 가비지콜렉터의 2가지 방식: 표시하고 지우기, 참조 카운팅

***표시하고 지우기***  


1.가비지 컬렉터가 작동하면 메모리에 저장된 변수 전체에 표시를 한다.(on)

2.해당 변수가 있는 컨텍스트의 변수와 컨텍스트에 있는 변수가 참조하는 변수는 표시를 제거한다.

3.표시가 남아 있는 변수는 삭제 

***참조카운팅***   


1.변수를 선언하고 참조 값이 할당되면 참조 카운트 = 1

2.다른 변수에 같은 참조 값이 할당 되면 참조 카운트 + 1

3.다른 참조 값으로 덮어씌워 지면 참조 카운트 - 1

4.참조 값이 0이 되면 어떤 변수에서도 참조하지 않는 값이므로 삭제한다. 


참조 카운팅은 ```순환 참조``` 문제가 있다. 
{% highlight javascript %}
function problem(){
	var objectA = new Object(); // objectA: 1
	var objectB = new Object(); // objectB: 1
	objectA.someOtherObject = objectB; // objectB : 2
	objectB.anotherObject = objectA; // objectA : 2 
}
{% endhighlight %}

각 object의 프로퍼티에서 서로를 참조하면서 참조 카운팅이 0이 안되기 때문에 메모리 해제가 안된다. 


***결론은....***  

코드실행에 필요한 데이터만 유지하기 위해서는 필요 없어진 데이터는 ```null``` 을 할당하여 참조를 제거하는 편이 좋다.   
지역 변수의 경우에는 컨텍스트가 파괴될 때 같이 참조가 제거 되지만, 전역 변수나 전역 객체의 프로퍼티를 위주로 삭제하면 된다.






