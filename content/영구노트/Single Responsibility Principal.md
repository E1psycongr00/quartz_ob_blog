---
tags:
  - 완성
  - 객체지향
aliases:
  - SRP
  - 단일 책임 원칙
date: 2024-10-16
title: Single Responsibility Principal
---
작성 날짜: 2024-10-16
작성 시간: 16:29


----
## 내용(Content)

### SRP

>[!summary]
>단일 책임 원칙은 객체는 **단 하나의 책임만을 가져야 된다** 원칙을 의미한다.

객체의 책임이란 무엇일까? `객체지향의 사실과 오해`에서 책임이란 역할과 밀접한 관련이 있다고 한다. 즉, 단일 책임이란, 하나의 객체는 하나의 일관된 역할을 수행해야 한다는 의미이다.
역할을 수행한다는 것은 객체가 어떤 일을 수행한다는 것이고, 이것은 클래스의 메서드를 의미한다. JAVA와 같은 객체 지향 기반 언어는 객체의 역할을 매우 중요하게 생각하기 떄문에 그들의 역할을 정의한 interface라는 개념이 존재한다.

왜 객체는 단일 책임을 가져야 할까? 객체가 단일 책임을 가질 때, 유지 보수가 쉽고, 보다 직관적으로 객체가 하는 역할을 이해할 수 있기 때문이다. 예를 들어 어떤 사원이 존재하는데 해당 사원이 회계, 인사, 기술 3가지 역할 모두 수행한다면, 조직이 작을 땐 효과적일지 몰라도, 조직이 커지면, 누구를 어떻게 써야 할 지 애매해진다. 이 때문에 우리는 객체가 단일 책임을 가지도록 노력해야 한다.

### 단일 책임을 위배한 경우

```java
@Getter
@AllArgsConstructor
public class Order {
    private long orderId;
    private List<Item> items = new ArrayList<>();
    private double totalPrice;
    private String status;
    private LocalDateTime orderDate;
    private LocalDateTime deliveryDate;
    private String deliveryAddress;
    private String customerName;
    private String customerEmail;
    private String customerPhone;

    public void addItem(Item item) {
        this.items.add(item);
    }

    public void removeItem(Item item) {
        this.items.remove(item);
    }

    public void removeRecentItem() {
        this.items.removeLast();
    }

    public double calculateTotalPrice() {
        return items.stream()
                .mapToDouble(Item::getPrice)
                .sum();
    }

    public void changeCustomerInfo(String name, String email, String phone) {
        this.customerName = name;
        this.customerEmail = email;
        this.customerPhone = phone;
    }

    public void changeDeliveryInfo(String address, LocalDateTime deliveryDate) {
        this.deliveryAddress = address;
        this.deliveryDate = deliveryDate;
    }

    public void changeOrderStatus(String status) {
        this.status = status;
    }
    
    
}
```

지금 코드를 살펴보면 Order 객체는 다음과 같은 역할을 가진다.

- Item 정보를 추가하거나 제거한다.
- 주문 내역의 전체 가격을 계산한다.
- 고객 정보를 업데이트 한다.
- 배달 정보를 업데이트한다.

이 역할을 보니 어떤 생각이 드는가? 주문을 하다보면 주문 내역, 전체 가격, 주문한 고객 정보, 배달 관련 정보들을 포함할 수 있고 경우에 따라 수정되기도 한다. 그러나 주문이 이러한 모든 상태를 관리하고 이런 모든 역할에 책임을 진다면, 주문(Order) 객체는 매우 무거워지고, 관리하기 쉽지 않다. 그리고 만약 가격의 경우에도 특별 세일이나, 결제 카드에 따라 가격이 달라진다면, 이것은 문제를 일으킬 수 있다.

고로 Order 객체를 분리할 필요성이 있다.

### SRP 위배 코드 개선해보기

#### 상태를 관리할 책임 분리하기

객체는 메서드를 통해 상태를 관리하지만, 주문 객체에서 고객 정보 상태를 직접 관리할 필요는 없다. 그렇다면 고객 객체를 따로 만들어 고객의 상태를 관리하고 이 객체를 받아서 Order에서 Customer를 업데이트해도 충분하다.


```java
@Getter
@AllArgsConstructor
public class Customer {
    private long customerId;
    private String name;
    private String email;
    private String phone;

    public void changeCustomerInfo(String name, String email, String phone) {
        this.name = name;
        this.email = email;
        this.phone = phone;
    }
}
```

이런 방식으로 배달 관련 정보도 Delivery로 빼고, 적용할 수 있다. 그러면 개선된 Order 코드는 다음과 같다.

```java
@Getter
@AllArgsConstructor
public class Order {
    private long orderId;
    private List<Item> items = new ArrayList<>();
    private double totalPrice;
    private OrderState state;
    private LocalDateTime orderDate;
    private Delivery delivery;
    private Customer customer;

    public void addItem(Item item) {
        this.items.add(item);
    }

    public void removeItem(Item item) {
        this.items.remove(item);
    }

    public void removeRecentItem() {
        this.items.removeLast();
    }

    public double calculateTotalPrice() {
        return items.stream()
                .mapToDouble(Item::getPrice)
                .sum();
    }

    public void updateCustomer(Customer customer) {
        this.customer = customer;
    }

    public void changeDeliveryInfo(Delivery delivery) {
        this.delivery = delivery;
    }

    public void changeOrderState(OrderState state) {
        this.state = state;
    }
    
}
```

updateCustomer나 changeDeliverInfo를 보면 객체를 따로 주입 받아서 상태를 업데이트한다. 만약 고객, 또는 배달에 대한 정보가 변경되거나 확장되더라도 Order엔 영향을 주지 않는다. 즉, Order에게는 이러한 책임이 존재하지 않는다는 것이다. 가져다 쓰기만 할 뿐...





## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










