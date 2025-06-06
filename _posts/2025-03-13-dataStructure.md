---
title: "25.03.13 / 자료구조(Data Structure)"
excerpt: ""

categories:
  - Programming
  - Data Structure and Algorithm
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-13
last_modified_at: 2025-04-25
---

자료구조는 컴퓨터가 데이터를 효율적으로 다룰 수 있게 도와주는 데이터 보관 방법과 데이터에 관한 연산의 총체를 뜻한다. 예를 들어 우리가 자주 사용하는 int도 자료 구조라 볼 수 있다. int는 32비트 메모리 공간 안에 수를 할당하되 첫 비트를 부호 표현에 사용하는 등 '보관 방법'을 정의하고 있고, 덧셈/뺄셈/나눗셈/곱셈 등의 다양한 '연산' 또한 정의하고 있다.

자료구조는 단순 자료구조(Primitive Data Structure)와 복합 자료구조(Non-Primitive Data Structure)로 나누어진다. 단순 자료구조는 위에서 말한 int를 포함해 프로그래밍 언어에서 통상적으로 제공하는 기본 데이터 형식을 말한다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250313_dataStructure/01.png" alt="01" style="max-width: 100%;" />
</div>

복합 자료구조는 다시 선형 자료구조(Linear Data Structure)와 비선형 자료구조(Non-Linear Data Structure)로 나눠진다. 선형 자료구조는 데이터 요소를 순차적으로 연결하는 자료구조로, 구현하기 쉽고 사용하기도 쉽다. 우리에게 친숙한 배열과 링크드 리스트, 스택, 큐 등이 여기에 해당한다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250313_dataStructure/02.png" alt="02" style="max-width: 100%;" />
</div>

비선형 자료구조는 선형 자료구조와 달리 데이터 요소를 비순차적으로 연결한다. 다음과 같이 한 데이터 요소에서 여러 데이터 요소로 연결되기도 하고, 여러 데이터 요소가 하나의 데이터 요소로 연결되기도 한다. 트리와 그래프가 여기에 해당한다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
   <img src="/assets/img/250313_dataStructure/03.png" alt="03" style="max-width: 100%;" />
</div>

자료구조와 관련하여 알아두면 좋은 개념이 한 가지 더 있다. 바로 추상 데이터 형식, 다시 말해 ADT(Abstract Data Types)이다. 이것은 자료구조의 동작 방법을 표현하는 데이터 형식으로, 정리하자면 자료구조가 갖춰야 할 일련의 연산이라고 할 수 있다.

리스트를 예로 들어보자. 리스트는 데이터에 순차적으로 접근해서 그 데이터를 다룰 수 있도록 여러가지 기능을 제공해야 한다. 목록의 특정 위치에 있는 노드에 접근(Get)하거나, 목록의 마지막에 데이터를 추가(Append)하거나, 목록의 중간에 삽입(Insert)하거나, 삭제(Remove)하는 기능을 제공한다.

이렇게 ADT가 청사진을 제시하면 자료구조는 이를 구현한다. 배열로 리스트를 구현한다고 가정하면, 배열의 길이가 곧 리스트이 길이가 되고 배열의 첫 요소는 시작 노드, 배열의 마지막 요소는 마지막 노드가 된다. 그리고 현재 다루고 있는 요소의 인덱스가 현재 노드가 된다. 여기에 탐색/추가/삽입/수정/삭제와 같은 기능을 구현하면 하나의 자료구조가 완성된다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250313_dataStructure/03.png" alt="03" style="max-width: 100%;" />
</div>

그렇다면 자료구조를 왜 공부해야할까?

첫째, 자료구조의 내부를 이해하면 라이브러리에서 엉뚱한 자료구조를 선택하는 일을 피할 수 있다. 동일한 ADT를 사용하더라도 자료구조에 따라 애플리케이션 성능이 크게 달라질 수 있다. 가령 네트워크 애플리케이션의 입출력 버퍼에는 링크드 큐보다 환형 큐를 사용하는 것이 처리 속도면에서 유리하고, 메모리 효율이 더 중요한 애플리케이션에서는 링크드 큐가 환형 큐보다 나은 선택일 수 있다. 자료구조 지식이 있는 프로그래머는 그 이유를 이해하고 라이브러리 문서를 통해 적절한 자료구조를 선택할 수 있다.

둘째, 자료구조는 알고리즘이 데이터를 효율적으로 사용할 수 있게 도와주는 핵심 부품 역할을 하기 때문이다. 다시 말해, 자료구조를 모르면 알고리즘을 공부하고 구현하는 데 어려움이 생긴다.

본 포스팅의 내용과 그림은 한빛미디어의 '이것이 자료구조+알고리즘이다 with C언어, 저자 박상현'에서 발췌했다. 본 포스팅은 어떤 방식으로도 블로그 소유자의 금전적 이익을 생성하지 않으며, 교육을 목적으로 써진 글임을 알린다.
