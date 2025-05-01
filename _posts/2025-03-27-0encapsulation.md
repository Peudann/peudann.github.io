---
title: "25.03.27 / 캡슐화(Encapsulation)"
excerpt: ""

categories:
  - Programming
  - C#
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-27
last_modified_at: 2025-04-29
---

캡슐화는 객체 지향 프로그래밍의 핵심 원칙 중 하나로, 객체의 상태(필드/프로퍼티)나 내부 동작(메소드)을 외부에서 직접 접근하거나 수정하지 못하도록 감추고, 필요한 경우에만 지정된 메소드나 프로퍼티를 통해 접근하도록 만드는 것을 의미한다. 이를 통해 코드의 유지 보수성과 안정성을 높이고, 변경에 유연하게 대처할 수 있다.

예시를 하나 보자.

```csharp
public class BankAccount
{
    private decimal balance; // 외부에서 직접 접근하지 못하도록 private

    // balance 값을 읽고 설정하기 위해 공개해놓은 프로퍼티
    public decimal Balance
    {
        get { return balance; }
        private set { balance = value; }
    }

    public BankAccount(decimal initialBalance)
    {
        Balance = initialBalance;
    }

    // 은행 계좌에 돈을 입금하는 메서드
    public void Deposit(decimal amount)
    {
        if(amount > 0)
        {
            Balance += amount;
        }
    }

    // 은행 계좌에서 돈을 출금하는 메서드
    public void Withdraw(decimal amount)
    {
        if(amount > 0 && Balance >= amount)
        {
            Balance -= amount;
        }
    }
}
```

위 예제에서 balance 필드는 private로 선언되어 외부에서 직접 전근할 수 없으며, 오직 Deposit()과 Withdraw() 메소드 같은 지정된 메소드를 통해서만 값이 변경되도록 설계되었다. 이게 캡슐화가 된 예시다.

그럼 캡슐화를 하면 무슨 장점이 있을까?

- 1\. 데이터 무결성(Data Integrity) 보장
  - 외부에서 임의로 내부 필드 값을 수정하지 못하도록 막아서 의도치 않은 변경이나 잘못된 값이 들어오는 상황을 예방한다.
- 2\. 코드 유지보수성 향상
  - 클래스 내부 구현 방식을 수정할 때, 해당 클래스의 공개된 인터페이스(메소드, 프로퍼티)만 동일하게 유지된다면 외부 사용자는 변경 사실을 몰라도 된다. 즉, 내부 구조를 자유롭게 변경하면서도 외부와의 호환성을 유지할 수 있어 유지보수가 쉬워진다.
- 3\. 모듈성 증진
  - 내부 구현이 다른 코드와 분리되므로, 특정 기능이나 로직을 수정할 때 전체 코드에 미치는 영향을 최소화할 수 있다.
- 4\. 재사용성 증대
  - 잘 정의된 인터페이스와 캡슐화된 내부 로직은 다른 프로젝트나 시스템에서도 재사용하기 쉽다.

이번 글을 마지막으로 객체 지향 프로그래밍의 핵심 원리 4가지 상속, 다형성, 추상화, 캡슐화를 모두 알아봤다. 이 네 가지 원리는 객체 지향 프로그래밍의 근간을 이루는 중요한 요소들이다. 4가지 핵심 원리를 잘 지켜가며 좋은 코드를 짜도록 해보자.
