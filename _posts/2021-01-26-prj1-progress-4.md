---
layout: post
title: "ToyDB : Iterator"
date: 2021-01-26
categories: projects ToyDB
description: ToyDB 프로젝트 네번째 기록, 반복자 구현
image: https://images.unsplash.com/photo-1447945702361-458a1d71f318?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80
image-sm:
comments: true
---

# ToyDB : Iterator
저번 글에서도 말했지만, 인덱스 구조만 짜는 것이 조금 질리기도 했고, 일종의 반복작업의 느낌이 있어서, 프로젝트의 진전 겸 refresh로 반복자를 구현했다. 소스코드는 [여기](https://github.com/pch6828/ToyDB)에 업데이트하고 있다.<br>
<br>

## Iterator?
C++이나 Java, 혹은 그 이외의 언어를 사용해봤다면, iterator를 들어봤거나, 써본 적 있을 것이다. 자료구조에서 저장된 element(이걸 한국어로 뭐라고 하지...?)들을 읽는 것을 관리하는 구조인데, 자료 구조의 내부구현을 고려하지 않고 element에 접근할 수 있다는 장점이 있다. iterator를 사용하면 후에 자료구조를 변경하게 되더라도 전체 코드의 유지보수가 쉽다.<br>
<br>
물론 내가 진행하는 프로젝트처럼 자료구조의 내부구조를 이미 모두 알고 있고, 혼자서 짜고 있는 프로젝트의 경우, 굳이 iterator를 짜지 않고 직접 element에 접근해도 문제는 없을 것이다. 그렇지만 이 프로젝트의 진행이 단순히 코딩 연습이 아닌, 현재 진행 중인 논문 리딩과 관련된 프로젝트이기에, iterator 구현을 하면 좋을 것 같다는 생각을 하게 되었다.<br>
<br>
뜬금없이 논문 리딩 이야기가 왜 나오나 했을 것이다. DBMS의 Query Processing Model 중에 Iterator Model이라는 것이 있는데, 이 모델은 모든 쿼리 연산자가 iterator 형태를 띄고 있어서 pipelining이 가능하게 되어있다.
![iterater_model](/assets/image/post/1-26-1.png)
위의 그림을 보면 (a)는 A가 끝나야 B가 진행되고, B가 끝나야 C가 진행되는 반면, (b)는 A, B, C가 모두 동시에 진행되는 것을 볼 수 있다. 각 단계를 쿼리 연산자라고 보면, (a)는 연산자의 결과가 모두 구해졌을 때 한번에 flush한다고 볼 수 있고, (b)는 일부만 완료되어도 순차적으로 flush된다고 볼 수 있는 것이다. 이렇게 하면 A, B, C가 동시에 진행되는 효과를 얻을 수 있다. 그리고 이러한 방식을 위해서는 각 연산자를 iterator의 형태로 구현해서, 순차적으로 결과를 받아올 수 있도록 해야 한다. 실질적 구현에 대한 이야기는 바로 아래에서 진행하겠다.<br>
<br>

## Iterator Implementation
이번에 내가 구현한 Iterator는 이전에 구현한 인덱스 구조에서 순차적으로 레코드를 읽어오기 위한 것이다. 쉽게 말하자면... GET 연산자를 만들었다고 보아도 된다. 인덱스 구조마다 서로 다른 구현의 iterator가 필요하겠지만, 후에 유지 보수하기 쉬우려면 어느정도 통일된 인터페이스가 필요하다. 따라서, abstract class로 다음과 같은 iterator interface를 만들었다.

<script src="https://gist.github.com/pch6828/7f54fbc035df0cc71d3013935c84c3a3.js"></script>

본래는 `next`외에도 `prev`를 구현하려 했으나, B+Tree의 구현이 양방향 연결이 아니라 포기했다. 또한, prev가 없어도 iterator의 시작점을 임의로 정할 수 있는 `set_iter_from`이 있으니 이후의 구현에 큰 문제는 없으리라고 판단했다.<br>
<br>
아무래도 C++로 구현하고 있어서, 처음에는 C++ STL의 iterator처럼 구현할까 하는 고민도 있었다. 다만, 그렇게 하면 인덱스 구조까지 싹 다 바꿔야 할 듯해서, Java의 iterator와 비슷하게 구현했다. iterator를 read-only로 구현하는 편이 훨씬 편하고, 안전하다고 보았기 때문에, 이런 식의 선택을 했다. 또한, 단순한 scan 연산 이외에 range-search 등에도 쓰일 수 있도록 구현했다.<br>
<br>
기본적으로 구현 자체는 어렵지 않았다. B+Tree의 구조에는 leaf 노드가 `right_sibling`으로 연결이 되어 있었고, `find_leaf`라는 함수도 구현했었기 때문에 찾고자 하는 leaf 노드에 쉽게 접근할 수 있었다. Skip List 또한 기본적으로는 Linked List이기에 사실상 그리 어렵지 않게 iterator를 구현할 수 있었다. 자세한 코드는 github에서 확인하길 바란다.

## Next Step
1. Next Index : Radix Tree<br>
이제야말로 논문에서도 상당히 많이 언급된 Radix Tree, 흔히 Trie라고 부르는 구조를 인덱스로 구현해볼 생각이다. 한다, 한다 말만 하고 미루고 있었는데, 드디어 손을 대보려 한다. 다만... 이 구조는 iterator를 만들어도 정렬된 구조를 유지하기 힘들 것이라.. 조금 걱정이다.