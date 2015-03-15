---
layout: post
title:  "Mybatis Mapper"
date:   2015-03-15 18:00:00
categories: jekyll update
---

#ResultMap
Mybatis의 ResultMap은 SQL 쿼리의 결과를 Model 객체에 값을 담을 수 있도록 해주는 기능을 한다.   

{% highlight xml %}
<resultMap id="userResultMap" type="User">
  <id property="id" column="user_id" />
  <result property="username" column="user_name"/>
  <result property="password" column="user_password"/>
</resultMap>
{% endhighlight %}

User Model에 user_id 는 id, user_name은 username, user_password는 password 프로퍼티에 값이 들어간다.

Mybytis는 기본적으로 ResultMap을 따로 설정하지 않은경우 column명에 해당하는 property의 이름으로 ResultMap을 생성해준다.

예를 들어 아래와 같은 ResultMap을 자동으로 생성 해준다.
{% highlight xml %}
<resultMap id="userResultMap" type="User">
  <id property="user_id" column="user_id" />
  <result property="user_name" column="user_name"/>
  <result property="user_password" column="user_password"/>
</resultMap>
{% endhighlight %}
column명이 property와 다른 경우에는 SQL에서 프로퍼티와 일치하는 alias를 주면 ResultMap을 만들지 않아도 되므로, 코드량을 줄일 수 있다.

# Association 과 Collection

모델이 모델의 프로퍼티로 모델을 갖는 경우나 콜렉션을 갖는 경우에는 어떻게 ResultMap을 작성해야 할까? 

has-one 관계를 갖는 경우에는 (모델에 프로퍼티로 모델을 갖는경우) assocation을 사용하고,  
has-many 관계를 갖는 경우에는 (모델에 List<T>와 같은 collection이 있는 경우) collection을 사용 하면된다.

두 가지 모두 Nested Select 와 Nested Results 방식이 있다. 

##Nested Select 
{% highlight xml %}
<resultMap id="blogResult" type="Blog">
  <association property="author" column="author_id" javaType="Author" select="selectAuthor"/>
</resultMap>
{% endhighlight %}

{% highlight xml %}
<select id="selectBlog" resultMap="blogResult">
  SELECT * FROM BLOG WHERE ID = #{id}
</select>
{% endhighlight %}

{% highlight xml %}
<select id="selectAuthor" resultType="Author">
  SELECT * FROM AUTHOR WHERE ID = #{id}
</select>
{% endhighlight %}

서브쿼리 처럼 ID가 selectBlog인 쿼리문을 실행하면 association의 select 속성에 정의된 ID가 selectAuthor인 SELECT 쿼리문을 실행한다.

## Nestted Results

{% highlight xml %}
<select id="selectBlog" resultMap="blogResult">
  select
    B.id            as blog_id,
    B.title         as blog_title,
    B.author_id     as blog_author_id,
    A.id            as author_id,
    A.username      as author_username,
    A.password      as author_password,
    A.email         as author_email,
    A.bio           as author_bio
  from Blog B left outer join Author A on B.author_id = A.id
  where B.id = #{id}
</select>
{% endhighlight %}

위와 같은 조인된 쿼리문이 있을 경우에는 nested select방식과 다르게 resultMap을 지정 해주면 Author 객체에 해당 resultMap으로 매핑된다. 

{% highlight xml %}
<resultMap id="blogResult" type="Blog">
  <id property="id" column="blog_id" />
  <result property="title" column="blog_title"/>
  <association property="author" column="blog_author_id" javaType="Author" resultMap="authorResult"/>
</resultMap>
{% endhighlight %}

{% highlight xml %}
<resultMap id="authorResult" type="Author">
  <id property="id" column="author_id"/>
  <result property="username" column="author_username"/>
  <result property="password" column="author_password"/>
  <result property="email" column="author_email"/>
  <result property="bio" column="author_bio"/>
</resultMap>
{% endhighlight %}

Nested Results 방식을 사용할 때는 각 resultMap에 유일한 키값을 id 프로퍼티로 주어야 한다. 

> [MyBatis Mapper XML][MyBatis Mapper XML]

[MyBatis Mapper XML]:https://mybatis.github.io/mybatis-3/ko/sqlmap-xml.html

