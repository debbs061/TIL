## 나는 왜 final 에 집착하는가?

* 자바 final 정의
  * The final keyword is used in several contexts to define an entity that can only be assigned once. - wikipedia
  * 한 번만 할당 가능하다는 선언
  * 즉, 재할당하려고 하면 컴파일 오류가 발생하여 바로 확인이 가능하다.
* final 을 사용하면 한 번에 한 가지만 집중할 수 있다. 
  * 값에 대한 검증 x  ➜ 로직 구현에 집중 가능



#### a 부터 b 까지의 합을 구하려 한다.

* 개발자의 실수로 b까지 더하지 못 한 코드이다.
  * 이런 실수는 왜 발생하는가? i 가 가변 객체이기 때문이다.

```java
public int addFromTo(final int a, final int b) {
    int sum = 0;
    for (int i = a; i < b; i++) {
        sum += i;
    }
    return sum;
}
```

* 가변 객체를 최대한 줄여 작성해보았다. ➜ 조금 더 객체지향적으로 작성이 가능하다.

```java
public int addFromTo(final int a, final int b) {
    return addFromOneTo(b) - addFromOneTo(a-1);
}

private int addFromOneTo(final int n) {
    return (n * (n+1)) / 2;
}
```





## 불변 객체란?

* 불변 객체의 정의
  * Immutable object is an object whose state cannot be modified after it is created - wikipedia
  * 한 번 생성되면 상태를 수정할 수 없는 객체 ➜ 생성이 된 불변 객체는 신뢰할 수 있다.
* 불변 객체 장점
  * 맞왜틀?
  * 스레드 동기화 문제 방지

---

출처 

https://www.youtube.com/watch?v=ej-bnXlHk-E