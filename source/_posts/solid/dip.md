---
title: "SOLID - 의존관계 역전 원칙 (DIP)"
date: 2017-06-16
categories:
 - Develop
 - SOLID
tags:
 - SOLID
---


## Dependency inversion principle
> 상위레벨 모듈이 하위레벨 모듈에 의존하면 안된다.
 - 상위/하위 모듈 모두 추상화에 의존해야 한다.
 - 추상화는 구현체를 의존하면 안된다.

##### 중요한점
 - OO의 핵심이다.
 - IoC를 통해 상위래밸의 모듈을 하위레벨 모듈로 부터 보호하는 것이다.
 - 이를 통해 OCP를 지키고, 새로운 요구사항을 반영 가능

##### 예제
 - let) 편의상 Object A => A, Object B => B, interface A => a 라고 하자...
![Dependency inversion](https://upload.wikimedia.org/wikipedia/commons/9/96/Dependency_inversion.png){: .center-image}
 - Figure1 그림에서, A -> B 방향으로 `Compile time` 의존성과 `Run time` 의존성을 갖고있다.
 - Figure2 그림에서, A,B -> a 방향으로 `Compile time` 의존성을 맺었지만, `Run time` 의존성은 그대로이다.
 - 즉, 더이상 A는 B로 의존하지 않으며, A,B 둘다 `Compile time`에 a에 의존하게 된다.
 - 이때, B의 `Compile time`의존성 방향이, A의 `Run time` 의존성 방향과 반대가 되게 된다.
###### 이와같이 소스 코드의 의존성 방향이 제어의 흐름과 반대 방향이 될 때, 의존성이 역전됐다고 한다.

> ###### DIP는 `Run time`의존이 아닌 `Compile time (source code)`의존을 역전시킴으로써 변경의 유연함을 확보할 수 있도록 만들어주는 원칙이다.

 - 위 예제에서 `Package`를 보면, DIP가 적용됨에 따라, `interface`를 `Package A`에서 가져가게 되었다.
 - 즉, `interface` 타입을 가져감으로써 A와 B를 독립적으로 배포(jar, DLL 등)할 수 있도록 만들어 준다.

## 참고자료
 - [개발자가 반드시 정복해야 할 객체 지향과 디자인 패턴](http://www.yes24.com/24/goods/9179120?scode=029)
 - [클린코더스 강의(15.1강 DIP)](https://www.youtube.com/watch?v=mI1PsrgogCw&index=18&list=PLagTY0ogyVkIl2kTr08w-4MLGYWJz7lNK)
