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
<img src="/assets/android_thread/erase-in-my-hair.png" style="width:200px; margin:20px;"/>
<br>
### 1. UI 스레드 구성<br><br>

**\<구성도\>**
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

우리는 무심코 Thread를 만들고 UI변경 코드를 넣었다가 런타임 에러를 만난적이 있을 것이다.<br> (컴파일러가 해당 에러를 찾아주지 않아서 초보자가 자주 하는 실수이다.)
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
<br><br>
**1. UI Handler를 참조하는 방법** <br>
{% highlight JavaScript %}
var uiHandler = Handler()   // UI Handler (Main Handler)
Thread {
  Thread.sleep(1000) // 스레드 작업 내용이 1초 걸린다고 가정
  uiHandler.post {
    //TODO: UI 변경 작업
  }
}.start()
{% endhighlight %}
주의 해야 할점: Handler() 생성을 생성한 Thread 안에 하게 되면 에러가 난다!!<br>
생성한 Thread는 Looper가 없기 때문에 Handler를 생성할 수 없다. <br>
(추후에 HandlerThread에 대해 포스팅 하겠다..)
<br><br>
**2. runOnUiThread 메서드 활용**
{% highlight JavaScript %}
Thread {
  Thread.sleep(1000) // 스레드 작업 내용이 1초 걸린다고 가정
  runOnUiThread {
    //TODO: UI 변경 작업
}
}.start()
{% endhighlight %}

runOnUiThread의 Document문서는 다음과 같다.
{% highlight JavaScript %}
Runs the specified action on the UI thread. If the current thread is the UI thread, then the action is executed immediately. If the current thread is not the UI thread, the action is posted to the event queue of the UI thread.
{% endhighlight %}
현재 스레드가 UI Thread(Main Thread) 라면 바로 실행, 아니라면 EventQueue에 삽입.
<br><br>
**3. AsyncTask 클래스 활용**
{% highlight JavaScript %}
class MyAsyncTask : AsyncTask<Void, Void, String>() {
    override fun doInBackground(vararg params: Void?): String {
        Thread.sleep(1000) // 스레드 작업 내용이 1초 걸린다고 가정
        return "Finish"
    }
    override fun onPostExecute(result: String?) {
        super.onPostExecute(result)
        //TODO: UI 변경 작업
    }
}
{% endhighlight %}
AsyncTask는 스레드 내용의 작업과 UI 작업을 하는 구역을 나누어져 있다. <br>
onPostExecute() 함수에 UI 작업 코드를 넣으면 된다.  <br><br>

**AsyncTask가 잴 좋아보이지?? 근데 상황봐가면서 써야함.. (ThreadPool Size Limitation)** <br>
바로 AsyncTask 생성 갯수에 제한이 있다. <br>
예를들어 페이지 갯수를 알 수 없는 문서의 ThumbNail 이미지 추출 작업으로 각 페이지마다 AsyncTask를 생성한다면
문제가 발생할 수 있다. (왜냐? 페이지 수가 몇백이 넘어갈 수 있자나~)
<br><br>

뭐, 나름 Thread에 대해서 정리해 봤어<br>
질문이나 문제제기는 언제든 한영이니.. 그럼 안녕
<br><br>
[출처]<br>
[다른 스레드에서 UI 처리하는 방법](https://devlab.neonkid.xyz/posts/android/Android,-%EB%8B%A4%EB%A5%B8-%EC%93%B0%EB%A0%88%EB%93%9C%EC%97%90%EC%84%9C-UI-%EC%B2%98%EB%A6%AC%EB%A5%BC-%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95.html)<br>
[Threading With HandlerThread in Android](https://www.raywenderlich.com/5466-threading-with-handlerthread-in-android)<br>

<br>


<br>
