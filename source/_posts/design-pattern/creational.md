---
title: "Design Pattern - 생성패턴"
date: 2017-05-09
categories:
 - Develop
 - Design Pattern
tags:
 - design pattern
 - creational
---

## 생성 패턴이란?
##### 인스턴스의 생성/합성 방법을 추상화하여 시스템이 어떤 서브클래스를 사용하지에 대한 정보를 캡슐화
> **누가**, **언제**, **무엇을**, **어떻게** 생성할 것인지 결정하는 데 유연성을 확보할 수 있다

---
## 1. Factory Method
> 객체를 생성하기 위해 인터페이스를 정의하지만, 어떤 인스턴스를 생성할지에 대한 결정은 서브클래스에게 위임

##### 구조
![Factory method](https://upload.wikimedia.org/wikipedia/commons/8/8e/Factory_Method_UML_class_diagram.svg)

##### 활용
- 생성할 객체 타입을 예측할 수 없을 때
- 생성할 객체를 기술해야하는 책임을 서브클래스에게 위임하고자 할 때
- 객체 생성의 책임을 서브클래스(ConcreteProduct)에 위임하고, 서브클래스에 대한 정보를 은닉할 필요가 있을 때

##### 결과
- 컴파일 타임에는 인터페이스/추상클래스에(Product) 의존성을 맺지만, 런타임에는 서브클래스에(ConcreteProduct) 의존성을 갖게되므로 느슨한 결합(loose coupling)을 할 수 있음
- 느슨한 결합을 통해, 새로운 서브클래스(ConcreteProduct)를 쉽게 추가할 수 있음
- 서브클래스(ConcreteProduct)에 대한 hook 메소드 제공 (객체 생성 전후로 어떤 행위를 추가하기 쉬움)
- 병렬적인 클래스 계통을 연결하는 역할을 담당 (자신의 책임을 다른 클래스에 위임하는것)

---
## 2. Abstract Factory
> 독립적인 여러 제품군(AbstractProduct)을 생성할 수 있는 인터페이스(AbstractFactory)를 제공

##### 구조
![Abtract Factory](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9d/Abstract_factory_UML.svg/677px-Abstract_factory_UML.svg.png)

##### 활용
- 서브 클래스의 생성/구성/표현 방식에 의존하지 않는 프로그램 작성할때
- 특정 Product 하나를 애플리케이션 설정과 함께 다른 Product으로 대체할 때
- 어떤 Product의 제약사항을 외부에서도 따르게 하고자 할때
- Product에 대한 클래스 라이브러리를 제공하고, 구현이 아닌 인터페이스만 노출하고자 할 때

##### 결과
- Product의 생성/구성/표현 방식에 대한 전반적인 과정을 캡슐화하여 서브 클래스에 위임했기 때문에, 구체적인 클래스를 분리되어 있음
- 애플리케이션내에 팩토리의 서브클래스(ConcreteFactory)를 변경하기 쉬기 때문에, 제품군(AbstracProduct)을 쉽게 대체 가능
- 오직 팩토리의 서브클래스(ConcreteFactory)에서만 객체를 생성함으로, 애플리케이션내에 Product 객체들의 일관성을 높일 수 있음
- 새로운 Product를 제공하기 어려움 (AbstractFactory가 영향받으면 하위 모든 서브클래스가 영향을 받게됨)

---
## 3. Prototype
> 인스턴스를 복사해서 새로운 인스턴스를 만드는 것

##### 구조
![Prototype](https://upload.wikimedia.org/wikipedia/commons/thumb/1/14/Prototype_UML.svg/600px-Prototype_UML.svg.png)

##### 활용
- 인스턴스화 할 클래스를 런타임에 지정하고 싶을 때
- 팩토리에서와 같이 Product와 Factory를 병렬적으로 만들고 싶지 않을 때
- 클래스의 인스턴스가 구성/표현 방식이 복잡할 때
- 프레임워크와 인스턴스를 분리하고 싶을 때
- Composite, Decorator 패턴이 많이 사용될 때

##### 결과
- 런타임에 새로운 Product를 추가/삭제 할 수 있음
- 새로운 클래스의 생성 없이 객체에 정의된 필드에 따라 행위를 변경 가능
- 구조를 다양화 하여 새로운 객체 정의 가능
- 서브클래스의 수를 줄일 수 있음
- 동적으로 클래스에 따라 애플리케이션 구성 가능

---
## 4. Singleton
> 오직 한개의 클래스 인스턴스만 갖도록 보장하고, 전역적인 접근을 제공

##### 구조
![Singleton](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fb/Singleton_UML_class_diagram.svg/1200px-Singleton_UML_class_diagram.svg.png)

##### 활용
- 클래스의 인스턴스가 오직 하나이면서, 전역적인 접근이 필요 할 때
- 유일한 인스턴스가 서브클래싱으로 확장되어야 하며, 사용자는 코드의 수정 없이 확장된 서브클래스의 인스턴스를 사용할 수 있어야 할 때

##### 결과
- 유일하게 존재하는 인스턴스로 전역적 접근 통제
- namespace의 제한
- 연산 및 표현의 정제 허용
- 인스턴스의 개수를 변경하기가 자유로움
- 클래스 연산을 사용하는 것보다 훨씬 유연
- 클래스로더가 여러개 있다면, 클래스 로더를 직접 지정해서 사용할 것(여러개의 인스턴스 생성 가능)
- 고전적인 방식의 lazy instantiation 생성시 멀티스레드 상황에서 문제가 생길 수 있으니 주의할것
  - 해결법은 다음과 같다.
  1. 속도의 문제가 없다면 synchronized 사용
  2. DCL(Double-checking locking)을 사용하여 getInstance()의 동기화 부분을 줄임
  ```java
  private Singleton instance;
  public static Singleton getInstance(){
    if (instance == null){
      synchronized (Singleton.class){
        if(instance == null)
          instance = new Singleton();
      }

  		return instance;
  	}
  ```
  3. 인스턴스를 처음부터 생성 (*권장사항*)
    ```java
    private static Singleton instance = new Singleton();
    ```
  4. 특별한 문제가 없다면
- Singleton으로 만들어진 객체는 반드시 추가적인 인스턴스 생성을 못하게 막아야 함

---
## 5. Builder
> 복잡한 객체를 생성 및 표현방법을 분리하여, 서로 다른 표현이라도 객체를 생성할 수 있는 동일한 방식을 제공

##### 구조
![Builder](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f3/Builder_UML_class_diagram.svg/1200px-Builder_UML_class_diagram.svg.png)

##### 활용
- 복합객체 생성 방법이, 각 요소들의 조합 방법에 독립적일 때
- 합성할 객체들의 표현방법이 다르더라도, 생성과정에서 이를 지원해야 할 때

##### 결과
- 객체 내부 표현을 다양하게 변화 가능
- 생성과 표현의 코드를 분리
- 복합 객체 생성 절차를 세밀하게 분류 가능