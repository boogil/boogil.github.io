---
id: 146
title: Javascript에서 Prototype이란?
date:   2018-04-22 11:02:38 +0900
comments: true
author: Gil Kim
layout: post
permalink: /javascript-prototype/
views:
  - 13
dsq_thread_id:
  - 4806778516
categories:
  - Git
tags:
  - filter
  - image
  - wordpress
sitemap :
  changefreq : daily
  priority : 1.0
---
<!-- ---
layout: post
title: Javascript에서 Prototype이란?
date:   2018-04-22 11:02:38 +0900
categories: blog jekyll github
excerpt: Javascript에서 객체지향적인 기능을 담당하는 Prototype에 대해서 설명합니다.
author: Gil Kim
--- -->
초급 웹 개발자의 경우는 Javascript에서 변수와 function 정도만 알고 사용하고 있을겁니다. (저도 그러했습니다.)  
그런데 라이브러리를 참조하거나, 코드 검색중에 간결한 자바스크립트 문장을 발견하고, 또는 function을 선언하고 변수를 사용하는 것을 발견한 적이 있었을 것입니다.
<br>
### 1. Prototype의 사용 예를 살펴봅시다.
  **1.** 외부 라이브러리를 참조해서 그 코드를 까봤더니 이런 코드도 있다.
  {% highlight JavaScript %}
  $.fn.backgroundCycle = function(options) {
    .. 코드 ..
  }
  {% endhighlight %}

  **2.** 자바스크립트 함수를 사용하는데 매개변수 자리에 넣지않는 경우도 있다.
  {% highlight JavaScript %}
  [1, 2, 3, 4, 5].duplicator ();
  {% endhighlight %}

<br>

### 2. 자바스크립트는 객체지향이 가능한가?

필자가 주로 사용하는 자바언어는 클래스를 선언하고, 상속을 받는 객체지향 언어입니다.<br>
자바스크립트의 function을 사용을 많이 해왔지만, 클래스를 선언하고 상속을 하는 코드를 본적이 없습니다.
<br>하지만 결론적으로 말하자면 자바스트립트도 **객체지향 언어** 의 형태로 만들 수 있습니다.

<br>
### 3. 프로토타입이란?
프로토타입은 객체지향 언어에서 사용하는 클래스와 비슷합니다. <br>
메모리를 할당받은 실 객체라고 보시면 됩니다.<br>
자바에서도 최상위 객체가 Object 인것처럼, 자바스크립트에서도 최상위 객체는 Object입니다.

**다음의 예제를 통해 더 자세히 알아보겠습니다.**

![예제1](https://tech-knot.github.io/assets/javascript_prototype_2.PNG)
1. Person이라는 함수를 선언하였습니다. 함수를 선언함과 동시에 Person 프로토타입 객체가 만들어 집니다.
2. Person의 프로토타입 객체에 eyes와 nose라는 변수를 선언하였습니다.
3. kim 이라는 Person 객체를 생성하였습니다.
4. kim의 \__proto__ 와  Person의 prototype을 출력해보면 같은 결과가 출력됨을 확인할 수 있습니다.

<br>
**구조를 살펴보면 다음과 같습니다.**
![예제2](https://tech-knot.github.io/assets/javascript_prototype_1.PNG)

Person function을 선언하면, Person프로토타입을 생성하게 됩니다.<br>
kim도 Person 객체이기 때문에 같은 프로토타입을 가지게 됩니다. (Person 프로토타입을 상속받은 것과 비슷합니다.)<br>
Person프로토타입의 상위 프로토타입 객체는 Object입니다. 따라서 Person 프로토타입의 \__proto__ 는 Object 프로토타입을 가리키게 됩니다. <br><br>

프로토타입은 생성자(constructor)를 꼭 가지고 있습니다. <br>
Person과 Object 프로토타입은 각각 생성자를 가지고 있습니다.

### 4. 위의 1번에서 언급했던 Prototype의 사용 예를 설명드리겠습니다.
  **1.** jQuery.prototype 에 backgroundCycle이라는 function을 추가 한 것입니다. <br>
  그러므로 jQuery 의 내장 함수처럼 backgroundCycle 함수를 사용할 수 있게 됩니다.

{% highlight JavaScript %}
$.fn.backgroundCycle = function(options) {
  .. 코드 ..
}
{% endhighlight %}



  **2.** Array.prototype 에 duplicator라는 function을 추가 한 것입니다.

{% highlight JavaScript %}
Array.prototype.duplicator = function() {
  return this.concat(this);
}
{% endhighlight %}

{% highlight JavaScript %}
[1, 2, 3, 4, 5].duplicator ();
{% endhighlight %}

출력 결과는  [1, 2, 3, 4, 5, 1, 2, 3, 4, 5] 입니다.
<br><br><br>
[출처]<br>
[Object prototype 이해하기](http://insanehong.kr/post/javascript-prototype/)<br>
[프로토타입 이해하기](https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67)<br>
<br>



<br>
