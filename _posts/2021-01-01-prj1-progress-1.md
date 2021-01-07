---
layout: post
title: "ToyDB : File, Buffer"
date: 2021-01-07
categories: projects
description: ToyDB 프로젝트 첫번째 기록, File, Buffer 시스템 구현
image: https://images.unsplash.com/photo-1544523830-2bef1d330b7d?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=675&q=80
image-sm:
comments: true
---

# ToyDB : File, Buffer
<b>ToyDB</b> 의 파일 관리 시스템과 버퍼 관리 시스템을 구현했다. 또한, 기존에 구현한 Index Structure인 B+Tree를 C++로 재구현했다. 전체적으로 2019년에 구현한 과제의 C++ migration이기에, 약 4일 정도로 구현을 마칠 수 있었다. 다만, 생각보다 순조롭지는 않았다. 자세한 이야기는 바로 아래에서 진행하겠다. 소스코드는 [여기](https://github.com/pch6828/ToyDB)에 있으니 혹시 궁금하신 분들은 들어가보시길. 프로젝트에 대한 조언은 언제나 환영하니... 제발 해주세요.<br>
<br>

## File System
DBMS는 <b>D</b>ata<b>B</b>ase <b>M</b>anagement <b>S</b>ystem, 즉 데이터베이스를 관리하는 소프트웨어이다. 요새는 In-Memory DB가 트렌드이긴 하지만, 하루종일 DBMS를 실행하고 있기도 어려울 뿐더러, 머신이라고는 노트북 한대가 전부이기 때문에 <b>ToyDB</b>는 Disk Oriented DB로 만들기로 했다. (사실 이런 이유보다는, 기존의 학부 과제로 구현한 DBMS가 Disk Oriented DB였다는 것이 더 크게 작용했다.) 아무튼, DB를 디스크에 저장하기로 결정을 했으니, 디스크에 저장될 파일을 관리하는 시스템이 필요했다. 따라서, File System을 가장 먼저 구현했다.<br>
<br>
Disk IO의 단위가 될 페이지는 4kb로 두었다. 또한 각 페이지에는 번호(== 파일 내에서의 offset)를 두어, 각 페이지를 가리키는 포인터 대신으로 쓰도록 했다. 각 파일의 첫번째 페이지(0번 페이지)는 카탈로그 페이지로 두어서 각 파일의 Index 구조, 페이지의 수, free 페이지의 번호, root 페이지의 번호 등을 저장하게 했다. 파일의 용량은 차지하지만, 데이터 삭제 등의 이유로 사용하지 않는 페이지는 free 페이지로 두어서 링크드 리스트 형태의 스택으로 관리했다.<br>
<br>
파일 클래스는 실제로 Disk IO 시스템 콜을 사용할 때 이용할 fd(file discripter)와 파일의 이름, 카탈로그 페이지를 가지도록 했다. 또한 pwrite와 pread의 wrapper 함수를 만들어서 Disk IO를 수행할 수 있게 했고, 새로운 페이지의 할당과 사용하지 않는 페이지의 관리를 모두 이 클래스에서 담당하게 했다.<br>
<br>
물론, DBMS가 동시에 여러 파일을 열어서 작업할 수도 있어야 하기 때문에 테이블 클래스를 두어 여러 파일을 동시에 관리할 수 있게 했다. 여기서 해시테이블을 사용해서 DBMS에서 할당한 파일 ID를 이용해 각 파일에 빠르게 접근할 수 있도록 했다. 여기서 해시테이블을 직접 구현한다면 더 좋았겠지만, 우선 편의를 위해 std::unordered_map을 이용했다. 이후에 시간이 된다면 해시테이블도 직접 구현해서 plug-in할 것이다.<br>
<br>

## Buffer System
Disk IO는 상당히 느린 연산이다. 따라서 DB의 매 연산마다 파일에서 직접 DB의 내용을 읽어들여오는 것은 아주 비효율적일 것이다. 이를 개선하기 위해 버퍼를 두어서 Disk IO의 횟수를 줄이도록 했다. 기존 학부 과제에서는 버퍼가 없는 Disk Oriented DB를 먼저 구현하고 Buffer를 후에 구현해서 plug-in하는 식이었지만, 이미 이론적 배경을 어느정도 갖춘 상황에서 굳이 그 흐름을 그대로 따라갈 이유는 없다고 생각했다. 따라서, B+Tree Index 구현 이전에 버퍼부터 먼저 구현했다.<br>
<br>
버퍼는 기본적으로 N개의 프레임으로 구성되며 각 프레임은 페이지의 복사본과 몇가지의 메타데이터로 이루어져 있다. Page Replacement Policy로는 LRU를 선택했고, 그 중에서도 reference counter를 사용한 approximate LRU를 사용했다. 이는 페이지가 참조될때마다 카운터를 하나씩 올리고, 후에 이 페이지가 victim으로 선정되었을 때 카운터가 0이 아니라면 카운터를 하나 내리고 다른 victim을 찾도록 하는 방식이다. 정확하게 LRU가 된다고 볼 수는 없지만, 단순한 배열로도 구현이 가능하며 성능도 크게 차이가 나지 않는다고 배웠다. 실제로 성능 차이가 나는지 아닌지는 이후에 시간이 될 때 구현해서 테스트해보도록 하겠다.<br>
<br>
모든 IO는 이 버퍼를 통해서 발생했고, Disk IO는 버퍼에서 page miss가 일어나거나, DBMS의 종료 혹은 파일을 닫는 등과 같이 페이지의 flush가 일어날 때 이외에는 발생하지 않도록 했다. 이로써 최대한 불필요한 Disk IO를 피할 수 있었고, 성능을 올릴 수 있었다. (과거 필자의 경험으로 순수하게 Disk IO만 하는 것보다 약 20배 정도의 성능 증가가 있었다.)<br>
<br>
또한, Buffer에서 페이지를 가져올 때는 우선 pin을 꽂아서 혹시라도 다른 사용자에 의해 이 페이지가 victim이 되지 않도록 했고, 모든 사용이 끝나면 이 pin을 뽑는 식으로 진행했다. pin은 단순한 카운터이며, 매번 증감 연산을 통해서 핀을 꽂았다 뽑았다 할 수 있게 했다. 실제로 필자의 소스코드에서 모든 페이지의 접근은 다음과 같은 형식으로 이루어진다.

<script src="https://gist.github.com/pch6828/b51d02ace894a91b7893ac8fa8576290.js"></script>

## Next Step
1. B+Tree Deletion<br>
현재 Insertion과 Find까지는 구현을 끝냈다. 이제 Deletion만 구현하면 첫번째 Index Structure의 구현이 끝나는 것이다. Deletion은 Delayed Merge를 적용해서 연산의 overhead를 최대한 줄일 것이다.
1. Next Index : Skip List<br>
B+Tree의 구현으로 상당히 머리가 아팠기 때문에(물론 정확히는 C++ migration으로 인해 고생한 것이지만) 다음 Index는 조금 더 간단한 Skip List로 구현할 생각이다. 다만, 이 구조도 4kb의 페이지에 맞게 구현해야 하기 때문에 어느정도 고민이 필요할 것이다.

첫 단추는 나름 잘 끼운듯 하다.<br>
그나마 한번 해본거라서 4일만에 끝낸 거 같긴 하지만(이걸 2019년엔 대체 어떻게 했던 거지...), 1달 반에서 4일로 줄인 거면 그래도 비약적인 실력 상승이라고 생각한다. 다음 포스팅은 B+Tree 구조를 모두 완성하고 돌아오겠다.