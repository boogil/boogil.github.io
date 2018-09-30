---
id: 144
title: Git관리를 이렇게 해요
date:   2017-11-28 11:02:38 +0900
excerpt: 안드로이드 프로젝트에서 Git관리를 어떻게 하는지에 대해서 이야기를 합니다.
comments: true
author: Gil Kim
layout: post
permalink: /git/
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
---
<!-- ---
layout: post
title: Git관리를 이렇게 해요
date:   2017-11-28 11:02:38 +0900
categories: blog jekyll github
excerpt: 안드로이드 프로젝트에서 Git관리를 어떻게 하는지에 대해서 이야기를 합니다.
comments: true
author: Gil Kim
--- -->
안녕하세요. 오늘 애기할 주제는 안드로이드 스튜디오에서 Git 연동을 어떻게 시작하면 좋을지에 대해서 설명하고자 합니다.
<br>
## 시작에 앞서..
일단 저희 안드로이드 개발팀은 소규모로 구성되어 있습니다. 2명이서 협업을 한 프로젝트도 있지만, 대부분은 1인 1프로젝트로 진행이 됩니다.
이전에는 Git의 사용용도를 소스 백업정도로 활용해왔습니다. Git의 여러 기능들을 활용하지 않아서 불편했던 점을 개선하고자 Git 사용법을 공부하고 전략을 세우게 되었습니다.
<br>
## Git-flow를 먼저 알고 가자
Git-flow란 Git 브랜치를 효과적으로 나누고 관리하는 전략을 말합니다.
크게 5가지의 브랜치가 있습니다.
메인 브랜치로는 master, develop 이 있고, 임시 브랜치로는 feature, release, hotfix가 있습니다. 각 브랜치를 간략하게 설명해보겠습니다.
- master: 최종본 브랜치
- develop: 개발 중인 브랜치
- feature: 기능 개발용 브랜치
- release: 이번 버전 출시용 브랜치
- hotfix: relase 후 발생한 버그 수정용 브랜치

아래 그림을 함께 보면 이해가 쉽습니다.

master브랜치로 시작을 하고, develop 브랜치를 만듭니다. develop 브랜치로 작업을 진행하다가 새로운 기능을 넣기위해 feature브랜치를 만들고 작업을 이어갑니다. (기능에 따라서 여러개의 feature브랜치가 생성됩니다.)
기능이 완료되면 develop 브랜치로 이동해서 feature브랜치를 merge합니다. (브랜치 이동을 checkout이라고 합니다.)
개발이 완료되고 출시를 하기위해 release브랜치를 만듭니다. QA를 진행하고, 발견된 버그 수정작업을 진행합니다.
QA를 모두 통과한 후, release 브랜치를 develop 브랜치로 merge하고, 최종적으로 master 브랜치로 merge합니다.
<br><br>
![git-flow 설명](http://3.bp.blogspot.com/-fn9dkyAGwyg/Vm2yi0CeHyI/AAAAAAAAKVY/Op31eQuKzus/s1600/gitflow_1.png)


<br>
## 나의 Git 전략
Git을 시작하면 master브랜치만 존재합니다. 나머지 4가지의 브랜치는 임의로 생성하고 명명한 것들입니다. (develop, feature, release, hotfix)
1인 1프로젝트이고, 개발기간이 주로 2~3개월의 기간인 소규모 프로젝트를 진행하는 입장에서 Git-flow 전략에서 제시한 5가지 브랜치를 모두 이용할 필요는 없다고 판단을 했습니다.
메인 브랜치로 mater, develop을 사용하고, 필요에 따라서 feature브랜치를 만들어 활용하는 방법을 선택하게 되었습니다.

원칙적으로 feature은 하나의 기능으로 구성되어 있지만, 필요에 따라서 같은 기능이지만 구현방법에 따라 브랜치를 나눠서 작업을 할 수도 있습니다.
A라는 기능을 구현하고자 할때, 구현방법은 다양합니다. 예를들어 3가지 방법의 구현방법이 있다고 가정해보겠습니다.
feature-1, feature-2, feature-3 이렇게 3가지의 feature 브랜치들을 생성하고, 각기 다른 구현방법으로 구현 작업을 진행하였습니다.
최종적으로 feature-2가 가장 좋은 구현방법이라는 결론을 내리고 feature-2를 develop로 merge시킵니다. 그리고 남아있는 3개의 feature 브랜치들은 삭제를 합니다.  

<br>
## 다수의 개발자가 협업하는 방법
협업시 업무를 분담해서 서로 다른 기능을 개발하게 됩니다. 이런 경우 상대방의 코드를 건드리거나, 오류있는 코드를 업로드하여  프로젝트를 고장나게 하면 안됩니다. 그러므로 개발 브랜치를 나눠서 작업하거나, 애초에 공통 프로젝트에서 여러갈래의 저장소로 Fork하는 방법이 있습니다.<br>
중요한것은 **Pull Request** 를 사용해야 합니다. Pull Request란 말 그대로 Pull을 요청하는 것입니다. 자신의 코드를 Pull 하기 전에, 팀원들이나 개발팀장에게 먼저 물어보고, 검토를 다 받으면 그때서야 최종 Pull과 함께 Merge를 진행합니다.

<br>
## 다수의 개발자가 같은 코드를 수정하여 충돌이 나는 경우도 많다.
코드의 충돌을 방지하기 위해서는 개발자들은 최대한 작업을 나누어서 진행합니다. 그래도 코드 충돌은 날 수 있습니다.
그럴경우에는 먼저 최초 Pull Request를 진행한 뒤에, 다음 Pull을 요청할 개발자가 코드를 받고 충돌 코드 영역을 직접 수정한 후에 Pull Request를 합니다.
충돌 코드 부분에 대해서는 개발팀과 협의를 해서 수정해야 할 부분과 삭제해야 할 부분을 확인하는 과정도 필요합니다.


<br>
[출처]<br>
[Git-Flow 그림](http://www.continuousimprover.com/2015/12/why-i-am-abandoning-gitflow.html)<br>
[우아한형제들 기술블로그](http://woowabros.github.io/experience/2017/10/30/baemin-mobile-git-branch-strategy.html)<br>
[Fork란 무엇인가](http://ourcstory.tistory.com/131)
<br>



<br>
