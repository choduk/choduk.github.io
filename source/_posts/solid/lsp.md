---
title: "SOLID - 리스코프 치환 원칙 (LSP)"
date: 2017-06-04
categories:
 - Develop
 - SOLID
tags:
 - SOLID
---


## Liskov substitution principle
> 상위타입 객체를 사용하는 프로그램은 상위타입 대신 하위타입 객체를 사용해도 클라이언트의 수정 없이 정상적으로 동작해야 한다.

#### LSP 위반 사례
 - 명세에 벗어난 값 리턴
 - 명세에 벗어난 익셉션 발생
 - 명세에 벗어난 기능 수행
 - instanceof / downcasting을 사용한것은 전형적인 LSP 위반

##### LSP의 대표적인 예(직사각형-정사각형 문제)
```java
class Rectangle {
  private int width;
  private int height;

  public void setWidth(int width) {
    this.width = width;
  }

  public void setHeight(int height) {
    this.height = height;
  }

  public int getWidth() {
    return width;
  }

  public int getHeight() {
    return height;
  }
}

// 직사각형을 상속받아 정사각형을 만들었을때
class Square extends Rectangle {
  @Override
  public void setWidth(int width) {
    super.setWidth(widht);
    super.setHeight(widht);
  }

  @Override
  public void setHeight(int height) {
    super.setWidth(height);
    super.setHeight(height);
  }
}
```

### 위 코드의 문제점
 - LSP를 위반하는 부분
     ```java
     void increaseHeight(Rectangle rec) {
        if(rec.getHeight() <= rec.getWidth()) {
          rec.setHeight(rec.getWidth() + 10);
        }
     }
     ```
 - 위와같은 코드를 작성하고 파라미터로 `Square`를 넘겼을때 `width`, `height`가 모두 증가하게 된다. 즉, `height`만 증가 시킬 수 없다
 - 떄문에, `if(rec instance of Square)` 와 같은 코드를 넣어서 막아야 한다
 - 이는 상위타입이 하위타입을 대체하지 못해서 코드의 수정이 이뤄졌으므로, LSP를 위반한다.

## 참고자료
 - [개발자가 반드시 정복해야 할 객체 지향과 디자인 패턴](http://www.yes24.com/24/goods/9179120?scode=029)
 - [클린코더스 강의(14.2강 LSP)](https://www.youtube.com/watch?v=OfVwuWJSHOY&list=PLagTY0ogyVkIl2kTr08w-4MLGYWJz7lNK&index=16)
