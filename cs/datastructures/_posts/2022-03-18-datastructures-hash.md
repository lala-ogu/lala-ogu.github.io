---
title:  "해시"
tags: [해시 함수, 해시 충돌, 해시 테이블, 해시]
excerpt: "1:1 검색에 사용되는 해시에 대해 알아봅니다."
header:
  teaser: /assets/images/etc/hash.png
---

+ 해쉬함수 전 보면 좋은 내용 [합의 알고리즘](https://steemit.com/kr/@yahweh87/1-consensus-problem)
+ 참조한 블로그 [yjshin](https://yjshin.tistory.com/entry/%EC%95%94%ED%98%B8%ED%95%99-%ED%95%B4%EC%8B%9C-%ED%95%A8%EC%88%98-%EC%9E%91%EC%84%B1-%EC%A4%91)
+ 참조한 블로그 [망나니개발자](https://mangkyu.tistory.com/102)
+ 참조한 블로그 [cherishee](https://hee96-story.tistory.com/48)
+ 원문 [ratsgo](https://ratsgo.github.io/data%20structure&algorithm/2017/10/25/hash/)


## 용어 요약
+ hashing : 해시함수를 이용하여 매핑하는 과정
+ key : 매핑 전 원래 데이터의 값
+ hash value(해시값) : 매핑 후 데이터의 값
+ hash table : 데이터의 색인 주소 + 해시값을 저장하는 자료구조

![해싱](https://user-images.githubusercontent.com/78904413/159003595-6d71e6fd-dc11-4c62-91db-fbe7d4f42def.jpg)


## 해시 함수
해시 함수는 임의의 길이의 데이터(입력)를 고정된 길이의 데이터(결과)로 매핑하는 함수 입니다.(1대1 대응) 해시 함수에 의해 얻어지는 값은 해시값, 해시코드라고 합니다. 주로 매우 빠른 데이터 검색을 위해 사용됩니다.  

해시 함수는 같은 **입력**에 대해서는 항상 같은 **출력**이 나오게 됩니다. 이러한 특징으로 무결성(조작불가, 유효성 판별)을 제공하기 위해 사용합니다. (암호학적)해시의 종류에는 md알고리즘 및 SHA알고리즘이 있습니다.  

> 필자는 hascode()의 결과값을 객체의 주소값으로 알고 있었지만, hashcode는 객체의 주소값이 아니다. 단지 해시코드 규약에 따라 반환하는 **고유한 정수값**이다.

## 해시 사용 예
1. 비밀번호 암호화
2. 복제문서 판별(이때 고정된 길이의 해시값으로 비교하므로 속도에 유리합니다.)
3. 효율적이고 빠른 데이터 검색용도

## 해시 함수의 특징
### 1. 어떠한 길이의 입력값에도 항상 **고정된 길이의 해시값**을 출력합니다.
![해시1](https://user-images.githubusercontent.com/78904413/159002987-eaa08823-48cf-454f-a932-45f2c2976d9e.jpg)  

![해시2](https://user-images.githubusercontent.com/78904413/159003155-f6262d2b-d04f-453c-83ec-e9f4a6ef49a5.jpg)  

"안녕하세요"의 입력값과 "가나다라마바사아자타카..."의 입력값의 길이가 다름에도 고정된 길이의 해시값이 출력됩니다.  

### 2. 입력값의 아주 일부만 변경되어도 **전혀 다른 해시값**을 출력합니다. = 무결성
![해시3](https://user-images.githubusercontent.com/78904413/159003185-11d50a54-5415-409d-bcd8-4116b9473e8b.jpg)  

"안녕하세요"와 "안녕하세요."의 사소한 차이에도 다른 해시값이 출력됩니다.

### 3. 그렇기에 출력된 결과 값을 토대로 입력값을 **유추할 수 없습니다**. = 암호용

## 해시 충돌
해시 함수에서 희박하게 같은 입력에 대해 다른 출력이 될 경우가 있습니다. 이는 입력값의 범위보다 해시값의 범위가 좁은 경우가 많기 때문에(many to one) 발생합니다. 이런 경우를 해시 충돌이라고 표현하며, 충돌이 적은 해시 함수가 좋은 해시 함수 입니다.  

![해시충돌](https://user-images.githubusercontent.com/78904413/159003625-1b276faa-23df-43eb-86eb-26c4d709e8c5.jpg)  

위 그림에서 'john smith'와 'sandra dee'의 해시값이 같으므로 해시 충돌입니다.  

## 해시테이블
+ 충분히 큰 공간(배열)을 할당 받은 다음 key에 해시 함수를 이용하여 **고유** 색인(index : 속도를 위해 정수값을 사용)을 생성합니다.
+ index가 **일종의 암호화 된 key값**이 되어 value와 함께 버켓(buckets) 저장됩니다.
+ 즉 데이터는 (index, value)형태로 버켓에 저장되고, (key, 버켓)의 묶음이 해시테이블 내에 저장됩니다.
+ index를 활용해 value값을 저장하거나 검색합니다.

해시 테이블은 버킷이 비어있는 index값이 존재할 수 있습니다. 키의 전체 개수와 동일한 버킷 개수를 가진 해시테이블을 direct-address table이라고 하지만 효율적이지 못한 리소스 문제로 운용하지 않습니다. 보통 해시테이블 크기(M)가 실제 사용하는 키 개수(N)보다 적은 해시테이블을 운용합니다. 이떄 n/m을 load factor라고 합니다.  

![img](https://user-images.githubusercontent.com/78904413/159024806-01143b28-edad-4d5d-aada-9b08b0486b26.png)  

예를들면 크기가 16인 hashtable에 ("John Smith", "521-1234")인 데이터를 저장한다면, index = 해시함수("John Smith") % 16 을 통해 index값을 계산합니다. 이 계산을 통해 array[index] = "521-1234"로(02, "521-1234") 버킷에 저장하게 됩니다.  
이러한 해싱 구조로 데이터를 저장하면 key값("John Smith")으로 데이터를 찾을 때 해시 함수를 1번만 수행하면 되므로(index값을 내기위해) 해시테이블의 크기에 상관 없이 매우 빠르게 데이터를 저장/조회/삭제할 수 있습니다. 해시테이블의 평균 시간복잡도는 $O(1)$입니다. 이는 조회에서는 1대1 대응이고, 저장/삭제에서는 다른 데이터로 채울 필요가 없기 때문입니다.



## 해시 충돌 해결
해시 테이블에서는 충돌에 대한 문제를 분리 연결법과 개방 주소법으로 해결하고 있습니다.

### 1. 분리 연결법(Separate Chaining)
![img (1)](https://user-images.githubusercontent.com/78904413/159031964-cb600b72-1537-4381-8ade-40d747f5e464.png)  

분리 연결법이란 위의 그림처럼 동일한 버켓에 접근시, **추가 메모리**를 사용하여 체인처럼 노드를 추가하여 다음 데이터의 노드(주소)가르키는 방식으로 구현하는 것입니다. 그림을 보면 한 버킷당 들어갈 수 있는 엔트리의 수에 제한을 두지 않습니다.  
즉, 해시테이블 내에 버킷이라는 LinkedList 자료구조가 존재하는 것입니다. 실제로 java8의 해시 테이블은 tree를 사용해 이 방식을 구현하였습니다.(data가 6개 이하면 linked list, 8개 이상이면 tree)  

해시테이블의 확장 없이 간단하게 구현이 가능하지만, 데이터의 수가 많아지면 연결된 리스트까지 검색해야 하므로 $O(n)$까지 시간 복잡도가 증가할 수 있습니다. 삭제에서도 index가 가리키는 linkedlist에서 해당 노드를 삭제합니다.  

### 2. 개방 주소법(Open Addressing)
개방 주소법은 추가 메모리를 사용하는 방식과 다르게 **비어있는 해시 테이블의 공간**을 활용합니다. 이를 구현하기 위한 방법으로 3가지 방식이 존재합니다. 만약 이 방식에서 데이터를 삭제하면 삭제된 공간은 dummy space로 사용되어, hash table을 재정리 해주는 작업이 필요하게 됩니다.  

#### 2.1. Linear Probing
현재의 버킷 index로 부터 고정 폭 만큼 이동하여 차례대로 검색해 비어 있는 버킷에 데이터를 저장합니다.(순차탐색) 삭제는 더미노드를 넣어서 다음 인덱스까지 검색을 연결해주는 역할을 해줘야 합니다.(삭제가 어렵고, 크기 낭비)

#### 2.2. Quadratic Probing
비어있는 저장 공간 탐색 폭을 제곱으로 정합니다. 예를들어 처음 충돌이 발생한 경우 1만큼 이동하고 그 다음 충돌에는 $2^2$, $3^2$, $4^2$ index만큼 옮겨서 탐색합니다.

#### 2.3. Double Hashing
index를 한번 더 해싱하여 해시의 규칙성을 없애는 방식입니다. 다른 방법들 보다 더 많은 연산을 하게 됩니다.

## 해시테이블의 성능 향상
Separate changing에 경우, 버킷이 일정 수준으로 차 버리면 각 버킷에 연결되어 있는 List의 길이가 늘어나 성능이 떨어지기 때문에 버킷의 개수를 늘려줘야 합니다. 또한 Open addressing의 경우, 고정 크기 배열을 사용하기 때문에 데이터를 더 넣기 위해서는 배열을 확장해야 합니다.  

보통 두 배로 확장하는데, 확장하는 임계점은 크기의 75%가 될 때입니다. 리사이징은 더 큰 버킷을 가지는 array를 새로 만든 다음에, 다시 새로운 array에 hash를 다시 계산해서 복사해줘야 합니다(리사이징 자체도 성능 저하).  

또, 해시 테이블에서 자주 사용하는 데이터에 cache 로직을 적용하면 성능을 향상 시킬 수 있습니다.  
