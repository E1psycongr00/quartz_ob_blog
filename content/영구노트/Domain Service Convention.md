---
tags:
  - 완성
  - JAVA
  - 객체지향
  - DDD
aliases: 
date: 2024-10-16
title: Domain Service Convention
---
작성 날짜: 2024-10-16
작성 시간: 21:48


----
## 내용(Content)

### Domain Service Convention

도메인을 구성하다보면 네이밍 컨벤션을 생각해야 할 때가 있다. 무작정 DomainService를 접미사로 붙인다고 가정해보자. Java Spring 을 활용할 때 보통 Web 영역과 application 영역, Domain 영역으로 나뉜다. 이 때 Applicaion 영역은 보통 접미사로 Service라고 이름을 짓는다.

![[대충네이밍을지은DomainService (draw).svg]]

Application Layer 영역에서 OrderService와 OrderDomainService 이렇게 두면 네이밍도 헷갈리고 OrderDomainService에 여럿 비즈니스 로직을 추가하게 되서 자칫 DomainService가 비대해 질 수 있다.

###  Rule

#### 클래스 규칙

- DomainService는 Domain Layer에 위치해야 한다.
- 특별한 이유가 없는 한 인터페이스를 만들지 마라(관리할 패키지가 많아질 수 있다)
- 접미사를 붙여서 구분 지어라

>[!example]
>```java
>public class IssueManager implements DomainService {
>	// logic
>}
>```

도메인 서비스를 인터페이스로 도입했는데, 개인적인 생각은 아예 없어도 되고, Domain Service라고 명확하게 구분짓고 싶다면 Common Package에 마커 인터페이스로 사용하는 것이 괜찮을 것 같다. 

>[!info]
>마커 인터페이스는 메서드를 정의하지 않은 인터페이스로 특정 객체가 어떤 객체인지 표시할 때 사용한다. 가독성 향상 및 다형성 등에 응용된다.

#### 메서드 규칙

- **Get 메서드 구현 X** (Domain Service는 상태를 가지지 않는다)
- VO가 아닌 **엔티티의 상태 변화**에 사용해야 한다. 
	- 주로 엔티티와 엔티티간의 소통(Aggergate)에 사용된다.
- **평범하고 일반적인 메서드 네이밍 사용 금지**(updateIssueAsync)
	- 평범한 메서드는 도메인의 메서드와 겹칠 우려가 있다.
- 유**효한 엔티티를 Parameter로 사용**해야 한다.

>[!example]
>```java
>public Task assignToAsync(Issue issue, IdentityUser user) {
>	// logic
>}
>```

#### 예외 처리 규칙

- 예외 발생시 BusinessException 또는 Custom BusinessException을 사용한다.

```java
public async Task AssignToAsync(Issue issue, IdentityUser user)
{
    var openIssueCount = await _issueRepository.GetCountAsync(
            i => i.AssignedUserId == user.Id && !i.IsClosed
        );

        if (openIssueCount >= 3)
        {
            throw new BusinessException("IssueTracking:ConcurrentOpenIssueLimit");
        }

        issue.AssignedUserId = user.Id;
}
```

#### 그 외 규칙

- **DTO를 리턴하지 않는다.**
	- DTO 는 Application Layer에서 책임져야할 객체이다.

### Naming Convention 예시

DomainService 네이밍은 Domain 객체에서 복잡한 비즈니스 규칙을 분리함과 동시에 이해하기 편한 네이밍을 정해야 한다.

꼭 정답은 아니지만 Domain Service 네이밍에 대한 대략적인 예시를 정할 수 있다.

**접미사**
- Calculator(복잡한 계산 로직)
- Policy(비즈니스 규칙 또는 의사 결정 로직)
- Strategy(알고리즘, 또는 전략)
- Manager(Service와 역할 동일)
- 그 외에 domain에 특화된 네이밍

#### Calculator

주로 복잡한 수학적 계산이나 복잡한 수치 연산에 사용된다.

#### Policy

Policy는 사전 의미로는 `정책`이다. 정책은 보통 정부에서 내놓은 공약인데 어떤 문제의 해결하기 위한 목표로 포괄적인 의미를 가지고 있다. 그래서 비즈니스 로직이나 의사 결정 로직의 경우를 캡슐화 할 때 사용한다. "How" 보다는 "What" 에 초점을 두고 있다. 거대한 시스템 및 비즈니스 로직에 사용될 수 있다.

>[!example] Example-1
>- DiscountPolicy
>	- AmountDiscountPolicy
>	- PeriodDiscountPolicy
>	- NoneDiscountPolicy
#### Strategy

Policy는 여러 조건을 평가해서 결정하는 반면에, Strategy는 단일 작업에 대해서 여러 방법을 세분화 하고자 할 떄 사용한다. 그래서 "How"에 초점을 둔다. 

>[!example] Example-1
>- WriteStrategy
>	- CapitalWriteStrategy
>	- CamelCaseWriteStrategy
>	- SnakeCaseWriteStrategy


#### Manager

DomainService의 경우 Application Service와 접미사가 같고 앞에 얼마나 구체적인 도메인에 관련된 네이밍으로 구성되었냐로 Domain Service와 Application Service를 판단해야 하기 때문에 헷갈릴 수 있다. Manager로 선언하면, Service와 같은 느낌으로 사용 가능하면서, Application Service와 Domain Service를 쉽게 구분 지어 줄 수 있다. 

개인적인 생각은 [[Domain Service를 사용해야 할 상황]] 의 5번의 경우에 쓸만하다고 생각한다. 

```java
public class OrderCompletionManager {
    private final OrderRepository orderRepository;
    private final InventoryRepository inventoryRepository;
    private final PaymentRepository paymentRepository;
    private final CustomerRepository customerRepository;

    public void completeOrder(Long orderId) {
        // 1. 주문 상태 변경
        Order order = orderRepository.findById(orderId)
            .orElseThrow(() -> new OrderNotFoundException(orderId));
        order.markAsCompleted();

        // 2. 재고 감소
        for (OrderItem item : order.getItems()) {
            Inventory inventory = inventoryRepository.findByProductId(item.getProductId())
                .orElseThrow(() -> new InventoryNotFoundException(item.getProductId()));
            inventory.decreaseStock(item.getQuantity());
        }

        // 3. 결제 처리
        Payment payment = paymentRepository.findByOrderId(orderId)
            .orElseThrow(() -> new PaymentNotFoundException(orderId));
        payment.markAsProcessed();

        // 4. 고객 포인트 적립
        Customer customer = customerRepository.findById(order.getCustomerId())
            .orElseThrow(() -> new CustomerNotFoundException(order.getCustomerId()));
        customer.addPoints(calculatePointsForOrder(order));

        // 변경된 애그리거트 저장
        orderRepository.save(order);
        inventoryRepository.saveAll(inventories);
        paymentRepository.save(payment);
        customerRepository.save(customer);
    }

    private int calculatePointsForOrder(Order order) {
        // 포인트 계산 로직
    }
}
```

위의 경우에는 여럿 로직 flow들이 보이지만 모두 도메인에 관련된 로직이고, 여러 애그거트가 모여있어서 복잡한 로직이다. 외부와 소통이 아닌 도메인만이 관련된 로직이라면 Application Service에서 관리하는 것이 아닌 DomainService에서 관리가 가능하다. 이 경우 Application Service와 매우 유사한 구조를 지니고 있기 때문에 Domain Service의 책임을 명확히 하는 것이 좋다.

다만 Repository를 Domain Service에 사용하는 것은 개발자마다 호불호가 갈리기 때문에 신중해야 한다. Application Service의 책임을 나눠 가지기 때문이다. 그렇기 때문에 Repository를 Domain에 써야 하는 패턴과 네이밍을 사용해야 한다면 엄격하게 통제해서 사용해야 한다.

[도메인과 도메인 서비스 Repository 관계](https://littlemobs.com/blog/using-repository-in-domain-model/)
#### Policy와 Strategy 적용 기준 이해하기

예시를 보고 적용 기준에 대해서 이해해보자.

- CachePolicy
	- HttpCachePolicy
	- DatabaseCachePolicy

캐시 정책은 우선 무엇(What)을 캐싱할 지 생각해본다. 해당 예시에서 적용 대상은 Http와 DB 캐싱이다. "캐시 정책을 수행하는데 무엇을 캐싱해야 할까?" 고민해서 Http 그리고 DB를 대상으로 캐싱 정책을 정한 것이다.

- DiscountPolicy
	- PeriodDiscountPolicy(기간별 할인 정책)
	- ItemsDiscountPolicy(제품별 할인 정책)
	- UserLevelDiscountPolicy(유저 등급별 할인 정책)

할인에 대한 전반적인 규칙 및 지침이 있고, 회사의 **장기적인 할인 전략**을 나타낼 때 사용된다.

- CachingStrategy
	- LazyCachingStrategy
	- EagerCachingStrategy

캐싱 전략의 경우, 캐싱에 대해 세부적으로 어떻게(How) 캐싱할 지 알고리즘에 대해 고민해본다. 그래서 바로 캐싱하는 방식과 Lazy 캐싱 방식 이렇게 어떤 방법에 대해서 캐싱한다면 이것은 전략이라 볼 수 있다.

- PaymentDiscountStrategy
	- CardPaymentDiscountStrategy
	- CachePaymentDiscountStrategy
	- SamsungPaymentDiscountStrategy



## 질문 & 확장

네이밍 컨벤션의 경우 Policy나 Strategy는 잘못 사용하면 독이 될 수도 있다. 경우에 따라서는 DiscountPolicy보다는 DiscountService를 만들고 따로 Policy나 Strategy를 만들어 주입하는 방향도 적절할 수 있다. 그러나 이 방법은 불필요하게 객체 갯수를 늘리고 결국엔 도메인에 관련된 로직이기 때문에 DomainService에 직접 구현하는 것이 나을 수도 있다. 모든 설계는 Trade-off이다.
## 출처(링크)

- https://www.nexcess.net/blog/domain-naming-best-practices/
- https://abp.io/docs/latest/framework/architecture/best-practices/domain-services
- https://littlemobs.com/blog/using-repository-in-domain-model/
## 연결 노트










