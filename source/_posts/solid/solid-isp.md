---
title: "SOLID - 인터페이스 분리 원칙 (ISP)"
date: 2017-06-05
categories:
 - Develop
 - SOLID
tags:
 - SOLID
---


## Interface segregation principle
> 클라이언트는 자신이 사용하는 메서드에만 의존해야 한다.
 - 사용하지 않는 의존성을 갖고 있다면 인터페이스 변경시 재컴파일/빌드/배포가 필요하므로 독립적인 개발/배포 불가능
 - 사용하는 기능만 제공하도록 인터페이스를 분리하여, 사이드이펙트 최소화
 - 클라이언트 입장에서 인터페이스를 분리


#### 흔하디 흔한 스타크래프트 예제
```java
interface Unit {
  void move();
  void attack();
}

class Marine implements Unit {
  @Override
  public void move() {
    // ... 생략
  }

  @Override
  public void attack() {
    // ... 생략
  }
}

class Medic implements Unit {
  @Override
  public void move() {
    // ... 생략
  }

  @Override
  public void attack() {
    // 메딕은 공격이 없는데??
  }
}
```

#### 개선된 코드
```java
interface Unit {
}

interface Movable {
  void move();
}

interface Attackable {
  void attack();
}

class Marine implements Unit, Movable, Attackable {
  // ... 동일하므로 생략
}

class Medic implements Unit, Movable {
  @Override
  public void move() {
    // ... 생략
  }

  // attack()에 대해 의존하지 않음
}
```

## 참고자료
 - [클린코더스 강의(14.3강 ISP)](https://www.youtube.com/watch?v=IIrjI7YUw6g&list=PLagTY0ogyVkIl2kTr08w-4MLGYWJz7lNK&index=17)
