---
layout: post
title:  "14장 폼 스크립트"
date:   2015-12-15 18:00:00
categories: jekyll update
---

# 폼 스크립트 
자바스크립트로 필드의 유효성을 검사하고 표준 폼 컨트롤의 기본동작을 확장하기 시작했다. 

- 웹 폼은 HTML에서는 <form> 요소로, 자바스크립트에서는 `HTMLFormElement` 타입으로 표현된다. 

# HTMLFormElement 프로퍼티와 메소드 
- `acceptCharset` : 서버가 처리할 수 있는 문자셋, HTML의 accept-charset 속성과 같다.
- `action` : 요청을 보낼 URL, HTML의 action 속성과 같다.
- `elements` : 폼에 있는 컨트롤 전체의 HTMLCollection이다.
- `enctype` : 요청의 인코딩 타입, HTML의 enctype 속성과 같다.
  - `application/x-www-form-urlencoded` (기본값) 모든 문자를 인코딩 한다.
  - `multipart/form-data` 어떠한 문자 인코딩도 하지 않는다. 주로 파일 업로드 컨트롤에서 사용.
  - `text/plain`  공백이 + 문자로 변환되며 특수 문자를 인코딩 하지 않는다.
- `length` : 폼에 있는 컨트롤 개수이다.
- `method` :  HTTP 요청 타입이며 일반적으로 "get"이나 "post"이다. HTML의 method 속성과 같다.
- `name` : 폼의 이름, HTML의 name 속성과 같다.
- `target` : 요청을 보내고 응답을 받을 창 이름이다. HTML의 target 속성과 같다.
- `reset()` : 품 필드를 모두 기본 값으로 리셋한다.
- `submit()` : 폼을 전송한다.

{% highlight html %}
<form method="post" action="/save">
<div>
  연락처 
  <input type="text" name="txtTel1"> - 
  <input type="text" name="txtTel2"> - 
  <input type="text" name="txtTel3">  
  이름 
  <input type="text" name="txtName">  
</div>
<input type="submit" value="Submit Form" id="submit-btn">
</form>
{% endhighlight %}

# 폼 접근
{% highlight javascript %}
document.getElementById("myForm") // 폼 name으로 접근 
document.getElement("form")[0]; // 폼 index로 접근 
document.myForm; // 폼 name 프로퍼티로 접근 (과거 브라우저에서 사용하던 방식으로 사용하지 않는 것이 좋다.) 
{% endhighlight %}

# 폼 전송 요소 생성  

{% highlight html %}
<input type="submit" value="Submit Form">  // 범용 전송 버튼
<button type="submit">Submit Form</button>  // 커스텀 전송 버튼
<input type="image" src="imageButton.jpg">  // 이미지 버튼
{% endhighlight %}


# submit 이벤트와 submit() 메소드 

- **submit 이벤트** 
  - 폼 전송버튼은 클릭하거나 엔터키를 치면 submit 이벤트가 발생하면서 서버로 폼이 전송 된다. 
  - 유효성 검사를 하기 위해서는 submit 이벤트에서 폼의 유효성검사를 실행 해야 한다.
  - 유효하지 않은 값이 존재하면 이벤트의 기본동작을 취소하도록 한다.
    - EventUtil.preventDefault();

- **submit() 메소드** 
  - submit() 메서드를 호출 할 경우 submit 이벤트를 발생시키지 않는다는 특징이 있다. 

# 중복 sumbit을 막는 방법 

- 폼 전송시 중복으로 전송이 가능하지 않도록 주의해야 한다. 

- 폼을 전송하는 즉시 전송 버튼을 비활성화 하는 방법 

  - submit 이벤트에서 해당 폼의 필드의 disabled 속성을 true 로 설정하여 비활성화 한다. 
  - onclick 이벤트에서 설정하면, onclick --> submit 순서로 이벤트가 발생되면 폼을 전송하기도 전에 버튼이 비활성화 되므로 폼을 전송하지 못하게 되는 경우가 발생하므로 주의한다. 

{% highlight javascript %}
(function(){
  var form = document.forms[0];
  
  //Code to prevent multiple form submissions
  EventUtil.addHandler(form, "submit", function(event){
      event = EventUtil.getEvent(event);
      var target = EventUtil.getTarget(event);
  
      //get the submit button
      var btn = target.elements["submit-btn"];
  
      //disable it
      btn.disabled = true;
      
  });

})();
{% endhighlight%}

# 폼 초기화 (reset)
{% highlight html %}
<input type="reset" value="resetForm"/>
{% endhighlight%}
- 폼을 리셋 하면 폼 필드값은 모두 페이지를 처음 렌더링 했을 때로 돌아간다.
- 비어있던 필드는 빈 필드가 되고, 기본값이 있는 필드는 기본 값으로 초기화 된다. 
- reset() 메서드   
  - submit()과 다르게 reset 이벤트를 발생시킨다. 


# 폼의 요소에 접근하기 
- form 요소의 elements 속성을 이용하여 폼의 요소에 접근 할 수 있다. 
{% highlight javascript %}
var form = document.getElementById("myForm"); // 컨트롤 ID로 접근 
var colorFileds = form.elements["color"]; // 이름 방식의 Index로 접근
{% endhighlight%}

# 폼 요소의 공통 프로퍼티 
- `disabled` : 필드가 비활성 상태임을 알려줌
- `form` : 필드가 속하는 폼을 가리키는 포인터 
- `name` : 필드 이름
- `readOnly` : 읽기 전용을 나타내는 불리언
- `tabIndex` : 필드의 탭 순서
- `type` : text, radio, checkbox 와 같은 type
- `value` : 서버에 전송할 필드 값이다. file 입력 필드에서는 이 프로퍼티가 읽기 전용이며 컴퓨터에 존재하는 파일의 경로이다. 

# 폼 요소의 공통 메서드
- focus()
  - 요소에 포커스(활성화)를 맞춘다.
  - display 또는 visibility 속성으로 숨겨진 요소는 focus시 에러가 발생한다. 
  - HTML5 에서는 autofocus 속성이 추가되어 자동으로 요소에 포커를 옮긴다. 
- blur()
  - 포커스를 제거하는 메서드는 blur()이다. 

# 폼 요소의 공통 이벤트
- blur : 필드가 포커스를 잃을 때 발생
- change : 필드의 value가 변경될 때 발생 
  - \<input\>와 \<textarea\> 요소에서는 value가 바뀌고, 필드가 포커스를 잃을 때 발생 
  - \<select\> 요소에서는 선택된 옵션이 바뀔때 발생
- focus : 필드가 포커스를 받을 때 발생

## 텍스트 박스 유효성 검사 

# 텍스트 박스 스크립트

HTML에는 한 줄짜리 텍스트 박스인 \<input\> 요소와 여러 줄 텍스트 박스인 \<textarea\> 요소가 있다. 

- text의 경우, `size` 로 textarea의 경우 `cols` 와 `rows` 를 통해 필드를 몇 글자 크기로 렌더링 할지 설정할 수 있다. 
- `value` 속성은 텍스트 박스의 초기 값이다.
- `maxlength` 속성은 텍스트 박스에 최대글자 수를 지정한다.   

# 텍스트 박스 값 가져오기 
- `value` 속성을 통해 텍스트 박스의 값을 읽고 쓸수 있다. 

# 텍스트 선택 
- select() 메서드를 호출하면 자동으로 포커스를 해당 텍스트 박스로 옮기며, default 로 전체 텍스트를 선택한다.
- select 이벤트
  - select 이벤트는 텍스트 박스의 텍스트를 선택할 때 발생한다. 
- 브라우저별로 이벤트 발생 시점이 다르다. (주의)
   - 익스 플로러 9이상, 오페라, 파이어폭스 등의 경우에는 텍스트 선택을 끝내는 시점에 발생한다. 
   - 익스플로러 8 버전 이전 등에서는 한 글자만 선택해도 즉시 발생한다. 

# 선택한 텍스트 가져오기 
- html5 에는 텍스트 박스에 `selectionStart` 와 `selectionEnd` 프로퍼티가 있다. 
- 선택된 텍스트의 시작 index와 마지막 index가 각각 프로퍼티에 저장되므로 해당 인덱스로 substring 하면 선택된 텍스트를 가져올 수 있다. 

# 텍스트 일부를 선택하기 

html5 에서는 setSelectionRange() 메서드를 호출해서 텍스트 박스의 텍스트의 일부를 선택할 수 있다. 

- 매개변수로는 선택할 테그스트의 첫번째, 마지막 문자 인덱스를 받는다. 
- 선택된 텍스트를 확인하려면 반드시 텍스트박스로 포커스 되어 있어야 한다.

{% highlight javascript %}
(function(){
  
      function selectText(textbox, startIndex, stopIndex){
          if (textbox.setSelectionRange){
              textbox.setSelectionRange(startIndex, stopIndex);
          } else if (textbox.createTextRange){
              var range = textbox.createTextRange();
              range.collapse(true); // 커서를 맨 처음으로 이동 
              range.moveStart("character", startIndex);
              range.moveEnd("character", stopIndex - startIndex);
              range.select();                    
          }
          textbox.focus();
      }
                    
      var btn = document.getElementById("select-btn");
      EventUtil.addHandler(btn, "click", function(event){
          var textbox = document.forms[0].elements[0];
          selectText(textbox, 4, 7);
      });
            
})();
{% endhighlight %}

# 입력 필터링 

- 문자를 텍스트 박스에 삽입 할 때 keypress 이벤트가 발생한다.
  - String.fromCharCode() 를 쓰면 문자코드를 문자열로 바꿀 수 있다. 
  - keypress 이벤트는 원래 문자 키에서만 발생해야 하지만 파이어폭스와 사파리 (버전 3.1 미만) 에서는 위아래 화살표,Backspace, delete 키에서도 발생한다.  
  - 문자 아닌 키 중 keypress를 발생하는 키의 문자코드는 파이어폭스에서 모드 0 이며, 사파리 3미만 버전에서는 8이므로 10 미만의 문자는 차단하지 않도록 하면된다. 
  - ctrl 키가 눌렸는지를 확인하려면 event.ctrlKey 프로퍼티에 불리언 값으로 저장되므로 해당 프로퍼티로 확인

# 클립보드 활용 이벤트 (html5 지원)

- `beforecopy` : 복사하기 직전에 발생 
- `copy` : 복사 할 때 
- `beforecut` : 잘라내기 전에 
- `cut` : 잘라내기 시 
- `beforepaste` : 붙여넣기 전에 발생 
- `paste` : 붙여넣기시 

- beforecopy, beforecut, beforepaste 이벤트는  실제 이벤트가 발생하기 전에 데이터를 수정할 때만 사용한다. 
- 이벤트를 취소해도 실제 동작이 취소되지 않으므로 주의 해야한다. 
- 취소를 할경우에는 copy, cut, paste 이벤트에서 취소해야 한다. 

- 클립보드 데이터는 익스플로러에서는 widnow객체, 파이어폭스, 사파리, 크롬에서는 evnet 객체의 clipboardData를 통해 접근 할 수 있다. 
- clipboardData 객체에는 getData(), setData(), clearData() 메서드를 제공한다. 
- getData() 메서드는 클립보드에서 문자열 데이터를 가져온다.
  - 매개 변수로 데이터 타입을 받는다. ("text", "url")  
- setData()는 데이터 타입과, 클립보드에 복사할 텍스트를 받는다. 

## SELECT 박스 
---

SELECT 박스는 \<select\> 요소와 \<option\> 요소로 생성한다. 

- 선택된 옵션이 없으면 SELECT 박스의 값은 빈 문자열이다.
- 선택된 옵션이 value 속성이 있으면 SELECT 박스의 값은 선택된 옵션의 value 속성이다. 
- 선택된 옵션에 value 속성이 없으면 SELECT 박스의 값은 해당 옵션의 텍스트이다. 

# select 박스 

- select 값 설정 
  - selectbox.selectedIndex = 0; 
- select 선택된 인덱스 가져오기 
  - var selIndex = selectbox.selectedIndex; 
- select 값 가져오기 
  - var selOption = selectbox.options[selIndex];인덱스로 값 정보 가져오기 

{% highlight javascript %}
EventUtil.addHandler(btn1, "click", function(event){
  selectbox.selectedIndex = 0;
});
EventUtil.addHandler(btn2, "click", function(event){
  selectbox.selectedIndex = 1;
});
EventUtil.addHandler(btn3, "click", function(event){
  var selIndex = selectbox.selectedIndex;
  var selOption = selectbox.options[selIndex];
  console.log("Selected index: " + selectbox.selectedIndex + "\nSelected text: " + selOption.text + "\nSelected value: " + selOption.value);
});
{% endhighlight %}

## HTML5 유효성 검사 
---

# html5 유효성 검사 API 
- html 마크업을 통해 필드 규칙을 constraint 을 지정하면 브라우저가 자동으로 폼 유효성을 검사한다.
유효성 검사는 단지 유효성을 검사 할 뿐이지 유효성 오류를 발생시키진 않는다.

# required 속성 
- 필수 값 입력 유효성 검사를 한다. 해당 프로퍼티에 값이 존재하면 true, 없으면 false를 반환한다. 

{% highlight html %}
<input type="text" name="userName" required> 
{% endhighlight %} 

# email 과 url 입력 타입 

{% highlight html %}
<input type="email" name="email">
<input type="url" name="url">
{% endhighlight %} 

email 타입은 입력된 텍스트가 이메일 주소 형식과 일치하는지 확인한다.

  - url 은 URI 포맷 이어야 한다. 
  - email 형식은 xxx@xxx.com 이메일 포맷이어야한다. 
  - 구형 브라우저에는 해당 type을 지원하지 않으므로 text로 변경하여 지원한다.

# 숫자 범위 형식 

- number, range, datetime, date, datetime-local, month , week, time를 지원한다.
- min, max, step(min 부터 max까지 값이 가질 수 있는 단계)를 지원한다. 
- 예를들어 0부터 100까지 사이에 있는 5의 배수만 받고자 하는 경우, step 속성의 값을 5로 설정한다. 

{% highlight html %}
<input type="number" min="0" max="100" step="5" name="count">
{% endhighlight %}

# 패턴 지원 

- html5 에서는 텍스트 필드에 pattern 속성을 추가되었다. 
- 정규표현식을 지정할 수 있다. 

{% highlight html %}
<input type="text" pattern="\d+" name="count"> 
{% endhighlight %}

- 정규표현식의 시작과 끝을 알리는 ^ 와 $ 는 생략하여 패턴에 입력한다. 

# 유효성 체크하기 

- 주어진 필드가 유효한지 체크를 할때는 checkValidity() 메서드를 사용한다. 
- 필드의 값이 유효하면 true를 반환하고, 아니면 false 를 반환한다. 

{% highlight javascript %}
if( document.forms[0].element[0].checkValidity()){
// 유효 
}
else {
// 유효 하지 않음 
}
{% endhighlight %}

- 폼 전체가 유효한지를 체크 하려면 폼에서 해당 메서드를 호출 하면 된다.

{% highlight javascript %}
docuemnt.forms[0].checkValidity() 
{% endhighlight %}

- 반환 값에는 유효한지 안한지 뿐만 아니라 왜 유효하지 않은지에 대한 결과 값을 반환 한다. 

  - `customError` : setCustomValidity() 가 설정되어있으면 true를 반환
  - `patternMismatch` : 패턴 속성에 일치 하지 않으면 true
  - `rangeOverflow` :  값이  max 속성에 지정한 값보다 크면 true
  - `rangeUnderflow` : 값이 min 속성 보다 작으면 true
  - `stepMisMatch` :  min, max 사이에 주어진 step 속성에 맞지 않으면 true
  - `tooLong` : 값이 maxlength 속성에 주저인 글자 수를 초과했을 때 true
  - `typeMisMatch` :  email, url 형식에 맞지 않은 경우 true
  - `valid` : 다른 프로터티의 유효성 검사 결과가 모두 false  일때 true 이다. (즉, checkValidity() 메서드 반환 값과 동일)
  - `valueMissing` : required 필수 필드 인데,값이 없을 때  true

# 폼의 유효성 검사를 비활성화 처리 (전체 폼)
  - HTML 방식 

{% highlight html %}
<form novalidate> 
{% endhighlight %}

 - javascript 방식  

{% highlight javascript %}
script: document.forms[0].noValidate = true;
{% endhighlight %}

# 특정 전송 버튼에 유효성 검사 비활성화 

{% highlight javascript %}
<input type="button" formnovalidate>
{% endhighlight %}

## 리치 텍스트 
---

# 리치 텍스트 
- 텍스트를 편집하는 위지윅 기능이 있다. 

# 리치 텍스트 (iframe 방식)
- 페이지에 빈 html 파일을 포함한 iframe을 삽입한다.
- designMode 프로퍼티를 통해 빈 문서가 편집가능한 상태가 되며 이 시점 부터 페이지 <body> 요소의  html 을 수정 할 수 있다. 
- designMode 값은 off(기본), on 이 있다. on으로 설정시 전체 문서가 편집 가능한 (캐럿이 나타남) 상태로 변경된다. 

# 리치 텍스트 (contenteditable 속성을 이용한 방식)
- contenteditable 속성은 페이지으 어느 요소에도 지정 할 수 있으며 지정하는 즉시 요소는 사용자가 편집 가능한 상태가 된다.
- 아이프레임과 빈 페이지, 자바스크립트를 섞어 써야 하는 방식에 비해 부담이 적다. 

{% highlight html %}
<div class="editable" id="richEdit" contenteditable> </div>
{% endhighlight %}

요소에 들어있던 텍스트는 자동으로 편집가능 한 상태가 되고, textarea 비슷하다.
contentEditable 프로퍼티를 설정하여 편집모드를 토글 할 수 있다. 

{% highlight javascript %}
var edit = document.getElementById("richEdit");
edit.contentEditable = false; 
{% endhighlight %}

해당 속성을 지원하는 브라우저는 익스 플로러, 파이어폭스, 크롬, 사파리, 오페라 이다. 
모바일 장치에서는 iOS5 이상, 안드로이드는 3이상의 웹킷 이다. 

# 텍스트 편집 메서드  
- document.execCommand() 메서드를 이용하여 텍스트를 편집 할 수 있다. 
- execCommand() API
  - [execCommand() API 참고 문서](https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand "execCommand() API 참고 문서")

# 리치 텍스트 폼 전송하기 
- 폼 리치 텍스트는 폼 컨트롤이 아니라 iframe 이나 contenteditable 속성이 지정한요소를 통해 구현하므로 폼에 속하지 않는다.
- 따라서, 서버에 전송할 때 자동으로 전송되지 않는다. html 을 직접 추출해서 전송해야 한다.
- 일반적으로 hidden 필드에 리치 텍스트의 값을 삽입하여 전송하는 방식을 사용한다. 

{% highlight javascript %}
submit 이벤트 {
// iframe 의 경우 
target.elements["comments"].value = frames["richEdit"].document.body.innerHTML;

// contenteditable 의 경우 
target.elements["comments"].value = document.getElementById("richEdit").innerHTML;

}
{% endhighlight %}
