---
title: "Design Pattern - 구조패턴"
date: 2017-05-14
categories:
 - Develop
 - Design Pattern
tags:
 - design pattern
 - structural
---

## 구조 패턴이란?
##### 더 큰 구조를 형성하기 위해 어떻게 클래스와 객체를 합성하는지에 관련된 패턴
> 인터페이스나 구현을 복합하는 것이 아니라 새로운 기능을 실현하기 위해 객체를 합성하는 방법을 제공하여 런타임에 복합 방법이나 대상을 변경할 수 있음


---
## 1. Adapter
> 클래스의 인터페이스를 원하는 형태로 변환하여, 서로 다른 인터페이스를 갖는 클래스들을 함께 동작하게 하도록 함

##### 구조
![Adapter](https://prashantbrall.files.wordpress.com/2010/12/adapter-pattern-2.png?w=595){: .center-image}

##### 활용
- 기존 클래스를 사용하고자 하는데 인터페이스가 맞지 않을 때
- 인터페이스가 맞지 않는 수정이 불가능한 라이브러리를 재사용하려 할 때
- (객체 적응자(`object adapter`)만 해당) 수정해야할 인터페이스가 현실적으로 불가능할 정도로 많을 때 객체 적응자를 써서 부모 클래스의 인터페이스를 변형

##### 결과
- 클래스 적응자(class Adapter)
 - `Adapter`는 명시적으로 `Adaptee`를 상속받고 있을뿐 `Adaptee의 서브클래스`들을 상속받는 것이 아니므로 서브클래스들에 정의된 기능을 사용할 수 없음
- 객체 적응자(object Adapter)
 - `Adapter`는 하나만 존재해도 여러 `Adaptee`와 동작 가능
 - `Adpatee`는 행위 재정의가 어려움

##### 잘 알려진 사용 예
- 그래픽 객체에서 `Graphic`과 `Line`, `Circle`, `Polygon`, `Spline`

---
## 2. Bridge
> 추상과 구현을 분리하여, 독립적인 다양성을 가질 수 있도록 함

##### 구조
![Bridge](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Bridge_UML_class_diagram.svg/500px-Bridge_UML_class_diagram.svg.png){: .center-image}

##### 활용
- `추상`과 `구현` 사이의 종속성을 제거하여 `런타임`에 구현 방법을 `선택`하거나 변경하고 싶을 때
- `추상`과 `구현` 모두 독립적으로 확장이 필요 할 때
- `구현` 내용을 변경한다고해서 `추상`이 다시 컴파일 될 필요 없어야 한다
- 클라이언트로부터 `구현`을 은닉하고 싶을 때
- 여러 객체들에 걸쳐 `구현`을 공유하며 이를 다른곳에 공개하지 않고 싶을 때

##### 결과
- 인터페이스와 구현의 분리 (컴파일 타임 의존성 제거)
- 확장성 제고 (`abstraction`과 `implementor`를 독립적 확장)
- 구현 세부 사항 은닉

##### 잘 알려진 사용 예
- Set은 집합에 대한 `추상`을 정의, LinkedSet과 HashSSet은 개념적 연결 리스트나 해시테이블에 대한 `구현`을 제공


---
## 3. Composite
> 부분과 전체의 계층을 표현하기 위해 객체들을 모아 트리 구조로 구성하여 개별 객체와 복합객체를 모두 동일하게 다룰 수 있도록 함

##### 구조
![Composite](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/Composite_UML_class_diagram_%28fixed%29.svg/2000px-Composite_UML_class_diagram_%28fixed%29.svg.png){: .center-image}

##### 활용
- `부분`, `전체`의 객체 계통을 표현하고 싶을 때
- 객체의 합성으로 생긴 `복합객체`와 `단일객체`의 사용을 동일하게 하고 싶을 때

##### 결과
- `단일객체`와 `복합객체`로 구성된 하나의 계통으로 정의하여 일관된 방식으로 애플리케이션 구성 가능
- 일관된 방식으로 구성가능하기 때문에 코드가 단순해짐
- 새로운 구성요소를 쉽게 추가 가능
- 지나치게 범용성을 갖게 되므로 구성요소에 대한 제약사항을 가하기 어려움

##### 잘 알려진 사용 예
- Smaltalk에서 MVC의 View가 `Composite` 이면서 뷰집합을 포함, 즉 `Component`이다
- 금융 분야에서 포트폴리오(portfolio)가 자산(asset)을 형성할 때, 포트폴리오는 각각 자신의 인터페이스를 만족하 전체를 하나의 집합으로 관리할 수 있는 Composite로 구현


---
## 4. Decorator
> 동적으로 새로운 책임을 추가하여, 서브클래스를 만드는것 보다 유연한 확장성 제공

##### 구조
![Decorator](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e9/Decorator_UML_class_diagram.svg/400px-Decorator_UML_class_diagram.svg.png){: .center-image}

##### 활용
- 다른 객체에 영향을 주지 않고 새로운 책임을 추가 가능
- 책임이 제거될 수 있을때 사용
- 모든 조합을 지원하기위해 상속해야할 클래스가 지나치게 많을 때

##### 결과
- 상속보다 유연한 설계로 확장성 제공
- 많은 기능이 상위 클래스에 누적되는 상황을 피할 수 있음
- `Decorator`와 해당 `ConcreteComponent`가 동일하지 않음. `Decorator`는 일관된 인터페이스를 제공하는 껍데기
- 객체의 `행동` 뿐만 아니라 `내부`도 변경 가능 (대표적인예 : `Strategy Pattern`)

##### 잘 알려진 사용 예
- GUI 툴킷에서 위젯의 기능을 추가하고자 할 때
- Stream Class (기본적인 I/O)


---
## 5. Facade
> 한 서브시스템을 합성한 여러 인터페이스의 집합에 대해 단순한 하나의 인터페이스를 제공

##### 구조
![Facade](https://upload.wikimedia.org/wikipedia/en/5/57/Example_of_Facade_design_pattern_in_UML.png){: .center-image}

##### 활용
- 복잡한 서브시스템에 단순한 인터페이스를 제공할 때
- 클라이언트와 서브시스템 간에 너무 많은 종속성이 존재할때, `Facade`를 통해 결합도를 줄이고 싶을 때
- 서브 시스템을 계층화하여

##### 결과
- 서브시스템의 구성요소 보호
- 클라이언트와 서브시스템 간의 결합도 약화
- `Facade`는 서브시스템을 완전히 막지 않기 때문에, 클라이언트가 `Facade`와 서브시스템을 선택할 수 있음

##### 잘 알려진 사용 예
- choices OS에서 다양한 H/W 플랫폼을 지원하기위해 FileSystemInterface와 주소 공간에 해당하는 Domain을 Facade로 정의


---
## 6. Flyweight
> 데이터를 서로 공유하여 사용하도록 하여 메모리 사용량을 최소화

##### 구조
![Flyweight](https://upload.wikimedia.org/wikipedia/ru/e/ee/Flyweight.gif){: .center-image}

##### 활용
- 애플리케이션이 대량의 객체를 사용할 때
- 객체의 수가 너무 많아져서 저장 비용이 너무 높을 때
- 대부분의 객체 상태를 부가적인 것으로 만들 수 있을 때
- 부가적인 속성을 제외하면 비교적 적은 수의 공유된 객체로 대체될 수 있을 때
- 애플리케이션이 객체에 식별자의 의존하지 않을 때

##### 결과
- 공유해야하는 인스턴스의 전체 수를 줄일 수 있음
- 객체별 상태의 양을 줄일 수 있음
- 부가적인 상태는 연산되거나 저장할 수 있음

##### 잘 알려진 사용 예
- Doc문서 편집기에서 하나의 `glyph`인스턴스를 만들어 특정 스타일의 문자로 공유할 수 있음 (본질적 상태는 글자의 코드와 스타일정보 부가적인 정보는 글자의 위치)
  - 그 결과 18만 글자를 저장하기 위해 단지 480개의 문자 객체만 저장하면 되었음
- ET++에서 룩앤필 독립성 보장
  - 스크롤바, 버튼 메뉴 등 위젯이라고 알려진 사용자 인터페이스 요소(layout)와, 음영 각도 등의 장식으로 분리


---
## 7. Proxy
> 객체에 대한 접근을 제어하기 위해서 그 객체를 담을 수 있는 그릇을 제공하는 것입니다

##### 구조
![Proxy](https://wiki.cdot.senecacollege.ca/w/imgs/Proxy.gif){: .center-image}

##### 활용
- remote proxy : 서로 다른 주소 공간에 존재하는 객체를 가르키는 객체
- virtual proxy : 요청이 있을 때만 필요한 고비용 객체를 생성
- protection proxy : 원래 객체에 대한 실제 접근을 제어
- smart reference : 실제 객체에 접근이 일어날 때 추가적인 행동을 수행
  1. 실제 객체에 대한 참조 횟수를 저장하다가 더는 참조가 없을 때 객체를 제거 (`smart pointer` 라고 불림)
  2. 맨 처음 참조되는 시점에 영속성 저장소의 객체를 메모리로 옮김
  3. 실제 객체에 접근하기 전에, 다른 객체가 그것을 변경하지 못하도록 실제 객체에 대한 잠금

##### 결과
프록시 패턴은 어떤 객체에 접근할 때 추가적인 작업을 할 수 있는 통로를 제공
- remote proxy는 객체가 다른 주소 공간에 존재한다는 사실을 숨길 수 있음
- virtual proxy는 요구에 따라 객체를 생성하는 등의 처리를 최적화 할 수 있음
- protection proxy / smart reference는 객체가 접근할 때 마다 생성 및 삭제 등의 추가 관리를 책임짐
