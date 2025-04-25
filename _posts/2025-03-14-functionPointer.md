---
title: "25.03.14 / 함수 포인터(Function Pointer)"
excerpt: ""

categories:
  - Programming
  - C
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-14
last_modified_at: 2025-04-25
---

```c
#include <stdio.h>


// 함수 포인터(Function Pointer)
int (*pFunc)(int, int) = NULL;

int Sum(int _lhs, int _rhs) { return _lhs + _rhs; }
int Sub(int _lhs, int _rhs) { return _lhs - _rhs; }
int Mul(int _lhs, int _rhs) { return _lhs * _rhs; }
int Div(int _lhs, int _rhs) {
    if (_rhs == 0) _rhs = 1;
    return _lhs / _rhs;
}


void main()
{
    pFunc = Sum;
    printf("3 + 5 = %d\n", pFunc(3, 5));

    pFunc = Sub;
    printf("3 - 5 = %d\n", pFunc(3, 5));

    pFunc = Mul;
    printf("3 * 5 = %d\n", pFunc(3, 5));

    pFunc = Div;
    printf("5 / 2 = %d\n", pFunc(5, 2));
}
```

```text
실행 결과
3 + 5 = 8
3 - 5 = -2
3 * 5 = 15
5 / 2 = 2
```

함수 포인터를 선언하고 사용한 예시이다.

함수 포인터는 함수 자체의 주소를 저장하는 포인터 변수이다. 일반적으로 포인터라고 하면 어떤 변수나 배열 등의 메모리주소를 가리키는 것을 떠올리는데, 함수 포인터는 함수가 시작되는 메모리 주소를 가리키는 포인터라고 이해하면 편하다.

```c
int (*pFunc)(int, int) = NULL;
```

괄호 안에 \*pFunc를 둘러싸고 그 앞에 int를 적어줌으로써, 매개변수 두개를 받아 int를 반환하는 함수의 주소를 저장하는 변수임을 나타낸다. 따라서 pFunc는 int (int, int) 형태의 함수 주소를 저장할 수 있게 된다.

```c
int Sum(int _lhs, int _rhs) { return _lhs + _rhs; }
int Sub(int _lhs, int _rhs) { return _lhs - _rhs; }
int Mul(int _lhs, int _rhs) { return _lhs * _rhs; }
int Div(int _lhs, int _rhs) {
    if (_rhs == 0) _rhs = 1;
    return _lhs / _rhs;
}


void main()
{
    pFunc = Sum;
    printf("3 + 5 = %d\n", pFunc(3, 5));

    pFunc = Sub;
    printf("3 - 5 = %d\n", pFunc(3, 5));

    pFunc = Mul;
    printf("3 * 5 = %d\n", pFunc(3, 5));

    pFunc = Div;
    printf("5 / 2 = %d\n", pFunc(5, 2));
}
```

첫 번째로 pFunc = Sum; 에서 Sum 함수의 주소를 pFunc에 대입한다. 그래서 pFunc(3, 5)를 호출하면 Sum(3, 5)가 호출되어 결과를 반환한다. 두 번째는 pFunc = Sub; 에서 Sub 함수의 주소를 pFunc에 대입한다. 그래서 pFunc(3, 5)를 호출하면 Sub(3, 5)가 호출되어 결과를 반환한다. 그 이후로도 똑같이 pFunc를 호출하면 대입된 함수가 호출되어 계산을 진행한다.

그럼 "그냥 함수를 쓰면 되지 왜 함수 포인터를 사용하냐?"라고 생각할 수 있다. 이유는 함수 포인터가 여러가지 장점이 있기 때문이다.

우선 동적 호출(Dynamic Call)이 가능하다. 런타임에 어떤 함수를 호출할지 결정해야 할 때, 함수 포인터가 유용하게 쓰인다. 예를 들어, 메뉴나 이벤트 처리 로직에서 선택된 항목에 따라 다른 함수를 호출 해야 할 때, 여러 if-else문 대신 함수 포인터를 배열 등으로 관리하면서 깔끔하고 효율적으로 구현할 수 있다.

그 다음으로 콜백(Callback) 구현이 가능하다. 특정 동작이 끝난 뒤에 "어떤 함수를 불러야 하는가?"를 미리 정하지 않고, 실행 중에 등록하는 방식을 말한다. C언어에서는 콜백 함수를 구현하기 위해 함수 포인터를 사용한다.

마지막으로 유연성과 확장성이 뛰어나다. 같은 형식(매개변수, 반환형)의 함수가 많을 때, 공통된 인터페이스 포인터 하나만 두고 계속해서 다른 함수를 연결해 쓸 수 있어 코드 구조가 유연해진다.
