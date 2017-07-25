---
title: "SOLID - 개방 폐쇄 원칙 (OCP)"
date: 2017-06-03
categories:
 - Develop
 - SOLID
tags:
 - SOLID
---


## Open-closed principle
> 확장에는 열려 있고 변경에는 닫혀 있어야 한다.
 - (변화하는 부분을 추상화하여) 기능에 대한 변경/확장이 쉬워야한다. (확장 : 새로운 타입을 추가)
 - 기능을 사용하는 코드는 고정된 추상(`abstract`, `interface`)에 의존하므로 소스코드의 수정이 이뤄지면 안된다.

### OCP를 위배하는 전형적인 특징
 1. 다운 캐스팅
    ```java
    // 개발자가 반드시 정복해야 할 객체 지향과 디자인패턴(114page 예제)
    public void drawCharacter(Character charcater) {
        if (character instanceof Missile) {
            Missile missile = (Missile) charcter;
            missile.drawSpecific();
        } else {
            character.draw();
        }
    }
    ```
 2. 비슷한 종류의 if-else 블록이 존재

---
### OCP의 현실적인 문제들 ***(이론적으로 OK, 비실용적)***

 - 문제점
   - main partition에는 if-else 존재
   - Crystal ball problem (마법의 수정구 문제)
     1. 확장을 위해 모든 인터페이스를 준비해놓는 것은 현실적으로 불가능(복잡해짐)
     2. 고객은 만져보기위해 자기가 원하는게 뭔지 모른다.
     3. 고객은 예측하지 못한 기능추가 변경을 요구 (Unknown Unknowns)
 - 해결책
   1. BDUF (Big Design Up Front)
     - 요구사항을 예측하여 과도한 설계로 인해 발생하는 문제점
     - 대부분 필요치 않은 추상화로 매우 크고 무겁고 복잡해짐
     - 추상화는 유용하고 강력한 만큼 비용도 큼
   2. Agile Design
     - 변화에 대한 가장 좋은 예측은 경험하는 것
     - 예측하고 추상화하기 전에, 고객이 변경을 요구할때 추상화를 한다.
     - 주단위 정도로 간단한 뭔가를 Delivery, 변경을 요구하면 코드를 리팩토링



## 참고자료
 - [개발자가 반드시 정복해야 할 객체 지향과 디자인 패턴](http://www.yes24.com/24/goods/9179120?scode=029)
 - [클린코더스 강의(14.1강 OCP)](https://www.youtube.com/watch?v=dqa-IdafeIE)
 - [위키백과](https://ko.wikipedia.org/wiki/%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84_%EC%9B%90%EC%B9%99)
