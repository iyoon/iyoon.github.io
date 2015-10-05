---
layout: post
title:  "자바스크립트 클라이언트 탐지와 DOM"
date:   2015-10-04 18:00:00
categories: jekyll update
---

#9장 클라이언트 탐지 

# 클라이언트 탐지의 필요성
플랫폼별, 브라우저별, 브라우저의 버전별로 동일한 자바스크립트의 동작이 보장 되지 않는다.
모든 브라우저에서 공통인 기능을 최소화 하여 개발하거나 클라이언트 탐지 방법을 통해 동작하지 않는 코드는 우회할 수 있도록 개발해야 한다. 

#기능탐지 

1. 메소드를 클라이언트의 브라우저에서 직접 실행 해본다.
2. 해당 코드의 프로퍼티 혹은 메소드가 존재하지 않거나 수행이 안될 경우 우회 할 다른 메소드를 이용한다.

예를 들어 DOM에서 element를 찾는 (IE+6)document.getElemementById는 익스플로러 버전6부터 지원한다. 
그 이하의 버전에서는 (IE 5-)document.all 메소드를 통해 같은 기능을 지원한다.

{% highlight javascript %}
function getElemeny(id){
	// 코드 실행의 능률 향상을 위해 가장 일반적인 case를 조건문의 첫번째로 비교한다. 
	if(document.elementById){
		return document.getElementById(id);
	}
	else if(document.all){
		return document.all[id];
	}
	else{
		throw new Error("No way to retieve element");
	}
}
{% endhighlight %}

#잘못된 기능 탐지 

기능탐지 방법을 통해 브라우저를 탐지 하려하면 안된다. 

파이어폭스 브라우저 탐지 코드 

{% highlight javascript %}
var isFireFox = !!(navigator.vendor && navigator.vendorSub);
{% endhighlight %}

과거에는 navigator.vendor 와 navigator.venderSub만 체크하면 파이어폭스 브라우저임을 알 수 있었지만 
현재는 사파리, 크롬 등도 해당 프로퍼티를 사용하고 있다. 

익스플로러 브라우저 탐지 코드 

{% highlight javascript %}
var isIE = !! (document.all && document.uniqueID);
{% endhighlight %}

미래에도 해당 프로퍼티 혹은 메소드가 존재 할 지 단정지을 수 없다. 


#브라우저 그룹 

{% highlight javascript %}
var hasDOM1 = !!(document.getElementById && document.createElement && document.getElementByTagName);
{% endhighlight %}

DOM Level1 의 기능을 지원하는지를 체크하는 기능탐지는 반복적인 기능탐지 코드를 사용하지 않아도 되므로 추천!


# 브라우저 탐지

브라우저의 사용자 ```에이전트문자열```을 통해 어떤 브라우저에서 실행 중인지를 확인하는 방법이다. 
해당 브라우저 탐지 방법은 항상 신뢰 할 수가 없다. 
그 이유는 브라우저가 에이전트 문자열을 통해 서버를 속이는 행위 지속해왔기 때문이다. 

# 서버를 속이는 행위 ? 
넷스케이프 커뮤니테이션즈 (이하 넷스케이프) 는 모질라("모자이크 킬러의 의미")라는 코드네임을 따서 사용자 에이전트 문자열을 다음과 같이 정의했다.

{% highlight text %}
Mozilla/Version [Laugage] (Platform; Encryption)
{% endhighlight %}

후속 주자로서 넷스케이프를 따라 잡으려 했던 익스플로러3의 에이전트 문자열은 다음과 같이 정의 했다. 

{% highlight text %}
Mozilar/2.0 (compatible; MSIE Version; Operating System)
{% endhighlight %}

MSIE 라는 문자열을 통해 IE 인지는 알겠지만... 왠 경쟁사의 Mozilar 코드네임을 사용할 수 밖에 없었을까? 

그 이유는 많은 웹 페이지의 서버에는 페이지를 전송하기 전 넷스케이프사의 브라우저인지를 확인하려고 한다.
서버가 브라우저를 판단하지 못할 경우 페이지가 깨지는 문제가 발생하게 되었다. 

따라서, 익스플로러는 넷스케이프사 브라우저 인 것 처럼 서버를 속일 수 있는 
넷스케이프사의 에이전트 문자열과 호환 될 수 있는 문자열을 정의 한 것이다. 


크롬의 에이전트 문자열 

{% highlight text %}
"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.101 Safari/537.36"
{% endhighlight %}


#10장 DOM 

#DOM
DOM(Document Object Model)은 HTML과 XML문서에 대한 애플리케이션 프로그래밍 인터페이스(API) 이다. 
DOM은 노드의 계층 구조로 이루어져 있다. 

# Node 타입 

노드는 12개의 노드 타입으로 구성되어있고, nodeType 프로퍼티를 통해 알 수 있다. 

- Node.ELEMENT_NODE(1)
- Node.ARRTIBUTE_NODE(2)
- Node.TEXT_NODE(3)
- Node.CDATA_SECTION_NODE(4)
- Node.ENTITY_REFERENCE_NODE(5)
- Node.ENTITY_NODE(6)
- Node.PROCESSING_INSTRUCTION_NODE(7)
- Node.COMMENT_NODE(8)
- Node.DOCUMENT_NODE(9)
- Node.DOCUMENT_TYPE_NODE(10)
- Node.DOCUMENT_FRANMENT_NODE(11)
- Node.NOTATION_NODE(12)

(익스플로러는 Node 타입 생성자를 인터페이스에 노출하지 않으므로 상수 값으로 비교해야한다.)

# nodeName과 nodeValue

nodeName과 nodeValue 프로퍼티는 해당 노드의 정보를 제공한다. 
각 노드 타입에 따라 프로퍼티값이 다르다. 
Node.ELEMENT_NODE는 nodeName은 요소의 태그이름과 일치하며 nodeValue는 null을 갖는다. 

# 노드 사이의 관계 

각 노드에는 childNodes 프로퍼티가 있다. 이 프로퍼티에는 NodeList가 저장된다.
NodeList 객체는 DOM 구조에 대한 쿼리 결과이며 문서가 바뀌면 NodeList 객체에도 자동적으로 반영된다. 
계속 바뀌므로 살아있는 객체라고 부르기도 한다. 

![javascript 노드구조](http://s27.postimg.org/w1db9jzpd/node.png)

# 노드 조작 API
- appendChild() : childNodes 목록에 노드를 추가한다. 
- insertBefore() : 특정 노드의 바로 앞에 노드를 삽입한다.
- replaceChild() : 기존 노드를 새로운 노드로 교체한다.
- removeChild() : 특정 노드를 삭제한다. 
- cloneChilde(boolean copyType) : 노드를 복제한다. (copyType : true로 설정시 deep copy, false로 설정시 shallow copy)

jQuery의 append() 메소드 구현 

{% highlight javascript %}
	append: function() {
		return this.domManip( arguments, function( elem ) {
			// nodeType(1): ELEMENT_NODE, (11): DOCUMENT_FRAGMENT_NODE (9):DOCUMENT_NODE
			if ( this.nodeType === 1 || this.nodeType === 11 || this.nodeType === 9 ) {
				var target = manipulationTarget( this, elem );
				target.appendChild( elem );
			}
		});
	},

	prepend: function() {
		return this.domManip( arguments, function( elem ) {
			if ( this.nodeType === 1 || this.nodeType === 11 || this.nodeType === 9 ) {
				var target = manipulationTarget( this, elem );
				target.insertBefore( elem, target.firstChild );
			}
		});
	},
{% endhighlight %}


{% highlight javascript %}
// Support: IE<8
// Manipulating tables requires a tbody
function manipulationTarget( elem, content ) {
	return jQuery.nodeName( elem, "table" ) &&
		jQuery.nodeName( content.nodeType !== 11 ? content : content.firstChild, "tr" ) ?

		elem.getElementsByTagName("tbody")[0] ||
			elem.appendChild( elem.ownerDocument.createElement("tbody") ) :
		elem;
}
{% endhighlight %}

# Document 타입 

document 객체를 통해 페이지에 대한 정보를 얻고, 구조 및 외관을 조작한다. 

- document.documentElement: \<html\>에 대한 참조를 얻는다. 
- document.body: \<body\>에 대한 참조를 얻는다. 
- document.doctype:  \<!DOCTYPE\> 에 대한 참조를 얻는다. 

# 문서 정보 
document.title : \<title\> 에대한 참조를 얻는다.

- document.URL: 페이지의 URL 
- document.domain: www.nhnent.com 의 경우 nhnent.com
- document.referrer: 이 페이지를 링크한 페이지의 URL이 들어있다. 없는 경우 빈문자열로 설정된다. 


# 요소 위치 
- document.getElementById("myDiv");
- document.getElementByTageName("div");
  - namedItem() 메서드: name 속성으로 탐색이 가능


# 속성 얻기 / 조작

속성명은 대소문자를 구분하지 않는다. 

element.getAttribute("속성명");
- 찾지 못한 경우 null반환 
- 커스텀 속성 값을 가져오는 경우에 사용 가능 (커스텀 속성 값은 프로퍼티로 접근이 불가)


element.setAttribute("속성명", "변경할 값");
- 속성명이 존재 하지 않을 경우에는 속성을 새로 정의하고 값을 설정한다. 

element.removeAttribute("속성명");


# Attribute Node 타입 
nodeName은 attribute 이름, nodeValue는 속성 값이 들어간다.  
Attr 노드는 NamedNodeMap 객체에 저장되며, 해당 객체에는 다음 메서드를 제공한다. 

- getNamedItem(name): nodeName 프로퍼티가 name인 노드를 반환한다.
- removeNameItem(name): nodeName 프로퍼티가 name인 노드를 목록에서 제거한다. 
- setNamedItem(name): node 목록에 속성을 추가하고 nodeName 프로퍼티에 따라 색인한다.
- item(pos): 인덱스가 pos 인 노드를 반환한다. 

