---
title: "25.03.21 / 멤버 접근 지정자(Access Modifiers)"
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

C#에서는 클래스 멤버(필드, 메소드, 프로퍼티, 이벤트 등)를 선언할 때 멤버의 접근 범위를 성정하기 위해 접근 지정자(Access Modifiers)를 사용한다. 접근 지정자를 통해 외부에서 어떤 멤버에 접근할 수 있는지, 그리고 어느 범위에서만 접근이 제한되는지를 결정할 수 있다. C#에서 가장 많이 쓰이는 접근 지정자를 알아보자.

- 1\. public
  - 모든 코드에서 접근 가능
  - 클래스 외부 및 다른 어셈블리(프로젝트)에 있는 코드에서도 자유롭게 접근할 수 있다.
  - 라이브러리(API) 형태로 제공하는 기능이나, 공개되어야 하는 멤버에 주로 사용된다.

```csharp
public class MyClass
{
    public int PublicField;
    public void PublicMethod() { ... }
}
```

- 2\. private
  - 현재 클래스 내에서만 접근 가능
  - 접근 지정자를 명시하지 않으면 기본적으로 설정되지만, 대부분의 경우 명시적으로 private를 붙여 사용한다.
  - 캡슐화(Encapsulation)를 위해 가장 많이 사용되는 접근 지정자이다.
  - 클래스 외부에서는 보이지 않아야 하는 데이터나 로직이 있을 때 사용한다.

```csharp
public class MyClass
{
    private int _privateField;

    private void PrivateMethod()
    {
        // 같은 클래스 내에서만 호출 가능
    }
}
```

- 3\. protected
  - 현재 클래스 혹은 파생 클래스(상속받은 클래스) 내에서만 접근 가능
  - 상속 개념과 함께 자주 사용된다.
  - 클래스 외부에서는 사용할 수 없지만, 상속받은 자식 클래스에서는 부모 클래스의 protected 멤버를 사용할 수 있다.

```csharp
public class BaseClass
{
    protected int ProtectedField;

    protected void ProtectedMethod() { ... }
}

public class DerivedClass : BaseClass
{
    public void SomeMethod()
    {
        // BaseClass에서 상속받은 protected 멤버에 접근 가능
        ProtectedField = 10;
        ProtectedMethod();
    }
}
```

- 4\. internal
  - 같은 어셈블리(프로젝트) 내에서만 접근 가능
  - 어셈블리(빌드 결과물, 예를 들어 .dll, .exe 등) 단위로 접근 범위를 제한한다.
  - 같은 프로젝트 안에서만 접근할 수 있고, 외부에서 참조하는 다른 프로젝트에선 접근이 불가능하다.
  - 보통 라이브러리를 만들 때, 라이브러리 내부적으로만 쓰이는 멤버나 클래스에 많이 사용한다.

```csharp
internal class InternalClass
{
    internal int InternalField;

    internal void InternalMethod() { ... }
}
```

- 5\. protected internal
  - 같은 어셈블리 내에서 접근하거나, 다른 어셈블리에 있더라도 상속받은 클래스에서 접근 가능
  - protected와 internal을 합친 개념이라고 생각하면 된다.
  - 기본적으로 동일 어셈블리 내에서는 public처럼 동작하지만, 어셈블리가 다를 경우에는 상속 관계에서만 접근할 수 있다.

```csharp
public class BaseClass
{
    protected internal void ProtectedInternalMethod() { ... }
}

// 같은 어셈블리 내의 다른 클래스
class SameAssemblyClass
{
    void Test()
    {
        BaseClass baseObj = new BaseClass();
        baseObj.ProtectedInternalMethod(); // 접근 가능
    }
}

// 다른 어셈블리에 있는 파생 클래스
public class DerivedClass : BaseClass
{
    void Test()
    {
        ProtectedInternalMethod(); // 상속받았기 때문에 접근 가능
    }
}

// 다른 어셈블리에 있지만 상속 관계가 아닌 클래스
public class AnotherClass
{
    void Test()
    {
        BaseClass baseObj = new BaseClass();
        // baseObj.ProtectedInternalMethod(); // 에러 - 직접 접근 불가
    }
}
```

- 6\. private protected
  - 같은 클래스 내에서 혹은 파생 클래스(상속 관계) 내에서만 접근 가능하며, 해당 파생 클래스도 같은 어셈블리여야 접근 가능
  - C# 7.2부터 도입된 지정자이다.
  - 내부적으로만 사용하면서도 상속받은 클래스 내에서 접근할 수 있도록 하고 싶을 때, 그런데 외부 어셈블리에서는 접근할 수 없게 하고 싶을 때 사용한다.
  - 한마디로 private + protected + internal이라고 볼 수 있다.
  - 단, protected internal과는 미세하게 다르다. protected internal은 동일 어셈블리에서도 접근이 거의 public 수준으로 열려 있고, 다른 어셈블리에서도 상속 관계라면 접근이 가능한 반면, private protected는 동일 어셈블리 내에서 상속 관계일 때만 접근이 가능하다.

```csharp
public class BaseClass
{
    private protected void PrivateProtectedMethod() { ... }
}

public class DerivedClass : BaseClass
{
    void Test()
    {
        // 같은 어셈블리 내에서 상속받았기 때문에 접근 가능
        PrivateProtectedMethod();
    }
}

// 같은 어셈블리에 있지만 상속 관계가 아닌 경우
public class SameAssemblyClass
{
    void Test()
    {
        BaseClass baseObj = new BaseClass();
        // baseObj.PrivateProtectedMethod(); // 에러 - 상속받은 클래스가 아님
    }
}

// 다른 어셈블리에 있으면서 상속받은 경우
public class DerivedClassInAnotherAssembly : BaseClass
{
    void Test()
    {
        // base.PrivateProtectedMethod(); // 에러 - 다른 어셈블리라서 접근 불가
    }
}
```

C#에서 멤버의 접근성을 정하는 것은 캡슐화와 정보 은닉, 그리고 코드 구조를 명확하게 유지하는 데 있어 매우 중요하다. 작성하는 클래스나 라이브러리의 의도와 구조에 따라 적절한 접근 지정자를 사용해 관리하는 것이 좋겠다.
