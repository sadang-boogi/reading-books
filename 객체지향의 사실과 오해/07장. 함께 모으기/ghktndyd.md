# 07. 함께 모으기

### 객체 지향 설계의 세 가지 상호 연관된 관점

- **개념 관점**
    - 개념 관점에서의 설계는 도메인 안에 존재하는 개념과 개념들 사이의 관계를 표현한다.
    - 이 관점은 사용자가 도메인을 바라보는 관점을 반영하기에 실제 도메인의 규칙과 제약을 최대한 유사하게 반영하는 것이 핵심이다.
- **명세 관점**
    - 소프트웨어 안의 객체들의 책임에 초점을 맞춘다. (인터페이스)
    - 객체가 협력을 위해 **무엇을** 할 수 있는가에 집중한다.
- **구현 관점**
    - 객체들이 책임을 수행하는 데 필요한 동작하는 코드를 작성하는 것이다.
    - 객체의 책임을 **어떻게** 수행할 것인가에 초점을 맞추며 인터페이스 구현하는 데에 필요한 속성과 메서드를 클래스에 추가한다.

## 커피 전문점 도메인

- [우아한 형제들 블로그의 관련 글](https://techblog.woowahan.com/2502/)
- 커피 전문점에서 커피를 주문하는 과정을 설명

### 커피 전문점이라는 세상

- **도메인**: 커피 전문점
- 손님 객체, 메뉴 항목 객체, 메뉴판 객체, 바리스타 객체, 커피 객체로 구성

#### 객체들 간의 관계

- 손님은 메뉴판을 알아야 한다. ➡️ 손님과 메뉴판은 관계 존재
- 손님은 바리스타에게 주문을 해야 한다. ➡️ 손님과 바리스타 관계 존재
- 동적인 객체를 정적인 타입으로 추상화해서 복잡성을 낮추자

#### 타입

- **타입**: 상태와 무관하게 동일하게 행동할 수 있는 특정 인터페이스
- 타입은 클래스가 아닌 것을 꼭 이해할 것.

#### 타입간의 관계

- 메뉴 항목과 메뉴판의 관계는 **합성(composition) 관계**
- 왜냐하면 하나의 메뉴판 객체는 다수의 메뉴 항목 객체로 구성되어 있다.
- 이 둘은 절대 떨어지지 않고 하나처럼 움직인다.

- 손님과 메뉴판의 관계는 **연관(association)** 관계
- 왜냐하면 손님은 메뉴판을 알아야 커피를 주문할 수 있다.
- 그렇지만 메뉴판은 손님의 일부가 아니다. 그저 협력을 위해 서로 알고 있어야 하는 관계다.

> [!note] 참고
> 실제 도메인 모델을 작성하는 단계에서는 포함 관계와 연관 관계를 구분하는 것은 중요치 않다.
> 초점은 어떤 타입이 도메인을 구성하느냐와 타입들 사이에 어떤 상호작용이 존재하는지를 파악함으로써 도메인을 이해하는 것이 중요하다.

## 설계하고 구현하기

### 커피를 주문하기 위한 협력 찾기

- 협력을 설계할 때는 메세지가 객체를 선택하게 해야 한다.
- 즉, 메세지를 먼저 선택하고 그 후에 메세지를 수신하기에 적절한 객체를 선택해야 한다.

### 인터페이스 정리하기

- 객체가 수신한 메세지가 객체의 인터페이스를 결정한다.
- 메세지가 객체를 선택했고, 선택된 객체는 메세지를 자신의 인터페이스로 받아들인다.

### 타입을 만들기

- **오퍼레이션**: **`행위`** 라는 뜻이고, 일반적으로 부르는 `메서드`를 객체지향식으로 부르는 것 같다.
- 일반적인 방법은 클래스를 이용하는 것이다.
- 협력을 통해서 식별된 타입의 오퍼레이션은 외부에서 접근 가능하도록 `public`으로 선언돼 있어야 한다.

**커피전문점 세상을 코드로**

```java
class Customer {
    public void order(String menuName) {
    }
}

class MenuItem {
}

class Menu {
    public MenuItem choose(String name) {
        return null;
    }
}

class Coffee {
    public Coffee(MenuItem menuItem) {
    }
}

class Barista {
    public Coffee makeCoffee(MenuItem menuItem) {
        return null;
    }
}
```

### 구현하기

- 위 코드들로 인터페이스를 식별했다.
- 다음은 오퍼레이션(행위)를 수행하는 방법을 메서드로 구현하는 것이다.

**아메리카노를 주문하자**

1. 손님은 아메리카노를 주문하라는 요청을 받았다.
2. 손님은 **메뉴판**에서 아메리카노가 존재하는지 찾아야 한다.
3. 메뉴판에 아메리카노가 존재한다면 **바리스타에게 아메리카노를 주문**한다.
4. 바리스타는 아메리카노를 주문 받음과 동시에 아메리카노를 만들어야 하는 책임이 생겼으므로 **아메리카노를 만들어서 손님에게 건넨다.**

- 위 과정들을 코드로 작성해야 한다. 문제는 Customer가 어떻게 Menu와 Barista에게 접근하는지를 알아야 한다.
- Customer는 어떤 방법으로든 자신의 협력 대상인 Menu와 Barista의 참조를 알고 있어야 한다.

**참조를 아는 방법 : `order()` 메서드의 인자를 활용하자**

```java
class Customer {
    public void order(String menuName, Menu menu, Barista barista) {
        //Menu 참조, Barista 참조
        MenuItem = menu.choose(menuName);
        Coffee coffee = barista.makeCoffee(menuItem);
		...
    }
}
```

- 설계 작업은 구현을 위한 스케치이므로 너무 오랜 시간을 쏟지 말자
- 코드를 통한 피드백 없이는 깔끔한 설계를 얻을 수 없다.

**Menu의 책임**

- Menu는 `String menuName`가 MenuItem으로 존재하는지 찾아야 하는 책임이 생겼다.
- 이 책임을 수행하기 위해서는 Menu가 내부적으로 MenuItem을 관리해야 한다.
- 합성 관계를 떠올려보자.
- Menu의 `choose()` 메서드는 MenuItem의 목록을 하나씩 검사하면서 이름이 동일한 MenuItem이 있다면 반환한다.

```java
class Menu {
    private List<MenuItem> itmes;
    //MenuItem 포함 (MenuItem을 찾아야 하는 책임이 있기에)

    public Menu(List<MenuItem> items) {
        this.items = items;
    }

    public MenuItem choose(String name) {
        for (MenuItem each : items) {
            if (each.getName().equals(name)) {
                return each;
            }
        }
        return null;
    }
}
```

> [!note] 주목할 것
> - `List<MenuItem>` 를 `Menu`의 속성으로 포함시킨 결정은 클래스 구현 중에 결정됐다.
> - 객체의 속성은 내부 구현이므로 캡슐화돼야 한다.
> - 캡슐화 됐다는 것은 인터페이스에는 내부 속성에 대한 힌트가 제공돼서는 안된다는 것이다.
> - 이를 위해서 인터페이스를 정하는 단계에서는 속성을 고려하지 않는 게 좋다.

**Barista, Coffee, MenuItem**

```java
class Barista {
    public Coffee makeCoffee(MenuItem menuItem) {
        Coffee coffee = new Coffee(menuItem);
        return coffee;
    }
}

class Coffee {
    private String name;
    private int price;

    public Coffee(MenuItem menuItem) {
        this.name = menuItem.getName();
        this.price = menuItem.cost();
    }
}

public class MenuItem {
    private String name;
    private int price;

    public MenuItem(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public int cost() {
        return price;
    }

    public String getName() {
        return name;
    }
}
```

- Barista는 이제 MenuItem을 이용해서 커피를 제조한다.
- Coffee는 생성자를 통해서 자신을 생성할 수 있도록 한다.
- MenuItem은 가격과 이름을 반환하는 메서드로 응답할 수 있도록 한다.

##### 중요한 것

> - 설계를 간단히 끝내고 최대한 빨리 구현에 돌입하자.
> - 설계가 제대로 그려지지 않는다면 실제로 코드를 작성해가면서 전체적인 밑그림을 그려보자.
> - TDD가 먼저 코드를 작성해가면서 협력을 설계하는 방식 중 하나다.

## 코드의 세 가지 관점

### 코드는 세 가지 관점을 모두 제공해야 한다.

- **개념 관점**
    - 도메인 클래스와 소프트웨어 클래스의 간극을 좁히자.
    - 클래스가 도메인 개념을 최대한 수용하면 변경에 유연하게 대처할 수 있고, 유지보수성이 향상 된다.
- **명세 관점**
    - 클래스의 public 메서드는 공용 인터페이스를 드러낸다.
    - 변화에 안정적인 대처를 위해 인터페이스를 만들 때에는 구현과 관련된 세부사항을 너무 깊게 고려하지 말자.
- **구현 관점**
    - 클래스의 내부 구현
    - 클래스의 속성이나 메서드 구현이 외부 객체에 영향을 미쳐서는 안된다.

### 도메인 개념을 참조하는 이유

- 도메인 개념 안에서 객체를 선택한다면 도메인에 대한 지식을 기반으로 코드의 구조와 의미를 쉽게 유추 가능하다.

### 인터페이스와 구현을 분리하라

- 명세 관점은 클래스의 안정적인 측면을 드러내야 한다. 쉽게 변하지 않는
- 구현 관점은 클래스의 불안정한 측면을 드러내야 한다. 쉽게 변할 수 있는
- 구현 관점이 중요해 보이지만 실제로 훌륭한 설계를 결정하는 측면은 명세 관점인 객체의 인터페이스다.