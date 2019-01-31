---
id: 146
title: Android UI Thread와 Custom Thread에서의 UI 처리
date:   2018-04-22 11:02:38 +0900
comments: true
author: Gil Kim
layout: post
permalink: /android-thread/
views:
- 13
dsq_thread_id:
- 4806778517
categories:
- Git
tags:
- filter
- image
- wordpress
---
<style>
.img-wrapper::before
{
  content: "";
  display:inline-block;
  vertical-align:middle;
  height:100%;

}
.img-wrapper > span
{
  display:inline-block;
  vertical-align:bottom;
  margin-left: 5px;
}
</style>

<br>
봐도봐도 까먹고, 조금 안사용하면 또 까먹게 되는!! <br>
Android에서 UI Thread가 어떻게 도는지, 그리고 직접 작성한 Custom Thread에서 UI 처리는 어떻게 하는지 알아보자. <br>
<img src="/assets/android_thread/erase-in-my-hair.png" style="width:200px; margin:20px;" />
<br>
### 1. UI 스레드 구성<br><br>

<구성도>
<br><img src="/assets/android_thread/ui-thread.png" style="width:100%; " /><br>

*Thread*, *Message Queue*, *Looper*, *Handler* 에 대해서 알아야 한다.  <br><br>
**1. Thread** <br>
프로세스 내에서 실행되는 세부 작업의 단위
(백그라운드 작업 처리를 해봤다면 모를 수가 없는 거죠??)

**2. Message Queue** <br>
Message 혹은 Task를 담는 Queue이다.
Task는 Runnable 인터페이스를 구현한 오브젝트이다.<br>
그렇다면, 누가 Message(또는 Task)를 Message Queue에 갖다 놓는건가?? 바로 핸들러이다.

**3. Handler** <br>
핸들러는 두가지 기능이 있다. <br>
첫째: Looper에게서 받은 Message 또는 Task를 일정 시간동안 수행하는 기능 <br>
둘째: 외부 스레드로 부터 받은 메시지를 Message Queue에 집어 넣는 기능 <br>

**4. Looper** <br>
Message Queue의 내용을 순차적으로 꺼내서 Handler에게 전달해 준다.
<br><br>
자 이제 이 4가지 요소들이 어떻게 돌고 도는건지 이해를 했는가?

<br>
### 2. Custom Thread에서 UI를 변경하기 <br><br>

**1.** 우리는 무심코 Thread를 만들고 UI변경 코드를 넣었다가 런타임 에러를 만난적이 있을 것이다.<br> (컴파일러가 해당 에러를 찾아주지 않아서 초보자가 자주 하는 실수이다.)
{% highlight JavaScript %}
android.view.ViewRootImpl$CalledFromWrongThreadException: Only the original thread that created a view hierarchy can touch its views.
{% endhighlight %}
<br>
왜 에러가 났을까?<br>
에러 메시지를 확인 해보면, Main Thread에서 만 Ui 처리를 할 수 있다고 한다.
왜 그렇게 만들었을까? <br>
스레드가 병렬처리를 한다고 해도, 같은 자원을 동시에 처리하지 않는다는 100% 보장은 없다.<br><br>
하나의 TextView의 내용을 바꾸는 스레드가 여러개 있다고 해보자. 어떤 스레드가 먼저 시작하고 먼저 끝날지도 모르고, 동시에 내용을 바꿀 수 도 있고, 또는 TextView를 바꾸고 있는 사이에 바꾸라고 처리를 할 수도 있다. <br><br>
**그래서! 각 UI 작업들을 일렬로 대기시키고, 하나씩 실행한다면 모든 문제는 해결된다.**<br>
따라서, UI 변경 Task들은 Main Thread의 Message Queue에 순차적으로 쌓이게 된다.<br><br>
<div class="img-wrapper" >
<img src="/assets/android_thread//cut-in-line.gif" style="" />
<span><strong>UI를 바꾸고싶으면 줄을 서시오..</strong></span>
</div>
<br><br>
그렇다면!! 방법을 알아보도록 하자.
<br>
1.







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

{% include disqus.html %}

<br>
