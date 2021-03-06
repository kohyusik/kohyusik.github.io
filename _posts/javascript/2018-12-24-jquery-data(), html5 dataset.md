---
title: "HTML5 dataset vs jQuery data()"
author_profile: true
categories: 
  - javascript
toc: true
comments: true
tags:
  - javascipt
  - jquery
  - dataset
---


HTML5 dataset
--------------------

`HTMLElement.dataset` 속성은 HTML tag의 커스텀 데이터 속성(data-*)을 을 읽고 쓸 수 있다.

~~~html
<div class="targetDiv" data-test="html">
    <h3>test</h3>
</div>
~~~

~~~javascript
let divTest = document.querySelector('.targetDiv');
let testData = divTest.dataset['test'];
console.log(testData); // print "html"
~~~

참고 : [HTMLElement.dataset](https://developer.mozilla.org/ko/docs/Web/API/HTMLElement/dataset){:target="_blank"}

jQuery data function
--------------------

`jQuery.data()` function 은 HTML tag와 관련된 데이터를 저장하거나 이미 저장된 값을 조회한다.

~~~javascript
let divTest = $('.targetDiv').data('test');
let testData = divTest.dataset.test;
console.log(testData); // print "html"
~~~

참고 : [jQuery.data()](https://api.jquery.com/jquery.data/){:target="_blank"}


difference
--------------------
비슷한 동작을 하는 것으로 알고 있었으나, 차이가 있었다. jQuery.data() 함수는 처음에 호출될 때,
이전에 .data(key, value)로 세팅된 값이 없을 때만 html element의 data-* attribute를 가져오고 
해당 값을 내부에 세팅해버린다.
따라서, 이 상태에서 html의 data-* 속성을 바꾼다해도 내부에 세팅된 값은 변하지 않기 때문에, 
다시 .data(key) 함수를 호출할 때, 바뀐 속성값이 아닌 이전에 세팅된 값을 가져오게 된다.

~~~javascript
// JQuery data()
debugger;
let $test = $('.test');
let jqueryData = $test.data('test');
// 처음에 내부 객체에 data가 없으면 속성 getAttribute(data-test) 값을 가져와
// 내부에 저장하고 해당 값을 리턴
console.log(jqueryData); // print "html"

jqueryData = $test.data('test');
// 위에서 세팅 된 값을 가져옴
console.log(jqueryData); // print "html"

// set HTML dataset
document.querySelector('.test').dataset.test = 'HTML';

// HTML5 dataset
let htmlData = document.querySelector('.test').dataset.test;
console.log(htmlData); // print "HTML"

jqueryData = $test.data('test'); 
// 최초에 세팅 된 값을 가져오기 때문에 이전 값인 html 출력
console.log(jqueryData); // print "html"
console.log($test.attr('data-test')); // print "HTML"
~~~  

dataset, data() 작동 원리와 차이를 이해하고 사용하면 상황에 맞게 적절하게 활용할 수 있다.
차이를 모르고 두가지 모두 혼용해서 사용하게 되면 원하지 않는 결과를 초래할 수도 있다.