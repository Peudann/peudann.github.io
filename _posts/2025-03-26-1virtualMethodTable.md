---
title: "25.03.26 / 가상 함수 테이블(Virtual Method Table)"
excerpt: ""

categories:
  - Programming
  - C#
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-26
last_modified_at: 2025-04-29
---

지난 글에서 virtual과 override를 다뤘었다. 이들이 실제로 동적하기 위해 내부적으로 사용되는 "가상 함수 테이블"이라는 게 있는데, 이것의 개념에 대해 설명해보겠다.

우선 virtual 키워드는 C#에서 메소드를 virtual로 선언해, 해당 메소드가 자식 클래스에서 재정의(Override) 될 수 있음을 의미한다. 부모 클래스에는 공통적인 기본 동작을 정의해 두고, 자식 클래스에서는 이 동작을 필요에 따라 다른 구현으로 재정의할 수 있도록 하는 것이 주된 목적이다.

예시 코드 하나를 살펴보자.

```csharp
public class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("동물이 소리를 냅니다.");
    }
}
```

Animal 클래스의 Speak() 메소드를 virtual로 선언했다. 이제 Animal을 상속받는 어떤 클래스라도 Speak()을 오버라이드해 자체 동작을 가질 수 있게 된다.

override 키워드는 자식 클래스에서 부모 클래스의 virtual 메소드(또는 abstract 메소드)를 재정의할 대 사용하는 키워드이다. 오버라이드 할 땐 지켜야 할 규칙이 몇 가지 있다.

- 1\. 반드시 부모 클래스에 virtual 혹은 abstract로 선언된 메소드가 있어야 한다.
- 2\. 반환형, 메소드 이름, 매개변수 시그니처가 부모 클래스의 메소드와 일치해야 한다.
- 3\. 접근 제한자(public, protected 등)는 부모 메소드와 같거나 더 넓을 수 있지만, 더 좁아질 수는 없다.

```csharp
public class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("멍멍!");
    }
}
```

이제 Dog 클래스는 Animal 클래스이 Speak() 메소드를 재정의하여, 독자적인 내용을 가진다.

virtual과 override에 대해 간단히 다시 알아봤으니, 본격적으로 가상 함수 테이블을 알아보자.

가상 함수 테이블은 C#(정확히는 .NET CLR)에서, virtual 메소드를 사용하면 동적 디스패치(Dynamic Dispatch)라고 불리는 방식을 통해 어떤 클래스의 메소드를 호출할지를 런타임에 결정하는데, 이 때 내부적으로 사용되는 구조가 가상 함수 테이블이다.

각 클래스(혹은 클래스 계층 구조)는 가상 메소드 목록과 해당 메소드를 어디서 어떻게 구현했는지(실제 함수 포인터)를 테이블(표) 형태로 관리한다. 객체를 호출할 때, 런타임은 객체의 타입 정보를 보고 해당 타입의 가상 함수 테이블에서 올바른 메소드 구현을 찾아 실행한다.

이런 특징에서 오는 장점이 여러 가지 있는데, 우선 다형성(Polymorphism)을 지원할 수 있게 된다. 부모 클래스 타입으로 선언된 변수라도, 실제로 담긴 객체 타입이 자식 클래스라면 자식 클래스의 오버라이드된 메소드를 호출할 수 있게 된다. 이로 인해 코드 구조를 유연하게 설계할 수 있으며, 유지보수가 편해진다.

가상 함수 테이블에 대한 참고 사항이 몇 가지 있다.

- C# 코드를 작성할 때 사용자가 직접 가상 함수 테이블을 다루지 않는다. CLR이 자동으로 관리한다.
- 복잡한 상속 구조일수록 많은 가상 메소드가 생길 수 있는데, 이 경우 런타임에서 가상 함수 테이블을 참고하여 올바른 구현 메소드를 호출하게 된다.
- 성능 측면에서는 매번 호출 시 메소드를 직접 가리키는 것보다 약간의 오버헤드가 발생하지만, 일반적인 애플리케이션 개발에서 크게 문제가 될 정도는 아니다.

정리하자면 C#의 virtual/override 키워드를 통해 객체지향 프로그래밍의 다형성을 손쉽게 구현할 수 있다. 내부적으로는 CLR이 가상 함수 테이블을 관리하여, 런타임에 올바른 메소드 구현을 찾아 실행하게 된다. 이를 통해 부모 클래스 타입 변수를 사용해도, 실제 객체가 자식 클래스라면 자식 클래스에서 오버라이드한 메소드를 문제없이 호출할 수 있게 된다.
