---
title:  "제네릭"
tags: [제네릭]
excerpt: "제네릭에 대해 알아봅니다."
header:
  teaser: /assets/images/java/generic.png
---

## Generic
### 1. 제네릭 이란?
 제네릭이란 클래스 내부에서 사용할 데이터 타입을 __외부__ 에서 지정하는 기법입니다.  
 "다양한 타입의 객체"를 다루는 메소드 혹은 컬랙션 클래스에 컴파일시 타입을 체크합니다.  
 이는 컴파일시점에서 오류를 발견할 수 있는 장점이 있습니다.  
 컴파일러가 필요한 곳에 자동으로 형변환을 진행합니다.  
 제네릭은 메소드, 클래스, 인터페이스에 사용 할 수 있습니다.

 ```java
 ArrayList list = new ArrayList();
 list.add("test");
 String temp = (String)list.get(0);
 // 제네릭을 사용하지 않을 경우 형변환이 필요합니다.

 ArrayList<Stirng> list = new ArrayList<>();
 list.add("test");
 String temp = list.get(0);
 // 제네릭을 사용할 경우 형변환이 자동으로 됩니다.

 class gen<T>{
     public static<T> boolean compare(){
         //이때 클래스의T 파라미터와 메소드의 T 파라미터는 같은 이름을 가졌지만 별개의 것입니다.
     }
 }
 ```
  
### 2.한정 된 타입 매개변수(Bounded Type Parameter)
 제네릭은 범위를 지정하는 목적으로 사용하기도 합니다.  
- <T extends K> : K의 서브클래스만 허용합니다.
  - 이때 인터페이스나 클래스 모두 extends로 사용합니다.
- <T super K> : K의 상위클래스만 허용합니다.
- <?> : 모든 클래스나 인터페이스 타입이 올 수 있습니다.

  
### 3. 제네릭을 사용 할 수 없는 경우
- 제네릭으로 배열 생성 : new 연산자가 컴파일 시점에 T가 무슨 타입인지 모르기 때문입니다.
- static 변수에 제네릭 사용 : 하나의 공유변수가 인스턴스에 따라 타입이 바뀌는 것은 불가능합니다.
  - 하지만 static 메소드에는 제네릭을 사용 할 수 있습니다.  메소드의 틀 안에서 지역변수처럼 타입 파라미터가 오가기 때문입니다.

