---
title: "25.03.24 / getter와 setter는 왜 쓰는걸까"
excerpt: ""

categories:
  - Programming
  - C#
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-24
last_modified_at: 2025-04-29
---

우선 예시 하나 보고가자.

```csharp
using System;

public class Person
{
    // 필드는 기본적으로 private로 선언
    private string name;
    private int age;

    // Name 속성: 외부에서 읽고 쓰기가 가능
    public string Name
    {
        get
        {
            return name;
        }
        set
        {
            // 원한다면 여기서 유효성 검사나 로그 처리를 추가할 수 있음
            name = value;
        }
    }

    // Age 속성: 외부에서 읽고 쓸 때 검증 로직을 수행
    public int Age
    {
        get
        {
            return age;
        }
        set
        {
            // 나이는 음수가 될 수 없으므로 유효성 검사
            if (value < 0)
            {
                throw new ArgumentException("Age cannot be negative.");
            }
            age = value;
        }
    }
}

// 사용 예시
public class Program
{
    public static void Main()
    {
        Person person = new Person();

        // setter를 통해 값 지정
        person.Name = "Alice";
        person.Age = 30;

        // getter로 값 읽기
        Console.WriteLine(person.Name); // 출력: Alice
        Console.WriteLine(person.Age);  // 출력: 30

        // 잘못된 값은 예외 발생
        try
        {
            person.Age = -1;
        }
        catch (ArgumentException e)
        {
            Console.WriteLine(e.Message); // 출력: Age cannot be negative.
        }
    }
}
```

이 예시에선 Name과 Age를 각각 public 속성으로 제공하면서, 내부 필드(name, age)에 간접적으로 접근하도록 한다.

그냥 내부 필드를 public으로 선언하면 되지, 왜 귀찮게 따로 public 속성을 가진 Name과 Age를 만들었을까?

C#에서 getter, setter(속성 접근자)를 사용하는 주된 이유는 캡슐화(Encapsulation)를 통해 객체 내부의 상태를 보호하고, 외부에서의 접근과 수정 과정을 제어하기 위해서이다. 구체적으로 다음과 같은 장점이 있다.

- 1\. 내부 구현 숨기기(정보 은닉)
  - 클래스 내부의 필드는 객체를 구성하는 중요한 데이터이다. 이 필드에 바로 접근이 가능하다면, 어디에서나 값을 마음대로 바꿀 수 있어 의도치 않은 동작이나 오류가 발생할 수 있다.
  - getter와 setter로 필드를 간접적으로 접근하게 함으로써, 외부에서는 객체 내부 구현 방법을 알 필요 없이 필요한 인터페이스만 사용하면 된다. 이는 코드의 재사용성 및 유지보수를 용이하게 한다.
- 2\. 유효성 검사(Validation)
  - 데이터를 설정(set)하는 시점에 해당 데이터가 유효한지 검사할 수 있다. 위의 예시를 보면 나이(age) 필드에 음수가 들어가는 방지하기 위해 setter 메소드 안에서 값이 0보다 큰지 확인 후에만 필드를 갱신할 수 있게 만들었다.
  - 이렇게 하면 데이터 무결성을 보장할 수 있고, 잘못된 데이터로 인해 발생할 수 있는 오류를 사전에 방지할 수 있다.
- 3\. 접근 권한 제어(Access Control)
  - C#의 속성은 접근 지정자를 통해 getter와 setter의 접근 범위를 독립적으로 설정할 수 있다. 예를 들어 public get, private set 형태로 읽기는 공개하지만 쓰기는 제한하는 식의 제어가 가능하다.
  - 이런 방식으로 세밀하게 접근 권한을 지정하면, 클래스 사용자가 데이터를 함부러 수정하는 것을 막고, 필요한 경우에만 수정할 수있도록 할 수 있다.
- 4\. 추가 로직 삽입 용이
  - 추후 유지보수 과정에서 값을 반환할 때(getter)나 변경할 때(setter), 별도의 로직이 필요한 경우가 있을 수 있다. 예를 들어 setter에 로깅, 이벤트 발생, 계산 로직 등을 추가해야 한다면, 이미 정의된 getter, setter 속성 안에 구현하기 쉽다.
  - 필드에 직접 접근했다면, 해당 로직을 추가하기가 어렵거나, 코드 전체를 손봐야 해서 유지보수가 어려워진다.
- 5\. 객체 지향 원칙 준수
  - 캡슐화, 정보 은닉 같은 객체 지향 프로그래밍(OOP)의 기본 원칙을 충실히 이행하게 된다. C# 역시 OOP 언어이므로 이러한 원칙에 맞게 getter/setter를 사용하는 것이 일반적이다.

정리하자면, C#에서 getter와 setter는 어떻게 데이터를 읽고 쓸지를 직접 제어하기 위한 안전 장치라고 할 수 있다. 이를 통해 외부와 내부를 깔끔하게 분리하고, 언제든지 필요한 로직을 추가하거나, 오류를 예방하며, 나아가 유지보수를 수월하게 하는 장점이 있다.
