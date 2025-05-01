---
title: "25.03.26 / 상속(Inheritance)"
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

C#에서 상속(Inheritance)은 기존에 만들어진 클래스(부모 클래스, 기반 클래스)의 속성(필드, 프로퍼티)과 기능(메소드)을 자식 클래스(파생 클래스)가 물려받아 사용할 수 있게 하는 객체 지향 프로그래밍의 중요한 개념이다. 상속을 사용하면 코드를 재사용하고, 유지보수를 쉽게 하며, 확장성을 높일 수 있다.

C++에선 기반 클래스를 Parent, 파생 클래스를 Child라고 칭하고 C#에선 각각 Base, Derived라고 칭한다. 이번 글에선 부모 클래스, 자식 클래스로 쓰겠다.

```csharp
class Parent
{
    protected int baseInt = 0;

    // 생성자
    public Parent()
    {
        Console.WriteLine("Parent Constructor Call!");
    }

    // 소멸자
    ~Parent()
    {
        COnsole.WriteLine("Parent Destructor Call!");
    }

    // 메소드
    public void Example()
    {
        Console.WriteLine("Parent Example Call!");
    }
}

class Child : Parent
{
    private int derivedInt = 0;
    baseInt = 100;	// 부모 클래스의 protected 변수 수정 가능

    // 생성자
    public Child()
    {
        Console.WriteLine("Child Constructor Call!");
    }

    // 소멸자
    ~Child()
    {
        Console.WriteLine("Child Destructor Call!");
    }


    // 오버라이드 된 메소드
    public override void Example()
    {
        Console.WriteLine ("Child Example Call!");
    }
}
```

```csharp
class Program
{
    private static void InheritanceExample()
    {
        Parent parent = new Parent();
        parent.Example();

        Child child = new Child();
        child.Example();
    }

    private static void Main()
    {
        InheritanceExample();

        GC.Collect()	// 가비지 컬렉터 호출
        Console.ReadLine();
    }
```

위의 코드에서 알 수 있듯 :를 사용해 자식 클래스 : 부모 클래스 이런 식으로 선언해 상속 관계를 만들 수 있다. 이렇게 만들면 자식 클래스는 부모 클래스의 속성과 기능들을 가지게 된다(baseInt와 Example() 함수를 가진다). 일단 실행 결과를 보고 더 이어가자.

```text
Parent Constructor Call!
Parent Example Call!

Parent Constructor Call!
Child Constructor Call!
Child Example Call!

Child Destructor Call!
Parent Destructor Call!
Parent Destructor Call!
```

실행 결과를 보면 부모 클래스를 사용할 땐 그냥 클래스가 쓰이듯 생성자 - 함수 순으로 호출이 된다. 그런데 자식 클래스에선 부모 클래스 생성자 - 자식 클래스 생성자 - 함수 순으로 호출이 되는 걸 볼 수 있다. 이렇게 자식 클래스를 호출하면 부모 클래스의 생성자가 먼저 호출되고 자식 클래스의 생성자가 호출된다. 함수 부분에선 부모 클래스의 함수가 호출되지 않고 자식 클래스의 함수만 호출되는데, 이는 다음 글에서 오버라이드(Override)를 다루며 설명하겠다.

자 그럼, 소멸자를 한 번 봐보자. 생성의 역순으로 자식 클래스의 소멸자부터 호출되고 부모 클래스의 소멸자가 호출된다. 단순히 생각하면 그냥 생성의 역순이구나 할 수 있지만, 이유가 무엇일까?

이는 자식 클래스가 부모 클래스의 리소스에 의존하기 때문이다. 만약 자식 클래스의 리소스 정리에 부모 클래스의 멤버나 메소드가 필요하다고 생각해보자. 예를 들어 자식 클래스에서 부모 클래스에 정의된 메소드를 호출해서 로그를 남기거나, 부모 클래스의 필드 상태를 확인하는 경우, 만약 부모 클래스의 소멸자가 먼저 호출되어 부모가 가진 자원이 해제되어 버리면, 자식 클래스는 안전하게 기능을 수행할 수 없어진다. 쉽게 말해 자식이 부모의 리소스에 의존할 수 있으므로, 자식의 정리를 먼저 수행해 부모의 리소스가 아직 유효한 상태에서 자식 정리가 끝나도록 보장하는 것이다.

그럼 상속을 하면 어떤 장점이 있길래 상속을 사용하는 것일까? 한 번 알아보도록 하자.

- 1\. 코드 재사용 용이
  - 공통 로직을 부모 클래스에 작성해두면, 자식 클래스들은 해당 로직을 다시 구현할 필요가 없다.
- 2\. 유지보수 용이
  - 공통 로직을 한 곳(부모 클래스)에 모아둘 수 있으므로 수정 및 유지보수가 쉬워진다.
- 3\. 확장성
  - 새로운 기능이 필요할 때, 이미 만들어진 클래스를 상속받아 확장할 수 있다.

예를 들어보자면 게임 캐릭터, 몬스터, 보스 등을 만들 때 공통으로 가질 요소(예를 들어 체력, 공격력, 방어력)를 부모 클래스에 미리 선언해놓고 상속받으면 객체 하나를 만들 때마다 따로 작성할 필요 없이 이미 만들어진 클래스를 상속받아 사용할 수 있는 것이다.

상속을 사용할 땐 주의해야 할 점이 몇 가지 있다.

- 1\. 단일 상속 원칙
  - C#에서는 클래스에 대해 단일 상속만 지원한다. 즉, 한 자식 클래스가 동시에 여러 클래스를 상속받을 수 없다(인터페이스는 다중 구현이 가능하지만 이건 나중에 인터페이스를 다룰 때 알아보자).
- 2\. 접근 제한자
  - public, protected, private 같은 접근 제한자에 따라 부모 클래스의 멤버를 사용할 수 있는 범위가 달라진다.
  - 부모 클래스에서 private로 선언된 멤버는 자식 클래스에서 직접 접근할 수 없다.
  - 부모 클래스에서 protected로 선언된 멤버는 자식 클래스에서만 접근할 수 있다(다른 외부 클래스는 접근 불가).
- 3\. sealed 클래스
  - sealed 키워드를 사용하면 해당 클래스를 더 이상 상속할 수 없게 만들 수도 있다.

정리하자면, C#에서 상속은 코드 재사용, 유지보수 편의성, 확장성을 향상시키는 핵심적인 객체 지향 프로그래밍의 개념이다. : 기호 뒤에 부모 클래스를 명시하여 상속 관계를 나타내며, 자식 클래스는 부모 클래스의 멤버를 물려받을 수 있고 필요한 기능을 추가로 정의할 수도. 있다. 단일 상속 원칙, 접근 제한자, sealed 키워드 등을 잘 이해하고 사용해 깔끔하고 유지보수하기 쉬운 코드를 작성해보자.
