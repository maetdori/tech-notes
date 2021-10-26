# 싱글톤 <sub>Singleton</sub>

GoF 디자인 패턴 중 **생성** 패턴에 해당한다. 어떤 클래스의 인스턴스는 하나임을 보장하는 패턴이다. 또한 그에 대한 전역적인 접근점을 제공한다. 주로 공통된 객체를 여러 곳에서 참조해야 하는 경우(ex. 데이터베이스 참조)에 사용한다. 

## 사용

* 클래스의 인스턴스가 하나임을 보장해야 할 때

## 구조

| Singleton |
| :--- |
| - instance |
| - Singleton() |
| + getInstance() |

## 구현

`Singleton` 클래스의 객체는 `getInstance()`를 통해 생성, 접근한다. 생성자의 접근제한자를 `private`으로 설정하고, 생성자에 `count++` 오퍼레이션을 추가해 인스턴스의 생성 횟수를 확인한다.

```java
public class Singleton {
    private static Singleton instance = new Singleton();
    private String name;
    private int count = 0;

    //생성자는 내부에서만 호출되게 한다.
    private Singleton(){
        count++;
        name = count + " 번째 레퍼런스";
    }

    //외부에서 객체를 받아갈 수 있는 방법은 getInstance()가 유일하다. 
    public static Singleton getInstance() {
        if(instance == null)
            instance = new Singleton();
        return instance;
    }
    
    public String getName() {
        return name;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Singleton object1 = Singleton.getInstance();
        System.out.println("First Instance Name: " + object1.getName());

        Singleton object2 = Singleton.getInstance();
        System.out.println("Second Instance Name: " + object2.getName());
    }
}
```

```java
[실행 결과]
First Instance Name: 1 번째 레퍼런스
Second Instance Name: 1 번째 레퍼런스
```

## 장점

* 유일하게 존재하는 인스턴스로의 접근을 통제
  * 인스턴스를 캡슐화하므로, 사용자의 접근을 제어할 수 있음
* 전역변수보다 좋음
* 인스턴스 개수를 변경하기 쉬움
  * 만약 인스턴스가 여러 개 존재하도록 하고 싶다면, 여러 개의 인스턴스를 생성해서 각각의 인스턴스로 접근할 수 있도록 구현하면 된다. 

<br/>
<br/>

## Reference
* https://4z7l.github.io/2020/12/29/design_pattern_singleton.html

