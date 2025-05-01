---
title: "25.03.24 / 메모리 풀(Memory Pool)"
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

C#에서 메모리 풀은 자주 재사용되는 객체나 버퍼 등에 대해 동적 할당과 해제를 반복하기 보다는, 미리 일정한 개수나 일정한 크기의 버퍼를 준비해두고 필요할 때 가져와 쓰고 사용이 끝나면 다시 반환하는 구조를 의미한다. 이렇게 하면 힙(Heap) 메모리에 대한 잦은 할당/해제를 줄여 가비지 컬렉션의 부담을 낮추고, 성능을 향상시킬 수 있다.

대표적인 예로 .NET의 System.Buffers.ArrayPool<T>와 System.Buffers.MemoryPool<T>가 있으며, 필요에 따라 커스텀 풀을 구현해 사용하는 경우도 있다.

- 1\. ArrayPool
  - 1-1. 특징
    - ArrayPool<T>는 T 타입의 배열을 풀링(Pooling)하기 위한 클래스이다.
    - .NET 프레임워크 4.5 이후 버전부터 사용할 수 있으며, 범용적으로 쓰기 쉽도록 설계되었다.
    - ArrayPool<T>.Shared라는 싱글톤 풀이 제공되며, 직접 풀을 만들 수도 있다.
  - 1-2. 기본 사용법

```csharp
using System.Buffers;

public class ArrayPoolExample
{
    public void UseArrayPool()
    {
        // 1) Shared 풀 얻기 (싱글톤)
        ArrayPool<byte> pool = ArrayPool<byte>.Shared;

        // 2) 배열 대여 (Rent). 필요로 하는 최소 크기를 인자로 넘깁니다.
        byte[] buffer = pool.Rent(1024);

        try
        {
            // 3) 대여한 배열 사용
            //    필요한 작업을 수행합니다. 예: 네트워크 I/O, 파일 I/O 버퍼로 활용 등
            DoWorkWithBuffer(buffer);
        }
        finally
        {
            // 4) 풀에 배열 반환
            pool.Return(buffer);
        }
    }

    private void DoWorkWithBuffer(byte[] data)
    {
        // 여기에 실제 비즈니스 로직이나 I/O 등을 수행
    }
}
```

- Rent(int minimumLength): 풀에서 최소 minimumLength 길이를 만족하는 배열을 빌려온다. 실제로는 더 큰 배열이 빌려질 수도 있다.
- Return(T\[\] array, bool clearArray = false): 빌려간 배열을 반환한다. clearArray를 true로 설정하면, 사용하던 데이터를 0으로 초기화한 후에 반환한다. 성능 저하가 있을 수 있으니 민감한 데이터가 아니라면 기본값 false로 두는 편이다.
  - 1-3. 이점
    - 빈번한 배열 할당/해제를 줄여서 가비지 컬렉터의 부담을 경감한다.
    - 쓰레드 안전(Thread-safe)하도록 설계되어 있으므로 멀티 쓰레드 환경에서 쉽게 사용할 수 있다.
- 2\. MemoryPool
  - 2-1. 특징
    - .NET Core 2.1 이후 제공되는 MemoryPool<T>는 메모리를 좀 더 효율적으로 관리하고, Memory<T>나 IMemoryOwner<T>같은 구조로 추상화해 제공한다.
    - 주로 고성능 I/O 시나리오(NetCore 서버, 네트워크 프로그래밍 등)에서 사용한다.
    - 내부적으로 버퍼 풀을 사용하여 메모리 할당 비율을 줄인다.
  - 2-2. 기본 사용법

```csharp
using System.Buffers;

public class MemoryPoolExample
{
    public void UseMemoryPool()
    {
        // 1) 기본 제공 Pool 얻기
        using var memoryPool = MemoryPool<byte>.Shared;

        // 2) 메모리 대여 (Owner 반환)
        IMemoryOwner<byte> owner = memoryPool.Rent(1024);
        Memory<byte> memory = owner.Memory;

        // 3) 메모리 사용
        //    필요한 범위에 대해 Span<byte>로 접근 가능
        Span<byte> span = memory.Span;
        DoWorkWithSpan(span);

        // 4) owner.Dispose() 호출 시 메모리 풀에 반환됨
        //    (using 구문을 사용하면 자동으로 Dispose가 호출됨)
    }

    private void DoWorkWithSpan(Span<byte> span)
    {
        // 할당된 메모리를 필요에 따라 접근/사용
    }
}
```

- MemoryPool<T>.Shared : 싱글톤 형태의 공용 풀이다.
- Rent(int minBufferSize) : 최소 크기를 만족하는 IMemoryOwner<T>를 반환한다.

- IMemoryOwner<T>는 IDisposable을 구현하므로, using 구문으로 사용하며 코드가 끝날 때 자동으로 풀에 반환된다.
  - 2-3. 이점
    - Span<T> 또는 Memory<T> 등의 구조와 함께 사용해 관리가 더욱 편해진다.
    - 비동기/동기 작업에서 모두 유연하게 사용할 수 있으며, 특히 네트워크 프로그래밍이나 스트림 처리에서 유용하다.
- 3\. 커스텀 풀 구현
  - 상황에 따라 ArrayPool<T>나 MemoryPool<T> 외에 직접 풀을 구현해야 하는 경우도 있다.
    - 특정한 객체(예를 들어 커스텀 클래스)를 풀링해야 하는 경우
    - 배열보다 작은 구조체나 매우 특수한 형태의 메모리 관리가 필요한 경우
  - 3-1. 예시

```csharp
using System.Collections.Concurrent;

public class ObjectPool<T> where T : class, new()
{
    private readonly ConcurrentBag<T> _objects = new ConcurrentBag<T>();

    public T Rent()
    {
        if (_objects.TryTake(out T item))
        {
            return item;
        }
        else
        {
            return new T();
        }
    }

    public void Return(T item)
    {
        _objects.Add(item);
    }
}
```

- - ConcurrentBag<T>를 사용해 멀티쓰레드 환경에서도 안전하게 객체를 풀링할 수 있다.
  - Rent()에서 먼저 풀에 있는 객체를 꺼내 쓰고, 없으면 새로 생성한다.
  - Return()에서 사용이 끝난 객체를 다시 풀에 반환한다.
    - 실제 성능, 초기화/정리 로직, 재사용 가능 여부 검사 등 다양한 부분을 고려해야 한다.

- 4\. 주의 사항
  - 4-1. 남용하지 말 것
    - 모든 경우에 메모리 풀이 이점을 주는 것은 아니다. 배보다 배꼽이 더 커질 수 있으므로, 할당이 매우 빈번하거나, 대용량 버퍼를 여러 번 할당하는 상황이 아니라면 기본 가비지 컬렉터가 관리하도록 두는 편이 더 단순할 수 있다.
  - 4-2. 풀 사용 시 초기화 비용
    - 풀에서 가져온 객체(또는 버퍼)에 이전 사용 흔적이 남아있을 수 있다. 필요하다면 명시적으로 초기화(예를 들어 0으로 채우기) 작업을 해줘야 한다.
  - 4-3. 스레드 안전
    - 기본 제공되는 ArrayPool<T>.Shared나 MemoryPool<T>.Shared, 그리고 ConcurrentBag<T> 기반 풀 구현은 쓰레드 안전 하도록 동작하므로 멀티 쓰레드 환경에서 사용할 수 있다. 하지만 커스텀 구현 시에는 쓰레드 안정성을 반드시 염두에 두어야 한다.
  - 4-4. 풀 크기 관리
    - 너무 크게 만들면 메모리가 낭비되고, 너무 작게 만들면 재사용 효율이 떨어질 수 있다. 실제 사용 패턴을 파악해 최적의 크기를 설정하는 것이 좋다.
    - .NET 제공 풀(ArrayPool<T>.Shared)은 내부 알고리즘으로 자동 조정하지만, 특수 용도의 풀을 만들 경우 수동으로 크기를 관리해야 한다.

C#에서 메모리 풀은 빈번한 메모리(객체/배열 등) 할당/해제 횟수를 줄여 가비지 컬렉터 부담을 최소화하고, 성능을 최적화하는 기술이다. .NET에서는 ArrayPool<T>이나 MemoryPool<T>와 같은 표준 클래스를 사용해 쉽게 구현할 수 있으며, 특수한 요구 사항이 있으면 커스텀 객체 풀을 만들 수도 있다.

그러나 작은 규모의 어플리케이션이거나 할당이 빈번하지 않다면 기본 가비지 컬렉터가 이미 충분히 우수하므로, 풀 설계의 이점이 크지 않을 수 있다. 이러한 이유로 고성능을 요구하는 네트워킹, 파일/스트림 I/O, 게임 서버 등에서 자주 사용된다. 반드시 풀을 사용하기 전, 해당 시나리오에서 메모리 풀로 인한 이점이 명확한지를 확인하고 적용하도록 하자.
