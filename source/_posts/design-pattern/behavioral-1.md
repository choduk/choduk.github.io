---
title: "Design Pattern - 행동패턴 (1)"
date: 2017-05-15
categories:
 - Develop
 - Design Pattern
tags:
 - design pattern
 - behaviroal
---


## 행동 패턴이란?
##### 책임 및 알고리즘을 어떤 객체에 할당하는 것이 좋은지 다룬 패턴
> 객체간의 교류방법에 대해 정의하여 런타임에 수행하기 어려운 복잡한 제어 구조를 패턴화 한 것

---
## 1. Chain of Responsibility
> 메시지를 보내고 처리하는 객체들 간의 `커플링`를 없애기 위한 패턴, 각 핸들러는 자신이 처리할 수 있으면 처리하고, 처리할 수 없으면 다음 핸들러에게 요청을 넘긴다.

##### 구조
![Chain of Responsibility](http://fox.wikis.com/graphics/chain094.gif){: .center-image}

##### 활용
- 하나 이상의 객체가 요청을 처리해야하며 핸들러의 우선순위가 없으며, 각 객체가 자동으로 선택 되어야 할 때
- 리시버를 명시하지 않은 채 특정 객체에게 요청하고 싶을 때
- 요청을 처리할 수 있는 객체집합이 동적으로 정의되어야 할 때

##### 결과
- 객체 간의 `결합도(coupling)`가 줄어듬 (다른 객체가 요청을 어떻게 처리하는지, 연결구조가 어떻게 된지 몰라도 된다)
- 책임을 할당하는데 유연성을 높일 수 있음 (책임을 여러 객체에게 분산 시킬 수 있으며, 런타임에 추가/변경/확장 이 가능함)
- 어떤 객체가 처리할지 명시하지 않으므로, 메시지 처리에 대한 보장이 되질 않음

##### 잘 알려진 사용 예
- 클래스 라이브러리에서 이벤트 처리 (EventHandler)
- Logger


---
## 2. Command
> 하나의 요청을 하나의 객체로 캡슐화하여 파라미터처럼 사용할 수 있도록하며 요청을 되돌릴 수 있는 기능을 지원

##### 구조
![Command](https://upload.wikimedia.org/wikipedia/commons/5/52/Command_design_pattern.png){: .center-image}

##### 활용
- 수행할 행위를 파라미터로 만들고자 할 때 (절차지향 프로그래밍의 `Callback()` 과 동일)
- `Command`객체는 요청과 다른 `생명주기`를 가지므로 서로 다른 시간에 요청을

##### 결과
- 호출객체와 구현객체를 분리
- `Command`는 [일급객체](https://ko.wikipedia.org/wiki/%EC%9D%BC%EA%B8%89_%EA%B0%9D%EC%B2%B4) 이므로 다른 객체와 같은 방식으로 조작되고 확장할 수 있음
- 여러 `Command`를 조합해서 다른 `Command`를 만들 수 있음 (`Composite`패턴을 이용하여 여러 명령어 구성 가능)
- 새로운 `Command`를 추가하기 쉬움

##### 잘 알려진 사용 예
- GUI에서의 메뉴에서 사용자가 MenuItem을 선택하면 `Command`에 등록된 `Execute()`연산을 호출함


---
## 3. Interpreter
> 어떤 언어에 대해, 문법에 대한 표현을 정의하면서 표현을 사용하여 해당 언어로 기술된 문장을 해석하는 해석자를 함께 정의

##### 구조
![Interpreter](https://upload.wikimedia.org/wikipedia/en/0/03/Interpreter_UML_class_diagram.jpg){: .center-image}

##### 활용
- 정의할 언어의 문법이 간단함
- 효율성은 고려사항이 아님
   - 가장 효율저인 `interpreter`를 구현하는 방법은 파스 트리를 직접 처리하는 것이 아니라, 일차적으로 파스트리를 다른 형태로 `번역(translate)` 시키는것
   - 예를들어, 정규표현식은 일반적으로 유한상태기계(fninite state machine)으로 변역함

##### 결과
- 문법을 확장하거나 바꾸기 쉽움
- 문법의 구현이 쉽움
- 복잡한 문법은 관리하기 어려움
- 새로운 표현식의 해석방법을 추가할 수 있음

##### 잘 알려진 사용 예
- 객체지향 컴파일러
- `Composite`패턴이 사용되는 곳에 `Interpreter`패턴을 사용할 수 있음, (단 `Composite` 패턴으로 정의한 클래스들이 하나의 언어 구조를 정의할 때만 `Interpreter`라고 불림)


---
## 4. Iterator
> 내부구조를 노출하지 않고, 집합 객체(aggregate object)에 속한 원소들을 순차적으로 접근할 수 있는 방법을 제공

##### 구조
![Iterator](https://upload.wikimedia.org/wikipedia/commons/c/ca/Iterator_design_pattern.png){: .center-image}

##### 활용
- 객체 내부 표현 방식을 모르고, 집합 객체의 각 원소들에 접근하고 싶을 때
- 집합 객체를 순회하는 다양한 방법을 지원하고 싶을 때
- 다른 집합 객체 구조에 대해서도 동일한 인터페이스를 제공하고자 할 때

##### 결과
- 집합 객체의 다양한 순회방법을 제공
- `Iterator`는 `Aggregate`클래스의 인터페이스를 단순하게 만듬
- 집합 객체에 따라 여러 순회 방법을 제공

##### 잘 알려진 사용 예
- 컬렉션 클래스


---
## 5. Mediator
> 객체들간의 상호작용을 캡슐화하여 하나의 객체에 정의 (참조 관계를 직접 정의하기보다는 독립된 다른객체가 관리)

##### 구조
![Mediator](https://upload.wikimedia.org/wikipedia/commons/e/e4/Mediator_design_pattern.png){: .center-image}

##### 활용
- 여러 객체가 복잡한 상호작용을 가질 때 (객체간 의존성이 구조화 되어 있지 않고, 이해하기 어려울 때)
- 한 객체가 다른 객체를 너무 많이 참조하고 너무 많은 일을 수행하여 재사용하기 어려울 때
- 여러 클래스에 분산된 행동들이 상속없이 수정되어야 할 때

##### 결과
- 서브클래싱 제한함
- `Colleague`객체 사이의 종속성 줄임
- 객체 프로토콜을 단순하게 만듬
- 객체 간의 협력 방법을 추상화함
- 통제의 집중화 (`Mediator` 클래스 자체의 유지보수가 어려워질 수 있음)

##### 잘 알려진 사용 예
- Windows 플랫폼용 스몰토크/V의 응용프로그램에서 `pane` 클래스 계통의 객체들이 많이 제공되는데 (`TextPane`, `ListBox`, `Button` 등), 이들은 별다른 상속없이도 ViewManager 클래스만 상속받으면 된다.
- ![사진](http://nekenciu.tv/oot/files/DP/hires/Pictures/media032.gif){: .center-image}{: .center-image}

#### [스몰토크/V 에서의 Mediator](https://sourcemaking.com/design_patterns/mediator)


---
## 6. Memento
> 캡슐화를 위배하지 않을 채 어객체의 내부 상태를 캡처해두고, 이후 그 상태로 되돌아올 수 있도록 함

##### 구조
![Memento](https://upload.wikimedia.org/wikipedia/commons/1/18/Memento_design_pattern.png){: .center-image}

##### 활용
- 객체 상태에 대한 스냅샷을 저장한 후 상태를 되돌릴 때
- 상태를 얻기위한 인터페이스를 두면 객체의 세부 구현이 드러나서 캡슐화가 위배될 때

##### 결과
- 캡슐화된 경계를 유지할 수 있음
- `Originator` 클래스가 단순해짐
- `Memento`의 사용으로 더 많은 비용이 발생할 수 있음
- 제한적 / 광범위한 인터페이스를 정의해야함 (어떤 언어에서는 `Originator` 객체에서만 `Memento`의 상태 접근을 보장하기 어려울 수 있음)
- `Caretaker`는 자신이 보관한 `Memento`를 삭제할 책임이 있음으로, `Memento`를 관리하는데 추가적인 비용이 들어갈 수 있음
