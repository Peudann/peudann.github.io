---
title: "25.04.03 / 벡터의 정규화(Vector Normalization)"
excerpt: ""

categories:
  - Programming
  - Game Development
tags:
  - []

toc: true
toc_sticky: true

date: 2025-04-03
last_modified_at: 2025-04-29
use_math: true
---

게임 엔진에서 벡터의 정규화란 주어진 벡터의 방향은 그대로 유지하면서 길이를 1로 만드는 과정을 의미한다. 벡터를 정규화하는 이유는 방향 정보만 필요할 때 크기의 영향을 없애기 위해서이다. 정규화된 벡터는 길이가 1이기 때문에, 오직 방향만을 나타내며, 이로 인해 다음과 같은 장점이 생긴다.

- 일관된 방향 계산
  - 이동, 회전 또는 물리 계산에서 벡터의 크기가 계산에 영향을 주지 않고, 순수한 방향 정보만 사용할 수 있다.
- 스케일에 의한 오류 방지
  - 서로 다른 크기의 벡터들을 비교하거나 결합할 때 크기 차이로 인한 부정확한 결과를 방지할 수 있다.
- 간단한 수학적 연산
  - 내적(dot product)이나 각도 계산 같은 연산을 할 때, 벡터의 크기가 1이면 계산이 단순해진다.

결국, 정규화는 게임 개발이나 물리 시뮬레이션 등에서 방향 제어를 보다 명확하고 안정적으로 처리하기 위한 기법인 것이다.

그래서 벡터의 정규화를 어떻게 하냐하면.

---

**1\. 벡터의 크기(길이)를 계산**

먼저 벡터의 크기를 계산한다. 예를 들어 3차원 벡터 $ \mathbf{v} = (x, y, z)$ 의 크기는

$ \\| \\mathbf{v} \\| = \\sqrt{x^2 + y^2 + z^2} $ 로 구할 수 있다.

**2\. 각 성분 나누기**

계산한 크기로 각 성분을 나누면, 결과 벡터

$ \\mathbf{v\_{norm}} = \\left(\\frac{x}{\\| \\mathbf{v} \\|}, \\frac{y}{\\| \\mathbf{v} \\|}, \\frac{z}{\\| \\mathbf{v} \\|}\\right) $ 는 크기가 1인 단위 벡터(Unit Vector)가 된다.

**3\. 유니티에서의 사용**

유니티에서는 벡터 클래스(Vector3 등)에 이미 정규화 기능이 내장되어 있어서, 예를 들어 Vector3.normalized를 사용하면 쉽게 정규화된 벡터를 얻을 수 있다. 이는 물체의 이동 방향 설정, 회전 계산, 물리 엔진의 충돌 처리 등 다양한 상황에서 유용하게 사용된다.

---

정리하자면, 벡터의 정규화는 벡터의 방향은 그대로 유지하면서 크기를 1로 만드는 과정으로, 게임 엔진에서는 주로 방향만 중요할 때 사용하게 된다.
