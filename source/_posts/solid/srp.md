---
title: "SOLID - 단일 책임 원칙 (SRP)"
date: 2017-05-29
categories:
 - Develop
 - SOLID
tags:
 - SOLID
---


## Single responsibility principle
> 클래스는 하나의 책임만 가지며 클래스는 그 책임을 완전히 캡슐화 해야 한다.


##### 여러 책임을 갖고 있는 DataViewer 클래스
```java
// 개발자가 반드시 정복해야 할 객체 지향과 디자인패턴(106page 예제)
class DataViewer {

    public void display() {
        String data = loadHtml();
        updateGui(data);
    }

    public String loadHtml() {
        HttpClient client = new HttpClient();
        client.connect(url);
        return client.getResponse();
    }

    private void updateGui(String data) {
        GuiData guiModel = parseDataToGuiData(data);
        tableUI.changeData(guiModel);
    }

    private GuiData parseDataToGuiData(String data) {
        // 파싱 처리 코드
    }
}
```

### 위 코드의 문제점
 - 하나의 책임이 다른 책임까지 영향을 준다 (`data`를 가져오는 방식이 `HTTP`방식에서 다른 방식으로 변경된다면, 관련된 모든 코드가 수정되어야 한다.)
 - `data`를 가져오는 새로운 방식이 추가됐다면, 사용하지 않더라도 불필요한 `package`를 포함되어야 한다.

### 개선방안
 1. `DataLoader`, `DataDisplayer` 두가지 책임을 분리한다.
 2. 둘 간에 주고 받을 데이터를 `String`이 아닌 추상화된 타입으로 변경한다.

#### 개선된 코드 1
```java
class Data {
    ...
}

class DataLoader {
    public Data load() {
        ...
    }
}

class DataDisplayer {
    public void display(Data data) {
        ...
    }
}

class DataViewer {

    private DataLoader loader = new DataLoader();
    private DataDisplayer displayer = new DataDisplayer();

    public void display() {
        Data data = loader.load();
        displayer.display(data);
    }

    public String loadData() {
        Data data = loader.load();
        return dataToString(data);
    }
}

```

### 남아있는 문제점
 - `DataViewer`가  `DataLoader`, `DataDisplayer` 두 클래스와 `Composition`을 관계를 형성하므로, 강한 `coupling`이 존재한다.
 - `DataLoader`, `DataDisplayer` 두 클래스가 변경되면, `DataViewer` 클래스가 영향을 받는다.

### 개선방안
 - `DataLoader`, `DataDisplayer`를 `interface`로 변경하여 의존성을 역전시킨다.

##### 개선된 코드 2
```java
class Data {
    ...
}

interface DataLoader {
    Data load();
}

interface DataDisplayer {
    void display(Data data);
}

class DataViewer {

    private final DataLoader loader;
    private final DataDisplayer displayer;

    // 외부에서 의존성 주입
    public DataViewer(DataLoader loader, DataDisplayer displayer) {
        this.loader = loader;
        this.displayer = displayer;
    }

    public void display() {
        Data data = loader.load();
        displayer.display(data);
    }

    public String loadData() {
        Data data = loader.load();
        return dataToString(data);
    }
}

```

---
### 책임을 나누는 방법
 1. 많은 프로그래밍 경험을 바탕으로 감각적으로...(-_-;;)
 2. 매서드를 실행하는 주체(`Actor`)를 확인해 본다

```java
class GUIApplication {
    ...
}

class DataProcessor {
    ...
}

class DataViewer {
    public void display() {
        ...
    }

    public String loadData() {
        ...
    }
}
```

> 위와 같은 상황이라면, `GUIApplication` 클래스는 `DataViewer.display()` 매서드를, `DataProcessor`클래스는 `DataViewer.loadData()` 매서드가 필요하므로, 두 매서드는 각각 다른 책임이므로 분리해야할 가능성이 높다.

## 참고자료
 - [개발자가 반드시 정복해야 할 객체 지향과 디자인 패턴](http://www.yes24.com/24/goods/9179120?scode=029)
 - [클린코더스 강의(13강 SRP)](https://www.youtube.com/watch?v=AdANHDp5dTM&t=1314s)
