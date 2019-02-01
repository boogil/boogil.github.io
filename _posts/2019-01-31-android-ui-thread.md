---
id: 148
title: Lamda! 넌 누구냐
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
- kotlin
tags:
- lamda
- kotlin
sitemap :
  changefreq : daily
  priority : 1.0
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
Kotlin으로 갈아탔더니.. Lamda를 안할래야 안 할 수가없다. <br>
Lamda를 왜쓰는지, 주관적인 장/단점이 무었인지 알아보자. <br>
<!-- <img src="/assets/android_thread/erase-in-my-hair.png" style="width:200px; margin:20px;"/> -->
<br>
### 1. 람다의 정의<br><br>

**\<구성도\>**
namu.wiki의 정의를 살펴보면 *익명함수* , *고차 함수에 인자(argument)로 전달되거나 돌려주는 결과값* 라고 한다. <br>
그런데 나는 그냥 **파라미터용 함수** 라고 정의 하겠다. <br><br>

왜 **파라미터용 함수** 라고 달랑 정의하겠냐고?? 이유는 다음과 같다. 

먼저 람다 함수를 왜 쓰는지 알아보자.
1) 함수의 간결함

**마치며** <br>
뭐, 나름 Thread에 대해서 정리해 봤다.<br>
질문이나 문제제기는 언제든 한영이니.. 사요나라
<br><br>
[출처]<br>
[다른 스레드에서 UI 처리하는 방법](https://devlab.neonkid.xyz/posts/android/Android,-%EB%8B%A4%EB%A5%B8-%EC%93%B0%EB%A0%88%EB%93%9C%EC%97%90%EC%84%9C-UI-%EC%B2%98%EB%A6%AC%EB%A5%BC-%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95.html)<br>
[Threading With HandlerThread in Android](https://www.raywenderlich.com/5466-threading-with-handlerthread-in-android)<br>

<br>


<br>
