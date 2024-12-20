# Effective_Java_Study


## 📖 상속을 고려해 설계하고 문서화하라. 그러지 않았다면 상속을 금지하라

### 핵심 정리

상속용 클래스를 설계하기란 결코 만만치 않다. 

클래스 내부에서 스스로를 어떻게 사용하는지 모두 문서로 남겨야 하며, 일단 문서화한 것은 그 클래스가 끄이는 한 반드시 지켜야 한다.

그렇지 않으면 그 내부 구현 방식을 믿고 활용하던 하위 클래스를 오동작하게 만들 수 ㅇ ㅣㅆ다.

다른 이가 효율 좋은 하위 클래스를 만들 수 있도록 일부 메서드를 protected로 제공해야 할 수도 있다.

그러니 클래스를 확장해야 할 명확한 이유가 떠오르지 않으면 상속을 금지하는 편이 나을 것이다.

**상속을 금지하려면 클래스를 final로 선언하거나 생성자 모두를 외부에서 접근할 수 없도록 만들면 된다.**


## 💡 주요 내용 정리 & 🛠️ 실습 코드


### 상속용 클래스는 재정의할 수 있는 메서드들을 내부적으로 어떻게 이용하는지(자기사용) 문서로 남겨야 한다.

### 클래스의 내부 동작 과정 중간에 끼어들 수 있는 훅(hook)을 잘 선별하여 protected 메서드 형태로 공개햐야 할 수도 있다.

### 상속용 클래스를 시험하는 방법은 직접 하위 클래스를 만들어보는 것이 유일하다

꼭 필요한 protected 멤버를 놓쳤다면 하위 클래스를 작성할 때 그 빈자리가 확연히 드러난다.

거꾸로, 하위 클래스를 여러 개 만들 때까지 전혀 쓰이지 않는 protected 멤버는 사실 private 이었어야 할 가능성이 크다.

널리 쓰일 클래스를 상속용으로 설께한다면 문서화한 내부 사용 패턴과, protected 메서드와 필드를 구현하면서 선택한 결정에 책임져야 함을 인식해야 한다.

이 결정들이 그 클래스의 성능과 기능에 영원한 족쇄가 될 수 있다.

그러니 **상속용으로 설계한 클래스는 배포 전에 반드시 하위 클래스를 만들어 검증해야 한다.**


###  상속용 클래스의 생성자는 직접적으로든 간접적으로든 재정의 가능 메서드를 호출해서는 안 된다.

상위 클래스의 생성자가 하위 클래스의 생성자보다 먼저 실행되므로 하위 클래스에서 재정의한 메서드가 하하위 클래스의 생성자보다 먼저 호출된다!

이때 그 재정의한 메서드가 하위 클래스의 생성자에서 초기화하는 값에 의존한다면 의도대로 동작하지 않을 것이다!


## 추가 강의 내용 정리

### 핵심 내용

상속을 고려해 설계하고 문서화하라. 그러지 않았다면 상속을 금지하라

### 상속을 금지하는 방법

상속을 고려해 설계하고 문서화하라. 그러지 않았다면 상속을 금지하라 -> 어지간 하면 하지 마라

1. Class를 final로 선언하는 방법

```java
final public class ProhibitInheritance {
    
}
```

2. 모든 생성자를 private or package-private로 선언하고 public static factory로 만드는 법

```java
@Getter
public class ProhibitInheritance {
    private int sum;
    private ProhibitInheritance() {}
    private ProhibitInheritance(int num) {this.sum = sum;}
    
    public static ProhibitInheritance getInstance() {
        return new ProhibitInheritance();
    }
}
```

### 올바르게 상속을 고려하려면

@implSpec을 통해 상속할 때 필요한 내용을 서술한다.

```java
/**
 * @implSpec 해당 method는 Blah~blah~
 */
public void draw(String color) {
    for(Shape shape : shapes) {
        shape.draw(color);
    }
}
```

Clone과 readObject 모두 직/간접적으로 재정의 가능 method를 호출해서는 안된다.

검증방법 : 하위 Object를 만들어 테스트 해 보면 좋다.


### 올바르게 상속을 고려하려면 (잘못된 예제)
```java
public class Super {
    public Super() {
        overrideMe();
    }
    public void overrideMe() {}
}
```
Constructor는 직/간접적으로 Override 가능한 method를 call하면 안된다.

```java
public class Sub extends Super {
    private final Instant instant;
    public Sub() {
        instant = Instant.now();
    }
    @Override
    public void overrideMe() {
        System.out.println(instant);
    }
}
```

값은 null, 시간 값 출력

### 정리

왠만하면 Interface를 통한 구현을 추천한다.

경험상 주석을 열심히 달아 놓아도, 대다수의 개발자들은 문제가 터졌을 때나 그것을 확인할 확률이 높다.

Final class 보다는 coding rule을 정할 때 왠만하면 상속을 피한다. 라고 협의 후

해당 규칙을 코드 리뷰에 반영하는 편이 좀 더 현식적일 수 있다.

변수 몇개가 겹친다고 해서 꼭 상속을 통한 확장을 해야 하는 것은 아니다.

Next Generation이 우리가 생각한 대로 행동할 수 있다는 보장이 없다.
