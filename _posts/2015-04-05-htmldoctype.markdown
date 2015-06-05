---
layout: post
title:  "브라우저 문서모드"
date:   2015-04-05 18:00:00
categories: jekyll update
---

과거 웹 표준이 정해져 있지 않았을 때 각 브라우저마다 페이지를 표시하는 렌더링이 규칙이 달랐고, 이 때문에 같은 소스코드 임에도 불구하고 페이지가 다르게 표시되는 문제가 있다.  

최근에는 웹 표준을 지켜 코드를 작성하기 때문에 최신 브라우저라면 어떤 브라우저에서나 같은 페이지를 표시 할 수 있다.  
하지만 오래된 웹 페이지의 경우에는 최신 브라우저에서 깨져 보이거나 브라우저마다 다르게 표시되는 문제가 있다. 

이런 문제를 해결하고자 임시 방편으로 문서 모드라는 것을 사용하여 두 가지 렌더링 모드를 지원하게 되었다.  
두 가지 렌더링 모드에는 표준모드와 쿽스모드(quirks mode)가 있다.  
쿽스 모드는 구식의 코드(웹 표준을 지키지 않은) 가 최신 브라우저에서 정상적으로 표시하기 위한 목적으로 생겼다.  
브라우저는 소스코드에 doctype에 따라 두 가지 모드 중에 선택하여 페이지를 렌더링 하게 된다.  

모드를 선택하는 방법은 각 브라우저 마다 DTD에 따라 문서모드가 선택된다. 

- Q : 쿽스모드(Quirks mode)
- S : 표준모드(Standard mode)
- A : Almost Standard mode 


| Doctype         | NS6 | Old Moz | Moz &Safari &Opera 10&IE10& HTML | Opera 9.0 | IE 8, IE 9 & Opera 9.5 | IE 7 & Opera 7.10 | IE 6 & Opera 7.0 | Mac IE 5 | Konq 3.2 |
|-----------------|-----|---------|----------------------------------|-----------|------------------------|-------------------|------------------|----------|----------|
| None            |Q|Q         |Q                                  |Q           |Q                        |Q                   |Q                  |Q          |Q          |Q
| <!DOCTYPE html> |Q     |S         |S                                  |S           |S                        | A                  |  A                | A         |          |
|<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN">                 | S    |S         |S                                  | S          |   S                     |   A                |       A           |   Q       |     Q     |






이하 생략 (링크 내 테이블 참고)...

> 참고: [Handling of Some Doctypes in text/html][Handling of Some Doctypes in text/html]

[Handling of Some Doctypes in text/html]:https://hsivonen.fi/doctype/