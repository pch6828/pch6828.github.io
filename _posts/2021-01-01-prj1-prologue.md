---
layout: post
title: "Project #1 : Prologue"
date: 2021-01-01
categories: projects ToyDB
description: 겨울 방학 첫번째 사이드 프로젝트
image: https://images.unsplash.com/photo-1544383835-bda2bc66a55d?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1021&q=80
image-sm:
comments: true
---

# Project #1 : ToyDB
본래 이번 겨울방학은 연구실 인턴을 하면서 보낼 계획이었다...만, 코로나 상황이 심각해서 상황을 지켜봐야 한다는 답을 받았다(이제 생각해보니 정중하게 거절하신 느낌이기도 하다.). 그렇다고 아무 것도 안하고 또 방학을 보내기에는 이제 내가 4학년이기 때문에, "뭐라도 해야겠다"는 생각을 하게 되었다. 그렇다고 논문만 읽고 있으니 뭔가 성장하고 있다는 느낌이 없어서... 사이드 프로젝트를 진행하고자 한다.<br>
<br>

## Choosing Topic
사이드 프로젝트 주제를 선정하기 위해서 여러가지를 고민했다.

>
1. 현재 난 무엇을 할 수 있지?
1. 현재 트렌드인 언어 or 분야는 무엇이지?
1. 이 중 난 무엇을 하고 싶지?
>

사실 1번은 되게 쉽게 답을 얻을 수 있었다. 내가 주로 사용하는 언어는 C, C++ 이었고, python이나 Java도 어느정도 사용할 수는 있었다. 따라서 첫번째 프로젝트는 C나 C++을 주로 쓰는 프로젝트를 하고 싶었다. 다만, 2번이 조금 문제였다. 한양대에서 운영하는 현장실습업무지원시스템 Hy-WEP에서 인턴 자격사항을 살펴본 결과, C나 C++ 등보다 JS 응용 역량, 혹은 머신러닝 관련 역량을 요구하는 경우가 많았다. 내가 지금 할 수 있는 것만 고려하기에는 미래에 내가 선택할 길이 좁아질 우려가 있었다.<br>
<br>
그렇지만 지금 내가 공부하는 분야는 DB, 그중에서도 DBMS의 설계였다. 단순히 트렌드라는 이유로 지금 하고 있는 공부와 별 관계 없는 프로젝트를 진행하고 싶지는 않았다. 그래서 우선은 1번에 중점을 맞춰서 C++ 로 간단한 DBMS를 구현하는 것을 첫번째 프로젝트 주제로 잡았다(이 결정에는 [이 글](https://www.codementor.io/@npostolovski/40-side-project-ideas-for-software-engineers-g8xckyxef)에서 simple OS implementation을 추천한 것도 한 몫 했다.). 그렇다고 트렌드를 아예 무시할 수도 없으니, JS 관련 프로젝트도 고민해서 진행해볼 생각이다.<br>
<br>

## ToyDB?
사실 2019년에 "데이터베이스시스템 및 응용" 수업에서 구현한 DBMS가 있었다. 다만 이 코드는 처음에 C로 시작해서 중간에 C++로 변경하는 덕분에 (이는 과제 명세의 요구사항이었다.)코드의 가독성이나, Layered Architecture와 같은 면에 있어서 조금 조잡했다. 게다가 DBMS의 꽃이라고 할 수 있는(교수님의 주장이다.) concurrency control과 recovery에 대해서는 구현에 완전히 실패해서, 사실상 미완성인 프로젝트였다. 언젠가 이를 보완할 생각이었는데, 이번 방학에 이를 시작할 생각이다.<br>
<br>
프로젝트의 이름인 <b>ToyDB</b>에 그리 특별한 의미는 없다. 사이드 프로젝트, 즉 토이 프로젝트로 진행하는 DB이기 때문에 이렇게 이름을 붙였다. 단순히 Database Implementation이라기엔, 이미 내 github project 중에 DB_Lecture_Implementation이라는 레포지토리가 있기 때문에 헷갈리기도 하고, 그냥 너무 단순한 이름이라 마음에 안 들기도 했다.<br>
<br>

## TODO
1. Index Structure Implementation<br>
기존의 과제는 B+Tree 구조만을 index로 사용하고 있었다. 여기에 radix tree와 skip list도 index로 가질 수 있도록 추가할 수 있다면 좋을 것 같다.
1. Concurrency Control & Recovery Implementation<br>
기존 과제 수행 시 실패했던 이 두 시스템을 구현해보고 싶다.
1. C++ migration<br>
기존 과제는 시간에 쫓기고 여타 학기 중 일정에 쫓겨서 C style로 짜여진 코드가 상당히 많이 남아있었다. 이러한 구조를 C++ style로 고쳐서 가독성을 올리고 싶다.<br>

우선은 이렇게 익숙한 주제로 사이드 프로젝트를 하나 시작하고자 한다. 언제 어떻게 마무리될지는 모르겠지만, 내가 좀 더 많이 공부할 수 있는 계기가 되길 바란다. 다음에는 이 프로젝트의 진행상황과, 새로운 프로젝트 주제로 돌아오겠다.
