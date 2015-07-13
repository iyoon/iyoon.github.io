---
layout: post
title:  "자바스크립트의 객체지향 프로그래밍"
date:   2015-07-13 23:00:00
categories: jekyll update
---

자바스크립트 6장 

## 객체 지향 프로그래밍 

- ECMAScript은 클래스라는 개념이 없다.
- ECAMScript의 객체는 프로퍼티의 순서 없는 컬렉션이며, 각 프로퍼티는 원시 값, 객체, 함수를 포함한다. 
- 즉, HashTable 처럼 key-value 쌍의 그룹이고, value에는 데이터나 함수가 포함된다. 

# 기본적인 객체 만들기 

- new Object 
{% highlight javascript %}
var person = new Object();
person.name = "hyungwon, Yoon";
person.age = 29;
person.job = "developer"

person.sayName = function(){
	alert(this.name);	
};
{% endhighlight %}

- 리터럴 형식
{% highlight javascript %}
var person = {
	name: "hyungwon, Yoon",
	age: 27,
	job: "developer",
	sayName: function(){
		alert("hyungwon, Yoon");
	}
}
{% endhighlight %}

# 추상화의 필요성 
- new Object, 리터럴 형식으로 객체를 생성하면 중복코드가 발생한다.
- 추상화 (같은 일을 하는 코드에서 공통점을 모아 함수로 만드는 것)

# 팩터리 패턴 
특정 객체를 생성하는 과정을 추상화 

{% highlight javascript %}
function createPerson(name, age, job){
	var o = new Object();
	o.name = name;
	o.age = age;
	o.job = job; 
	o.sayName = function(){
		alert(this.name);
	};
	return o;
}

var person1 = new createPerson("Nicholas", 29 , "Software Enginner");
var person2 = new createPerson("Greg", 27, "Doctor");
{% endhighlight %}
- 생성한 객체가 어떤 타입인지 알 수 없다는 문제가 있다. 



#생성자 패턴 
- ECMASCript의 생성자를 이용 
- 특정한 타입의 객체를 만들 수 있다. 

{% highlight javascript %}
function Person(name, age, job){
	this.name = name;
	this.age = age;
	this.job = job; 
	this.sayName = function(){
		alert(this.name);
	};

	// new Function("alert(this.name)");
}

var person1 = new Person("Nicholas", 29 , "Software Enginner");
var person2 = new Person("Greg", 27, "Doctor");
{% endhighlight %}

***내부적인 과정***

- 객체를 생성 
- 생성자의 this에 생성한 객체를 할당 즉 this 는 객체를 가르키는 포인터 
- 생성자 내부 코드를 실행 (객체에 프로퍼티 추가)
- 새 객체를 반환 

{% highlight javascript %}
alert(person1 instanceof Person) // true
{% endhighlight %}

- 객체의 타입을 알 수 있는 장점이 있다. 

{% highlight javascript %}

	alert(person1.sayName == person2.sayName); // false;
{% endhighlight %}

- 생성자의 문제 인스턴스마다 메서드가 생성되는 문제

# 해결방안 

1. 해당 함수를 전역에 선언
{% highlight javascript %}
function sayName(){
	alert(this.name);

}
{% endhighlight %}

- 전역 스코프에 놓음으로서 코드관리가 어렵다. 


# 프로토타입 패턴 

모든 함수는 prototype 프로퍼티를 가진다.
이 프로퍼티는 해당 참조 타입의 인스턴스가 가져야 할 프로퍼티와 메서드를 담고 있는 객체이다. 
프로토타입의 프로퍼티와 메서드는 객체 인스턴스 전체에서 공유된다 

{% highlight javascript %}
function Person(){
	
}

Person.prototype.name = "hyungwon, Yoon";
Person.prototype.age = 27;
Person.prototype.job = "developer";
Person.prototype.sayName = function(){
	alert(this.name);
}

var person1 = new Person();
person1.sayName();	// hyungwon, Yoon

alert(instance.getSuperValue);
{% endhighlight %}







