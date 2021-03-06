---
layout: post
title: "AnimateJS : A, M, P"
date: 2021-02-13
categories: projects AnimateJS
description: AnimateJS 프로젝트 A, M, P 스토리
image: https://images.unsplash.com/photo-1607018407033-7df1d72c3f6c?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=967&q=80
image-sm:
comments: true
---

# AnimateJS : A, M, P
<b>AnimateJS</b>의 A, M, P 스토리를 추가했다. 또한 디자인적으로 어느정도 수정을 더했다. 소스코드는 [여기](https://github.com/pch6828/AnimateJS)에, 실제 사이트는 [여기](https://pch6828.github.io/AnimateJS/)에서 확인할 수 있다. 조언/코드 리뷰 등등은 언제든 환영한다.<br>
<br>

## Design Refactoring
이전부터 고민이었던 '좁은 화면에서의 요소 배치'에 대한 문제를 드디어 해결했다. 좁은 화면 상에서 설명 창과 캔버스를 동시에 보여주는 것은 무리가 있다고 생각이 들어서, 아예 캔버스만 화면에 보이도록 하기로 했다. 처음엔 세로로 배치한 다음, 스크롤로 아래에 내려갔다가 올라올 수 있도록 하려 했는데, 모바일 환경에서는 캔버스에 스크롤하는 것도 화면을 내리는 것으로 인식해서 사용하기 상당히 불편했다.

![new_design](/assets/image/post/2-13-1.png)
<center>좌측은 처음 열었을 때, 우측은 설명창을 올렸을 때</center><br>

결국 다시 스크롤을 막아두고, 버튼을 두어서 설명 창을 토글할 수 있도록 만들었다. 따라서 처음 컨텐츠를 열면 설명창 없이 캔버스만 보여진다. 하단의 반투명한 버튼을 터치/클릭하면 아래에서 설명창이 올라오고, 이때 같이 올라온 버튼을 다시 터치/클릭하면 설명창이 다시 내려가도록 했다. 컨텐츠를 닫는 버튼의 위치도 고민을 했었는데, 이는 좌측 중앙에 고정했고, 설명창이 올라오면 가려지도록 했다.<br>
<br>

그리고 컨텐츠가 열릴 때 바로 나타나는 것보다는 로딩되는 효과를 두는 것이 좋을 것이라 생각했다. 뭔가 특별한 효과보다는 단순하게 오른쪽에서 날아오도록 처리했다. 또한 큰 화면에서는 설명창과 캔버스가 날아오는 속도를 다르게 해서 조금 더 세련되어 보이도록 했다.<br>
<br>

## A - Algorithm
이번에 추가한 스토리는 내 알고리즘 관련 경험에 대한 스토리이다.<br>
<br>

![A_algorhtim](/assets/image/post/2-13-2.png)

역시 알고리즘하면 떠오르는 것은 '정렬'이다. 그 중에서도 버블 소트 알고리즘이 가장 먼저 떠오르는데, 그래서 이 알고리즘을 애니메이션으로 구현해보았다. 위치가 고정된 기둥은 검은색으로 표시하고, 정렬 과정에서 발생하는 swap은 두 기둥의 높이를 50프레임동안 일정 수치만큼 교환하는 식으로 만들었다.<br>
<br>

처음 열었을 때는 모든 기둥이 정렬된 상태라 거의 곧바로 검은색으로 바뀌는데 여기서 캔버스를 터치하면 랜덤으로 기둥들이 셔플된다. 또한 꾹 누르면 계속해서 랜덤으로 셔플되도록 했다.<br>
<br>

## M - Mentor
알고리즘에 대해서 스토리를 만들면서 멘토링에 대한 스토리도 만들어 보았다.<br>
<br>

![M_mentor](/assets/image/post/2-13-3.png)

멘토링이라던지 동기, 후배들한테 질문을 받으면서 가장 많이 들었던 말은 "그저 빛..."이라는 말이었다. 그래서 빛과 관련된 이미지를 만드는 것이 좋다고 생각했고, 등대, 가로등, 뭐 여러가지에서 타이포그래피에 가장 맞는 '백열 전구'를 선택했다. 클릭으로 백열전구를 켜고 끄도록 했는데, 켜질 때 자연스럽게 필라멘트가 깜박깜박하는 효과를 추가했다. 또한 다 켜지고 나서 나타나는 빛은 일정 주기로 일렁일렁하도록 했다.<br>
<br>

## P - Problem setter
알고리즘에 대해서 스토리를 만들면서 출제 경험에 대한 스토리 또한 만들어 보았다.<br>
<br>

![P_problem_setter](/assets/image/post/2-13-4.png)

프로그래밍 대회를 하다 보면 참가자에게 풍선을 달아주는 일이 많다. 그래서 이 풍선이 떠오르는 이미지를 만들었다. 다만, 어떤 효과를 넣을지가 조금 고민이었는데, 마우스 드래그를 인식해서 그 방향과 크기에 맞게 바람이 부는 것과 같은 효과를 넣었다. 이때 풍선들이 그 방향에 맞게 날리고, 흔들리도록 설정했으며, 일정 주기에 맞게 풍선이 바닥에서 나타나도록 했다.<br>
<br>

## Next Step
1. Next Content<br>
현재 제목만 정해놓고 컨텐츠는 추가하지 않은 알파벳이 몇 있다. 이들에 대한 내용을 추가해야 한다.
1. Buttons<br>
컨텐츠를 닫는 버튼, 설명창을 열고 닫는 버튼에 대한 디자인을 추가해야 한다. 또한 인터랙션에 대한 도움말 버튼도 추가해야 한다.

나름대로 잘 진행되는 듯하다. 다만, 너무 이쪽에 몰입한 듯 하니, 기존 프로젝트와 어느정도 균형을 다시 맞춰야겠다.