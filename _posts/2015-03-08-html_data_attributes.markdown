---
layout: post
title:  "HTML data-* Attributes"
date:   2015-03-08 18:00:00
categories: jekyll update
---
# HTML data-* Attributes

HTM5 에서 Custom Data Attributes를 사용하여 element에 데이터 값을 추가 할 수 있다.  
data-* 의 표현식으로 html element 의 attribute로 추가하면 된다. 
{% highlight html %}
<ul>
  <li data-animal-type="bird">Owl</li>
  <li data-animal-type="fish">Salmon</li> 
  <li data-animal-type="spider">Tarantula</li> 
</ul>
{% endhighlight %}

> HTML data-* Attributes : [w3schools example][att_global_data] 참고 


이전에는 element의 Custom한 데이터를 저장하기 위해 비공식적인 attribute를 추가하여 사용하곤 했었다.     
웹 표준에 없는 attribute 이어서 항상 warning으로 뜨는게 마음에 걸렸었다. 

# CSS에서 해당 data attribute를 select 하는 경우 

{% highlight css %}
[data-animal-type]{
	color: #000000
}
{% endhighlight %}

{% highlight css %}
[data-animal-type="bird"]{
	color:red;
}
{% endhighlight %}

# jQuery 에서 data attribute의 get/set 하기 

<div data-role="page" data-last-value="43" data-hidden="true" data-options='{"name":"John"}'></div>

data-을 제외하고 select 한다.

{% highlight javascript %}
$( "div" ).data( "role" ) === "page";
{% endhighlight %}

여러 개의 -(hypen)으로 연결된 경우에는 camel cased string 형식으로 사용하면 자동으로 data-last-value로 convert 된다. 
{% highlight javascript %}
$( "div" ).data( "lastValue" ) === 43;
{% endhighlight %}

value 값에 따라 convert 된다. 
{% highlight javascript %}
$( "div" ).data( "hidden" ) === true;
{% endhighlight %}

data atrribute가 object인 경우에는 jQuery.parseJOSN으로 파싱하여 object로 사용 할 수 있다.   
valid하지 않은 데이터인 경우에는 string으로 표시한다. 
{% highlight javascript %}
$( "div" ).data( "options" ).name === "John";
{% endhighlight %}


data를 set경우에는 두번째 인자값으로 value를 넣는다. 
{% highlight javascript %}
$("div").data("role","element");
{% endhighlight %}


> [jQuery Data][jQuery Data] 참고 

[jQuery Data]:http://api.jquery.com/data/
[att_global_data]:http://www.w3schools.com/tags/att_global_data.asp
