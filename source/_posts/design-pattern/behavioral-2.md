---
title: "Design Pattern - 행동패턴 (2)"
date: 2017-05-19
categories:
 - Develop
 - Design Pattern
tags:
 - design pattern
 - behaviroal
---


## 7.Observer
> 객체 사이에 1:N 의존관계를 맺고있으며, 한 객체의 상태가 변경되면 의존적인 다른 모든 객체들에게 통지하여 상태를 갱신할 수 있는 패턴

##### 구조
![Observer](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8d/Observer.svg/500px-Observer.svg.png){: .center-image}

##### 활용
- 두개의 추상화 개념이 있고 하나가 다른 하나에 대해 종속적일때, 각각을 별도의 객체로 캡슐화하여 재사용할 수 있음
- 한 객체에 변경으로 인해 다른 객체를 변경해야하는데, 이때 얼마나 많은 객체들이 변경되어야 할지 모를 때
- 한 객체가 자신이 변화를 통보하지만 이 변화에대해 관심있는 객체들이 누구인지 몰라도 될 때

##### 결과
- `Subject`와 `Observer` 간에 추상적인 결합도만 존재함
- `브로드캐스트(broadcase)` 방식의 커뮤니케이션 가능
- 예측하지 못한 정보를 갱신할 수 있음

##### 잘 알려진 사용 예
- 스프레드시트와 바 차트


---
## 8. State
> 객체의 내부 상태에 따라 스스로 행위를 변경하도록 하는 패턴 (마치 클래스가 바뀌는 것처럼 보인다)

##### 구조
![State](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e8/State_Design_Pattern_UML_Class_Diagram.svg/400px-State_Design_Pattern_UML_Class_Diagram.svg.png){: .center-image}

##### 활용
- 런타임에 객체의 행동이 상태에 따라 달라져야할 때
- 객체의 상태를 `나열형 상수(enumerated constant)`로 정의해야 할때, 이를 각각의 객체로 정의하여 다른 객체들과 상관없이 그 객체의 상태를 다양화 함

##### 결과
- `if`, `switch-case` 을 통해 상태별 행위를 처리하지 않고, 서로 다른 상태에 대해 별도의 객체로 표현하여 상태에 따른 행위를 국소화할 수 있음
- 상태 전이를 명확하게 만듬 (`Context`객체가 일관되지 않은 상태가 되는것을 막아 줌)
- 상태객체는 **공유**될 수 있음 (행동만 있는 `Flyweight` 처럼 보임)

##### 잘 알려진 사용 예
- TCP 프로토콜의 연결
- 포토샵과 같은 도구에서 `선택도구`를 이용한 방법 (그림관련 도구 활성화 -> 그림 그려짐, 선택도구 활성화 -> 그림 선택)


---
## 9. Strategy
> 동일 계열의 알고리즘을 정의하고 각 알고리즘을 캡슐화하여 이들을 상호교환 되도록 한다
알고리즘을 사용하는 클라이언트와 상관없이 독립적으로 알고리즘을 다양하게 변경할 수 있게 한다

##### 구조
![Strategy](https://upload.wikimedia.org/wikipedia/commons/thumb/0/08/StrategyPatternClassDiagram.svg/600px-StrategyPatternClassDiagram.svg.png){: .center-image}

##### 활용
- `Strategy`패턴은 많은 행동 중 하나를 가진 클래스를 구성할 수 있는 방법을 제공
- 알고리즘의 변형이 필요할 때
- 사용자가 몰라야 하는 데이터를 사용하는 알고리즘이 있을 때 노출하지 말아야 할 복잡한 자료구조를 `Strategy`에 캡슐화 한다
- 하나의 클래스가 많은 행동을 정의하고, 그 클래스의 연산 안에서 복잡한 다중 조건문의 모습을 취하는 경우 많은 조건문보다는 각각의 `Strategy`클래스로 바꾸는것이 좋다.

##### 결과
- 동일 계열의 관련 알고리즘군이 생김
- 서브클래싱을 사용하지 않는 대안 (context와 무관하게 알고리즘 변경 가능)
- 조건문을 없앨 수 있음
- 구현의 선택가능
- 클라이언트가 서로 다른 전략을 알고 있어야 함
- `Strategy`객체와 Context` 객체 사이에 메시지 오버헤드가 있음 (모든 `ConcreteStrategy` 클래스는 `Strategy`인터페이스를 공유하기 때문에 사용되지 않는 파라미터를 초기화 경우도 있음)
- 객체 수 증가


---
## 10. Template Method
> 어떤 작업을 수행하는 알고리즘의 뼈대만 정의하고, 각 스탭별로 수행하는 구체적인 처리는 서브클래스에게 위임

##### 구조
![Template Method](https://upload.wikimedia.org/wikipedia/commons/b/b5/Template_Method_design_pattern.png){: .center-image}

##### 활용
- 어떤 알고리즘에 변하는 부분과 변화지 않는 부분을 분리하여, 뼈대만 작성하고(변하지 않는 부분) 구현은(변하는 부분) 서브클래스에게 위임할 때
- 서브클래스간에 중복된 부분을 추출하여 공통 클래스에 몰아둠으로써 중복을 제거하고자 할때(`refactoring to generalize`의 좋은 예)
- 서브클래스 확장을 제어할 수 있음 (특정시점에 `hook`을 호출하도록 정의하여 그 시점에만 확장 가능하도록 할 수 있음)

##### 결과
- `Hollywood principle`(Don't call us, we'll call you)와 같은 역전된 제어 구조가 나타남
- `Template Method`는 아래 operation 중 하나를 호출함
  1. Concrete operation
  2. Abstract class
  3. Factory method
  4. hook operation

##### 잘 알려진 사용 예
- 거의 모든 추상 클래스에서 사용할 정도로 다양하게 쓰임


---
## 11. Visitor
> 객체 구조를 이루는 원소에 대해 수행할 연산을 표현, 연산을 적용할 원소의 클래스를 변경하지 않고도 새로운 연산을 정의할 수 있음

##### 구조
![Visitor](https://www.codeproject.com/KB/architecture/csdespat_6/visitor.gif){: .center-image}

##### 활용
- 객체 구조가 다른 인터페이스를 가진 클래스를 포함하고 있어서 구체 클래스에 따라 오퍼레이션을 수행하고자 할때
- 각각 특징이 있고 관련되지 많은 오퍼레이션들이 어떤 객체에 대해 수행해야 하지만, 오퍼레이션때문에 객체가 복잡하게 되고 싶지 않을 때
- 객체 구조를 정의한 클래스는 변경되지 않지만, 전체 구조에 걸쳐 새로운 연산을 추가하고 싶을 때 구조를 변경하면 인터페이스 변경이 필요하므로 비용이 더 커질 수 있다. (이경우 해당 연산을 클래스에 정의하는 것이 나음)

##### 결과
- 새로운 오퍼레이션 추가가 쉬움
- `Visitor`를 통해 관련된 연산을 한 군데로 모아, 관련되지 않은 오퍼레이션을 뗴어낼 수 있음
- 새로운 `ConcreteElement` 클래스를 추가하기 어려움
- 클래스 계층 구조에 걸쳐서 방문함
- 상태를 누적할 수 있음
- 캡슐화를 위반할 수 있음
- `Visitor`패턴 적용시 이슈
  1. `Double Dispatch` : 실행되는 오퍼레이션이 요청의 종류와 수신자의 타입에 따라 달라짐 (방문자는 원소의 각 클래스에 대해 서로 다른 연산을 요청할 수 있음)
  2. 누가 객체 순회를 책임지는가?

##### 잘 알려진 사용 예
- Smalltalk-80 컴파일러 (소스코드 분석시에 사용)