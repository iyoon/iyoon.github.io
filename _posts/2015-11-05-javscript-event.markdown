---
layout: post
title:  "13장 자바스크립트 이벤트"
date:   2015-11-15 18:00:00
categories: jekyll update
---

## 1. 이벤트 
- 문서나 브라우저에서 특정 순간에 일어 난 일 
- DOM 레벨2에서 DOM 이벤트에 대한 표준화를 시작하였음

## 2. 이벤트 흐름 

# 2.1 페이지에서 이벤트가 전달 되는 순서 
  - DOM 트리구조에서 각 요소(Element)에 이벤트를 부여할때 이벤트 흐름 방식을 지정 할 수 있다.  
  - 익스플로러는 ```버블링```, 넷스케이프는 ```캡쳐링``` 방식을 지원한다.

# 2.2 버블링 방식 

- 문서 트리에 가장 깊이 위치한 요소 부터 부모 페이지로 거슬러 올라가는 방식 
- 마치 거품이 올라가는 모양 같아서 '버블링' 이라고 이름 지어짐.
- 최신 브라우저는 모두 이벤트 버블링 방식을 지원을 한다. 
- div 요소를 클릭하게 되면 아래의 그림과 같이 div 요소 부터 DOM 트리를 거슬러 올라간다. 

![버블링 방식](http://s17.postimg.org/51kxf6il7/bubble.png)

# 2.3 캡쳐링 방식 

- 최상위 노드에서 처음으로 이벤트가 발생하여 가장 명시적인 노드에서 마지막으로 발생한다.
- 이벤트가 의도한 요소에 도달하기 직전에 잡아채려는 목적으로 만들어졌다. 
- IE 8이하에서는 지원을 하지 않기 때문에 많이 사용하지 않는다. 

![캡쳐링 방식](http://s17.postimg.org/iwj7xnd0b/capture.png)


## 3.이벤트 핸들러 연결하는 방법

# 3.1 이벤트 핸들러(리스너) 란? 
- 이벤트에 응답하여 호출되는 함수

# 3.2 HTML 이벤트 핸들러 

- html 속성에 이벤트를 설정하는 방식 
{% highlight html %}
<input type="button" vlaue="click me!" onclick="alert(event.type);" />
{% endhighlight %}

# 3.3 html에 이벤트 핸들러를 할당하면 안좋은 이유!
1. 핸들러 코드가 로드되기 이전에 html 요소를 통해 호출 할 수 있음.
2. html 과 자바스크립트가 너무 단단히 묶이게 된다. (이벤트 핸들러 수정시 html과 script 모두 변경이 필요하다.)

##4. DOM 레벨0 이벤트 핸들러

1. 객체에 대한 참조를 얻는다.
2. 이벤트 핸들러 프로퍼티에 핸들러 연결한다. 
  - 일반적으로 소문자로 입력한다.

{% highlight javascript %}

  var btn = document.getElementById("myBtn");
  btn.onclick = function(){
  	alert("clicked");	
  };

{% endhighlight %}

##5. DOM 레벨0 핸들러 제거 

{% highlight javascript %}
btn.onclick = null;
{% endhighlight %}

##6. DOM 레벨 2 이벤트 핸들러 
- DOM 레벨0과의 차이점: 여러 개의 이벤트 핸들러를 연결 할 수 있다. 
- 이벤트 추가: addEventListener()
  - 캡처링: true
  - 버블링: false 
- 이벤트 제거: removeEventListner()
  - 추가 등록시 동일한 함수 객체를 매개변수로 넘겨야 제거가 가능하다. 

{% highlight javascript %}

  var btn = document.getElementById("myBtn");
  btn.addEventListener("click", function(){
    alert("clicked");
  }, false);

{% endhighlight %}

##7. 익스플로러 이벤트 핸들러

- 이벤트 등록: attachEvent()
- 이벤트 제거: detachEvent() 
- IE8이하 버전은 버블링 방식만 지원한다. 

##8. DOM event 객체 

- DOM과 관련된 이벤트가 발생하면 관련 정보는 모두 ```event``` 라는 객체에 저장된다. 
- 이 객체에는 이벤트가 발생한 요소, 이벤트 타입 같은 기본 정보는 물론 이벤트와 관련된 다른 데이터도 저장된다.
- 매개변수로 ```event``` 객체가 전달 된다. 
- ```event``` 객체는 이벤트 핸들러가 아직 실행 중일 때만 존재하며 이벤트 핸들러가 실행을 마치면 ```event``` 객체는 파과된다.

# 8.1 event 프로퍼티 
- **type**: 발생한 이벤트 타입 (onclick, onload...)
  - 포커스 이벤트: 요소가 포커스를 얻거나 잃을때 발생 (focus, blur, focusin, focusout)
  - 마우스 이벤트: 마우스로 어떤 동작을 취할때 발생 (click, dbclick, mousedown ...)
  - 휠 이벤트: 마우스 휠로 어떤 동작을 취할때 발생 (mousewheel ...)
  - 키보드 이벤트: 키보드로 어떤 동작을 취할때 발생 (keydown, kypress, keyup, textInput ...)
  - 구성 이벤트: IME 를 통해 문자를 입력 할 때 발생 
  - 변경 이벤트: DOM 구조가 바뀔 때 발생(요소나 속성의 이름이 발생) (DOMNodeInserted,DOMNodeRemoved,DOMAttrModifed... )

- **bubbles**: 이벤트가 버블링 되는지 나타낸다. 
- **cancelable**: 이벤트의 기본 동작을 처리 할 수 있는지 나타낸다.
- **currentTarget**: 현재 이벤트를 처리 중인 요소, 이벤트 핸들러의 내부의 this와 같다.
- **target**: 이벤트의 실제 타깃

# 8.2 event 함수 

- preventDefault() 
  - 이벤트의 기본 동작을 취소한다. 
  - 이벤트를 취소하려면 해당 이벤트의 ```cancelable``` 프로퍼티가 ```true``` 여야 한다.
  
{% highlight html %} 
<a href="#" id="myLink"> 링크 </a> 
{% endhighlight %}

{% highlight javascript %}
var link = document.getElementById("myLink");
link.onclick = function(event){
  event.preventDefault();
};
{% endhighlight %}

- stopPropagation()
  - 이벤트 전파를 막는다. (캡처링, 버블링을 모두 취소)

{% highlight javascript %}

var btn = document.getElementById("myBtn");
btn.onclick = function(event) {
  alert("Clicked");
  event.stopPropagation();
};

document.body.onclick = function(event) {
  alert("Body Clicked");
};

// Clicked
{% endhighlight %}


##9. UI 이벤트

#9.1 load 이벤트 
- 이미지, 자바스크립트, CSS 같은 외부 자원을 포함해 전체 페이지를 완전히 불러왔을 때 호출됨 
- window 객체에 load event에 handler를 할당 할 수 있다. 
- \<body\> 요소에 onload 속성 추가할 수 있다. 
  - html에서는 window객체에 접근할 수 없기 때문에 일종의 핵 같은 방식
- \<img\> 요소에도 사용가능 (이미지을 불러 왔을 때 hanlder 호출)
  
#9.2 unload 이벤트 
- 다른 페이지로 이동할 때 발생된다. 
- 참조를 제거하여 메모리 누수를 방지하는 목적으로 사용한다. 
- 모든 것이 해제 될때 발생하므로 페이지에 존재하던 객체를 전부 사용 할 수 없다. 
- DOM 노드의 위치나 외형을 조작하려 하면 에러가 발생 할 수 있다.

#9.3 resize 이벤트 
- 익스플로러/사파리/크롬/오페라: 사용자가 브라우저 창 크기를 조절하는 동안 계속 발생 
- 파이어폭스: 창 크기 조절을 멈추는 시점에 발생 

#9.4 scroll 이벤트
- html 요소의 scrollTop과 scrollLeft 를 이용 
  - document.documentElement.scrollTop
  - document.docuemntElement.scrollLeft
- 사파리의 경우 body 요소에 포함
  - document.body.scrollTop
  - document.body.scrollLeft
- 문서를 스크롤 하는 동안 반복 실행된다. 

#9.5 focus 이벤트 
- ```blur``` : 포커스를 잃을때 발생 (버블링 지원X)
- ```focus``` : 요소가 포커스를 받을 때 발생 (버블링 지원X)
- ```focusin``` : 요소가 포커스를 받을 때 발생 (버블링 지원O)
- ```focusout``` : 요소가 포커스를 잃을 때 발생 (버블링 지원O)
- 부모가 div고 내부에 input 요소가 있을때
  - input 요소에 ```focusin``` 이벤트 발생시 부모 div에도 그 이벤트가 전달된다.
  - input 요소에 ```focus``` 이벤트 발생시 부모 div에는 그 이벤트가 전달되지 않는다.

## 10. 마우스 이벤트 

# 10.1 이벤트 종류 

- ```click``` : 사용자가 주요 마우스 버튼(일반적으로 왼쪽 버튼)을 클릭하거나 엔터키를 누를 때 발생
- ```dbclick``` : 사용자가 주요 마우스 버튼을 더블 클릭할 때 발생
- ```mousedown``` : 사용자가 마우스 버튼을 누를 때 발생 
- ```mouseenter``` : 마우스 커서가 요소 밖에서 요소 경계 안으로 처음 이동할 때 발생 ```DOM 레벨3``` ```버블링X```
- ```mouseleave``` : 마우스 커서가 요소 위에 있다가 경계 밖으로 이동 할 때 발생 ```DOM 레벨3``` ```버블링X```
- ```mousemove``` : 마우스 커서가 요소 주변을 이동하는 동안 계속 발생
- ```mouseout``` : 마우스 커서가 요소 위에 있다가 다른 요소 위로 이동할 대 발생 
- ```mouseover``` : 마우스 커서가 요소 바깥에 있다가 요소 경계 안으로 이동할 때 발생 
- ```mouseup``` : 사용자가 마우스 버튼을 누르고 있다가 놓을 때 발생 

# 10.2 마우스 이벤트 발생 순서
  1. ```mousedown```
  2. ```mouseup```
  3. ```click ```
  4. ```mousedown```
  5. ```mouseup```
  6. ```click```
  7. ```dbclick```

##11 좌표 

# 11.1 클라이언트좌표

- 클라이언트 영역 내에서의 좌표 
- 스크롤은 계산하지 않는다. 
  - event.clientX
  - event.clientY


# 11.2 페이지좌표 

- 페이지 기준의 좌표 
  - event.pageX
  - event.pageY
- 스크롤 계산포함 

# 11.3 화면좌표 

- 전체 화면 기준의 좌표 
- 브라우저를 움직여도 값이 같음. 
  - event.screenX
  - event.screenY

{% highlight javascript %}
$(document).click(function(event){
    console.log("clientX : " + event.clientX + "\nclientY : " + event.clientY);
    console.log("pageX : " + event.pageX + "\npageY : " + event.pageY);
    console.log("screenX : " + event.screenX + "\nscreenY : " + event.screenY);
});
{% endhighlight %}


##12 마우스 휠 이벤트 

- onmousewheel 이벤트 핸들러 
- event.wheelDelta : 마우스 휠을 어떤 방향으로 얼마만큼 움직였는지에 대한 정보 


##13 키보드 이벤트

- ```keydown``` : 사용자가 키를 처음 누를 때 발생하며 누르고 있는 동안 계속 발생
- ```keypress``` : 사용자가 키를 누른 결과로 문자가 입력되었을 때 처음 발생하며 누르고 있는 동안 계속 발생 
  - esc 키에서도 발생 
  - DOM 레벨 3 에서는 폐기되었으므로 textInput 이벤트를 권장
- ```keyup``` : 사용자가 키에 손을 뗄 때 발생 


#13.1 사용자가 키를 누르면 아래와 같은 순서로 이벤트가 발생 
1. keydown 
2. keypress (텍스트 입력 전 발생)
3. keyup (텍스트 입력이 된 후에 발생)

#13.2 키 코드 
- keydown 과 keyup 이벤트에서 event 객체의 keyCode 프로퍼티에는 각 키에 대응하는 코드가 있다. 
  - keyCode 는 아스키 값이 들어간다. 
  - 예를들어 7은 55, a나 A는 65 이다.(shift 키 상태와 관계없이)

- DOM 레벨3에서는 ```key```와 ```char``` 두 가지 프로퍼티가 추가 되었다.
  - ```key``` 프로퍼티의 값은 문자 키 인 경우 그대로의 문자 값 (a, A)
  - 문자가 아닌 키 인 경우 키 이름(shift, down)
  - ```char``` 프로퍼티의 값은 문자 인경우 ```key``` 프로퍼티와 동일하고, 문자가 아닌 값은 ```null``` 이 들어간다.

##14 텍스트 이벤트 

#14.1 textInput 이벤트

- DOM 레벨3 에서는 편집 가능한 영역에 문자가 입력 될 때 발생하는 textInput 이벤트를 추가하였다. 
- keypress를 교체할 목적으로 만들어짐
- keypress 이벤트와의 차이점 
  - keypress 이벤트는 포커스를 받을 수 있는 요소 전체에 대해 지원
    - 즉, 백스페이스나 esc 키 같은 키는 무시 
  - textInput은 키를 누른 결과로 새문자가 삽입될 키에 대해서만 동작 
 
- event 객체의 data 프로퍼티에 삽입된 문자 자체가 저장됨 
  - shift와 a를 누르면 event.data = A, a만 누르면  event.data= a


##15 조작 이벤트 
- DOM 레벨2 에서는 DOM의 일부가 변경되었을 때 알리는 이벤트를 제공한다. 

- ```DOMSubtreeModified``` : DOM 구조가 어떻게든 변경되었을 때 발생 
- ```DOMNodeInserted``` : 노드가 다른 노드의 자식으로 삽입될 때 발생
- ```DOMNodeRemoved``` : 노드가 부모 노드로부터 제거될 때 발생
- ```DOMNodeInsertedIntoDocument``` : 노드가 직접적으로 또는 자신이 존재하는 서브트리를 통해 삽입되었을 때 발생 (DOM 레벨3에서 폐기)
- ```DOMNodeRemovedFromDocument``` : 노드가 직접적으로 또는 자신이 존재하는 서브트리를 통해 제거되었을 때 발생 (DOM 레벨3에서 폐기)
- ```DOMAttrModifed``` : 속성이 바뀌었을 때 발생 (DOM 레벨3에서 폐기)
- ```DOMCharacterDataModifed``` : 텍스트 노드의 값이 변경될 때 발생 


#15.1 노드 제거

- removeChild()나 replaceChild()로 DOM에서 노드를 제거한다.
- DOMNodeRemoved 이벤트가 발생한다. 
- ```event.target``` 프로퍼티에는 제거된 노드의 참조가 있다. 
- ```event.relatedNode``` 프로퍼티에는 부모 노드에 대한 참조가 있다. 
- 이 이벤트가 발생하는 시점에서는 아직 노드가 제거 된 상태가 아니다. 
  - 즉, element의 parentNode 프로퍼티는 event.relatedNode와 같다.

#15.2 노드 삽입

- appendChild()나 replaceChild(), insertBefore()로 노드를 DOM에 삽입한다.
- DOMNodeInserted 이벤트가 발생한다.
- ```event.target``` 프로퍼티에는 삽입된 노드의 참조가 있다.
- ```event.relatedNode``` 프로퍼티에는 부모 노드에 대한 참조가 있다.


##16. html5 event 

#16.1 beforeunload 
  - 페이지를 언로드 하기 직전에 발생
  - 확인메시지를 통하여 페이지에 계속 머무를지 닫을지 확인한다.

{% highlight javascript %}
  
  document.body.addEventListener("beforeunload",function(event){
    var message = "이 페이지에서 정말로 나가겠습니까?";
    // 대화 상자에 표시할 문자열을 지정한다. 
    event.returnValue = message;
    // 사파리와 크롬에서는 함수 값으로 반환이 필요 
    return message;

  },false);

{% endhighlight %}

#16.2 DOMContentLoaded 
  - DOM 트리가 완전히 구성되면 발생
  - 외부자원(이미지, 자바스크립트, CSS 등)는 기다리지 않는다.
  - 이벤트 핸들러 등록, DOM 조작에 사용 

##17 메모리와 성능 

#17.1 이벤트 핸들러와 성능의 관계 

- 이벤트 핸들러의 개수가 성능에 직접적으로 영향을 미친다.
  - 각 함수가 메모리를 점유하는 객체 이기 때문이다. 메모리에 객체가 많을 수록 성능은 떨어진다. 

- 이벤트 핸들러를 많이 할당 할 수록 DOM 접근이 많아진다. 

#17.2 이벤트 핸들러의 성능 최적화 

- 이벤트 버블링의 장점을 활용하여 이벤트 핸들러를 하나만 할당하여 이벤트를 처리하는 방식을 사용한다.

{% highlight html %}
<ul id="myLinks">

  <li id="item1"> itme1 </li>
  <li id="item2"> itme2</li>
  <li id="item3"> itme3 </li>
  
</ul>
{% endhighlight %}


{% highlight javascript %}
  
  var list = document.getElementById("myLinks");
  
  EventUtil.addHandler(list, "click", function(event){
      event = EventUtil.getEvent(event);
      var target = EventUtil.getTarget(event);
  
      switch(target.id){
          case "item1":
              document.title = "I changed the document's title";
              break;
  
          case "item2":
              location.href = "http://www.wrox.com";
              break;
  
          case "item3":
              console.log("hi");
              break;
      }
  });

{% endhighlight %}

#17.3  이벤트 핸들러 제거 

- 페이지를 떠나기 전 제거하지 않은 이벤트 핸들러는 메모리에 계속 남는다.
- onunload 이벤트 핸들러를 써서 이벤트 핸들러를 모두 제거한다. 

