---
id: 145
title: Jekyll 블로그에 댓글 구현하기
date:   2017-11-28 11:02:38 +0900
comments: true
author: Gil Kim
layout: post
permalink: /jekyll-comments/
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
title: Jekyll 블로그에 댓글 구현하기
date:   2017-11-28 11:02:38 +0900
categories: blog jekyll github
excerpt: Jekyll 블로그에 댓글 서비스인 Disqus를 연동하는 방법을 설명합니다.
comments: true
author: Gil Kim
--- -->
Jekyll 블로그는 정적 페이지 입니다. 동적 요소인 댓글 구현을 위해 Disqus 서비스를 많이 이용합니다.
Disqus 서비스를 블로그에 삽입하는 과정을 설명하려고 합니다.
<br>
### 1. Disqus 회원가입을 먼저 합니다.
[Disqus 홈페이지](https://disqus.com)에 회원가입을 하고, [Get Started]를 찾아서 클릭합니다.

2가지 선택사항 중에 [I Want to Install Disqus on my site] 를 눌러 다음 페이지로 이동합니다.

WebSite Name과 Category 를 선택하여 입력합니다. (WebSite Name 예시: user@github.io)

무료로 진행할 경우 [Continue on Basic]을 선택하여 다음으로 이동합니다.

사용하는 플랫폼을 선택합니다. (이 블로그는 Jekyll 플랫폼을 사용하였습니다.)

<br>
### 2. [Jekyll install instructions] 을 따라합니다.
[Jekyll install instructions]를 해석해보면 아래와 같습니다.

<br>
**<1>** YAML Front Matter에 `comments:true`를 작성합니다.

{% highlight YAML %}
layout: android-post
title: Getting Started with Android Studio
comments: true
user-defined-variable: This is a text sentence.
{% endhighlight %}

<br>
 **<2>** Disqus를 로드할 **Universal Embeded Code** 를 작성합니다.


```
<원문>
{% raw %}In between a {% if page.comments %} and a {% endif %} tag, copy and paste
the Universal Embed Code in the appropriate template
where you'd like Disqus to load.

<해석>
페이지 코멘트 설정이 되어있으면 (if page.comments),
Disqus를 로드할 **Universal Embeded Code** 를 붙여 넣으면 된다.
{% endraw %}
```
<br>
**Universal Embeded Code란?**
```
Universal Embeded Code 란 사이트에 Disqus를 로드하고 표시하는 JavaScript 코드 입니다.
```
<br>
**Universal Embeded Code 예제** ( _includes 폴더에 작성된 disqus.html 입니다. )

{% highlight HTML %}
{% raw %} {% if page.comments %} {% endraw %}
<div id="disqus_thread"></div>
<script>

(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://https-tech-knot-github-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
{% raw %} {% endif %} {% endraw %}
{% endhighlight %}

<br>

참고로 이 블로그는 _includes폴더에 disqus.html파일을 따로 만들고, 사용할 post페이지에서 include하여 사용하였습니다.

<br>
*출처: [Universal Embed Code란](https://help.disqus.com/customer/portal/articles/472097-universal-embed-code)*

<br>    
