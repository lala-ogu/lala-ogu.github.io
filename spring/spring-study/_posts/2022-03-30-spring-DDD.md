---
title:  "DDD - 도메인 주도 개발"
excerpt: "도메인 주도 개발이란 무엇인지 알아봅니다."
tags: [도메인, DDD]
header:
  teaser: /assets/images/spring/ddd.jpg
---

## 읽기전 보면 좋은 개념
- [domain, VO, DTO, Entity](https://hunnycombo.github.io/spring/spring-domain_DTO/)
- [layered architecture](https://hunnycombo.github.io/spring/spring-architecture/)

## 1. DDD
### 1.1 도메인
도메인이란 소프트웨어로 해결하고자 하는 문제 영역으로 볼 수도 있고, 기존의 개념으로 소프트웨어의 비즈니스적인 부분으로 볼 수 있습니다.  
도메인은 다시 하위 도메인으로 나눌 수 있습니다. 예를들어 온라인 쇼핑몰의 "쇼핑"이라는 도메인의 하위 도메인으로,  
주문, 결제, 배송 같은 도메인을 가질 수 있습니다.

### 1.2 domain model
도메인 모델은 DB의 엔티티 설계에 반영이 되기 때문에(JPA) 엔티티와 유사할 가능성이 높습니다.  
하지만 DB에는 없고 비즈니스 로직상에서만 존재하는 모델도 있습니다.  
도메인 모델이란 결국 한 도메인을 개념적으로 표현한 것입니다.  

![img](https://user-images.githubusercontent.com/78904413/160830440-8cd17a29-b9a9-47b2-a4db-c284ae6db198.png)  

도메인 모델은 개발하고자 하는 영역을 분석한 결과로 도출된 객체라고 할 수 있습니다.  


### 1.3 도메인 객체 중심 개발
도메인 모델을 반영하는 객체를 만들어두고, 그것을 중심으로 개발하는 아키텍처입니다.  
도메인 객체를 만들어두고 그 안에 정보를 담아서 각 계층(layer)사이에 전달하게 만드는 것입니다.  

### 1.4 빈약한 도메인 모델
도메인 객체도 결국 객체이기 때문에 속성과 행위를 가지고 있어야 온전한 객체라 할 수 있습니다.  

```java
public class Category { 
  private int id; 
  private String name; 
  private Set<Product> productList; 
} 

public class Product { 
  private int id; 
  private String name; 
  private int price; 
  private Category category; 
}

// 이렇게 정보만 담겨있고 기능이 없다면 온전한 객체라고 보기 힘듭니다.
// 그래서 해당 도메인 객체에 대한 비즈니스 로직이 있다면, 서비스 계층(application layer)보다,
// 해당 도메인에 작성해주는 것이 좋습니다.
```
  
만약 어떤 카테고리에 포함된 모든 상품의 가격을 합해야 하는 비즈니스 로직이 필요하다면,  
이 로직을 도메인 객체에 포함시킵니다.  

```java
public class Category { 
  private int id; 
  private String desc; 
  private Set<Product> products; 
  public int totalPriceOfCategory() { 
    int sum = 0; 
    for(Product product : category.getProducts()) { 
      sum += product.getPrice(); 
    } 
  }
}
```

비즈니스 로직을 도메인 객체에 작성해줌으로써 얻는 장점으로는,  

- 도메인 객체의 응집도를 높입니다.
- 서비스 계층의 코드가 간결해집니다.
- 속성과 행위를 가져 OOP의 원칙을 지킬 수 있습니다.
- 비즈니스 로직이 서비스 계층에만 있다면 서비스 계층을 DI해야 하지만, 도메인 계층에 둠으로서 불필요한 DI를 줄입니다.

대신 여러종류의 도메인 객체를 조합해야 하거나, 해당 도메인 객체와 결합도가 떨어지는 비즈니스 로직(메일 발송) 또는 인프라스트럭처(DAO)계층과 연동되어야 하는 로직 등은  
서비스 계청에서 작성하게 합니다.

## 2. domain layer
도메인 계층의 객체는 spring에 의해 싱글톤으로 관리되지 않음에도, 상태정보를 갖기 때문에 사용자별 요청에 대해  
독립적인 상태를 유지해야 하므로, 짧은 시간 존재하고 사라집니다.  
복잡한 도메인의 구조와 로직을 도메인 계층에 반영하면, 도메인 모델 설계에 변경이 발생했을 경우 빠르게 대응할 수 있는 장점이 있습니다.  

### 2.1 도메인 계층의 분리 이유
결국 도메인 객체에 담을 수 있는 비즈니스 로직은 한계가 있습니다.  
도메인 객체를 활용하려면 기존의 3계층 방식으로는 부족합니다.  
도메인 객체가 필요한 정보를 얻고 반영하기 위해서 DAO계층의 오브젝트를 직접 DI받아서 이용할 방법이 필요해졌습니다.  
그래서 기존의 3계층 방식에서 도메인 계층을 추가한 방식이 등장하게 되었습니다.  
도메인 계층은 서비스 계층과 DAO계층(인프라스턱처)계층 사이에 존재합니다.  
도메인 계층이 분리됨으로서 비즈니스 로직은 기존의 서비스 계층이 아닌 도메인 계층의 객체가 처리하고,  
도메인 객체가 기존 데이터 접근 계층의 기능을 직접 활용할 수 있게 합니다.  

### 2.2 분리의 결과
서비스 계층의 서비스 로직은 여러 도메인 객체의 기능을 조합하는 복잡한 작업을 처리하게 됩니다.  
즉 단순한 위임의 역할을 하게 되는 것입니다.  
그리고 서비스 계층에서 도메인 객체를 만들었든, DAO계층을 통해 도메인 객체를 얻었든 출처에 상관없이  
도메인 객체가 비즈니스 로직을 **스스로** 처리하도록 요청할 수 있습니다.
도메인 객체는 스피링의 빈이 아니기 때문에 AOP를 적용하여 다른 빈을 DI받도록 설정을 해주어야 합니다.  

### 2.3 도메인 객체의 범위
도메인 계층을 둘 경우, 도메인 객체가 도메인 계층을 벗어나도 될지 말지를 고려해야 합니다.  

#### 2.3.1. 모든 계층에서 도메인 객체를 사용한다.
DDD의 장점을 많이 얻어 갈 수있지만 비즈니스 로직과 DB관련 작업을 갖고 있기 때문에 주의해서 사용해야 합니다.

#### 2.3.2. 도메인 계층을 벗어나지 못하게 한다.
도메인 계층 밖으로 전달될 때에는 정보 전달을 위한 DTO를 만들어 넘겨주는 방식입니다.  
도메인 겍체와 유사한 객체를 만들고 변환해야하는 번거로움이 있습니다.

## 참고
[망나니](https://mangkyu.tistory.com/160)