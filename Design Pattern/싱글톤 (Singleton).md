# 싱글톤 <sub>Singleton</sub>

싱글톤 패턴은 단 하나의 인스턴스를 생성해 사용하는 디자인 패턴이다.

| Singleton |
| :--- |
| -instance |
| -Singleton() |
| +getInstance() |

* 하나의 인스턴스만을 생성
* getInstance 메서드를 통해 모든 클라이언트에게 동일한 인스턴스를 반환

## 코드

```java
public class SingleObject {
    private static SingleObject instance = new SingleObject();

    //생성자는 내부에서만 호출되게 한다.
    private SingleObject(){}

    //외부에서 객체를 받아갈 수 있는 방법은 getInstance()가 유일하다. 
    public static SingleObject getInstance() {
        return instance;
    }
}
```
```java
public class SingletonPatternDemo {
    public static void main(String[] args) {
        
        //항상 getter로 객체를 받아와야 함.
        SingleObject object = SingleObject.getInstance();
    }
}
```