---
title: "25.03.21 / 값 타입(Value Type)과 참조 타입(Reference Type)"
excerpt: ""

categories:
  - Programming
  - C#
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-21
last_modified_at: 2025-04-29
---

C#에서 값 타입(Value Type)과 참조 타입(Reference Type)은 메모리에 저장되는 방식과 복사(할당) 시 동작 방식에서 큰 차이가 있다.

값 타입은 스택(Stack) 영역에 할당된다. 값을 복사해서 전달하므로, 한 변수를 다른 변수에 대입할 때 실제 값이 복사된다. 변경사항이 다른 변수에는 영향을 주지 않는다(얕은 복사가 아닌 진짜 값 복사). 대표적인 예시로 기본 자료형(int, float, bool 등), struct, enum 등이 있다.

참조 타입은 힙(Heap) 영역에 실제 데이터가 할당되고, 스택에는 힙을 가리키는 참조(주소)가 저장된다. 참조를 통해 값을 공유하므로, 한 변수를 다른 변수에 대입할 때 주소(참조)만 복사된다. 변경 사항이 참조된 모든 변수에 영향을 준다. 대표적인 예시로 class, interface, string, 배열, 델리게이트 등이 있다.

예시를 살펴보자.

```csharp
using System;

public class Program
{
  public static void Main()
  {
    int a = 10;
    int b = a;  // a에 담긴 '10'이 b에 복사됨

    b = 20;     // b값만 변경
    Console.WriteLine($"a: {a}, b: {b}");
    // 출력 결과: a: 10, b: 20
    // b를 변경해도, a에는 영향이 없음
  }
}
```

값 타입의 예시다. a와 b는 스택에 각각 별도의 공간을 가지고, 10과 20이라는 실제 값을 별도로 저장한다. 따라서 b = 20을 할당해도 a의 값은 그대로 유지된다.

```csharp
using System;

public class SampleClass
{
  public int Value;
}

public class Program
{
  public static void Main()
  {
    SampleClass x = new SampleClass();
    x.Value = 10;

    // x 자체가 아니라, x가 가리키는 힙의 주소를 y에 복사
    SampleClass y = x;

    y.Value = 20;
    Console.WriteLine($"x.Value: {x.Value}, y.Value: {y.Value}");
    // 출력 결과: x.Value: 20, y.Value: 20
    // y에서 변경한 값이 x에도 반영됨
  }
}
```

new SampleClass()로 생성된 객체는 힙에 저장된다. x는 힙에 있는 SampleClass 객체를 참조하는 주소를 갖고 있다. y = x;는 x의 참조(주소)값을 y에도 똑같이 복사하므로, x와 y는 같은 객체를 가리키게 된다. 따라서 y.Value = 20;으로 값을 바꾸면, 결국 같은 힙 객체를 수정하는 것이므로 x.Value도 20이 된다.
