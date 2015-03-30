---
layout: post
title:  "AngularJS_1"
date:   2015-03-22 18:00:00
categories: jekyll update
---


AngularJS (W3school Tutorial)

> [W3school Tutorial][W3school Tutorial] 따라 해보기 

[W3school Tutorial]:http://www.w3schools.com/angular/default.asp

# AngularJS?
AngularJS는 opensource Javascript Framework 이다. 
Miško Hevery에 의해 개발되었으며 현재 구글이 support하고 있다. 

# AngularJS의 특징 
- MVW(Model View Whatever) 웹 프레임워크: MVC 패턴의 개발이 가능하다.

- SPA(Single Papge Application): 한 페이지에서 동기적으로 처리가 수행되는 페이지 개발에 유리하다. 

- 양방향 데이터 바인딩(two-way data binding): 기존에는 DOM Element를 Select한 후에 해당 Element을 조작하는 기능을 추가하여 DOM을 조작하는 방식이 이었다.  
AngularJS는 데이터의 변화에 따라 바로 Bind 하여 데이터 출력을 자동으로 수행한다. 


# Tutorial 

AngularJS CDN 
{% highlight html %}
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.3.15/angular.min.js" />
{% endhighlight %}

AngularJS는 javascript framework으로 위의 CDN을 \<body\> 안에 추가하면 된다.  
\<body\>에 넣게되면 해당 script가 load 되기 까지 lock이 안생기므로 페이지 로딩을 개선 할 수 있다.  


# AngularJS Directives

AngularJS는  "ng-*" 라는 prefix를 갖는 attribute를 HTML 에 추가하여 확장한다.    
HTML5에서는 Custom Data Atrribute로 "data-ng-*"로 사용할 수 있다. 

{% highlight html %}
  <div ng-app="" ng-init="names=[
  {name:'Jani',country:'Norway'},
  {name:'Hege',country:'Sweden'},
  {name:'Kai',country:'Denmark'}]">

  <ul>
    <li ng-repeat="x in names">
      {{ x.name + ', ' + x.country }}
    </li>
  </ul>

  </div>
{% endhighlight %}

- ng-app: AngularJS Application을 초기화한다.    
attirbute에 ng-app을 추가하면 페이지가 로드되었을 때 Angular Application이 로드된다.    
ng-app="moduleName" 과 같이 module을 적어주면 해당 모듈과 연결된다.   
- ng-init: AngularJS application 데이터를 초기화 한다. 예제에서는 names 배열을 초기화하고 있다.     
주로 ng-init 보다는 controller 또는 ng-app과 연결된 모듈에서 데이터를 초기화한다.    
- ng-model: HTML Controls(input, select, textarea)의 값을 Application Data에 바인딩 한다.     
- ng-bind: Application Data를 HTML View에 출력한다.    
- ng-repeat: collection을 순차적으로 접근(foreach)하여 HTML Element를 반복적으로 출력할 때 사용한다.    


# Angular Expressions 

{{ expression }} double brace 안에 표현식을 작성하면 해당 View에 표현식의 결과가 출력된다.   
(ng-bind Driectives 와 같은 동작을 한다.) 표현식에는 변수나 연산자, 리터럴을 넣을 수 있다.   

{% highlight html %}
<div ng-app="" ng-init="firstName='John';lastName='Doe'">
  <p>The name is {{ firstName + " " + lastName }}</p>
</div>
{% endhighlight %}

{% highlight html %}
<div ng-app="" ng-init="firstName='John';lastName='Doe'">
  <p>The name is <span ng-bind="firstName + ' ' + lastName"></span></p>
</div>
{% endhighlight %}

- Object 접근  
person = {firstName:'John',lastName:'Doe'}  
person.firstName     
결과: John    

- Array 접근  
points=[1,15,19,2,40]   
\{\{ point[0] \}\}    
결과: 1


#AngularJS Controllers
AngularJS Application은 Controller에 의해 동작한다.  
Contoller는 일반적인 javascript obejct 생성자에 의해 만들어지고, 하나의 javascript object로 생각하면 된다.   

{% highlight html %}
<div ng-app="myApp" ng-controller="myCtrl">

First Name: <input type="text" ng-model="firstName"><br>
Last Name: <input type="text" ng-model="lastName"><br>
<br>
Full Name: {{firstName + " " + lastName}}

</div>

<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.firstName = "John";
    $scope.lastName = "Doe";
    $scope.fullName = function() {
        return $scope.firstName + " " + $scope.lastName;
    }
});
</script>
{% endhighlight %}

ng-controller에서 "myCtrl" 컨트롤을 연결하고, \<script\> 에서 해당 컨트롤을 작성하면 된다.   
$scope는 controller 객체로, firstName, lastName 과 같은 프로퍼티를 추가 할 수 있으며, 메소드도 추가가 가능하다.   


참고 자료 
----
> AngularJS 도입 선택가이드: http://helloworld.naver.com/helloworld/1172239  
> AngularJS API Doc: https://docs.angularjs.org/api  
> w3schools AngularJS: http://www.w3schools.com/angular/default.asp  





