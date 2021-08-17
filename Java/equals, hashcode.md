## equals 는 일반 규약을 지켜 재정의하라

equals 메서드는 동치관계(equivalenca relation)를 구현하며, 다음을 만족한다.

1. 반사성  *reflexive* : for any non-null reference value `x`, `x.equals(x)` should return `true`.

2. 대칭성 *symmetric*: for any non-null reference values `x` and `y`, `x.equals(y)` should return `true` if and only if `y.equals(x)` returns `true`.
   * **대칭성**은 두 객체는 서로에 대한 동치 여부에 똑같이 답해야 한다는 뜻이다. 

#### 예) 대소문자를 구별하지 않고 문자열이 같은 지 여부를 반환하도록 equals 메소드를 재정의 해보자.

```java
public final class CaseInsensitiveString {
    private final String s;

    public CaseInsensitiveString(String s) {
        this.s = s;
    }

  	// 대칭성 위배 ! 
    @Override
    public boolean equals(Object obj) {
        if (obj instanceof CaseInsensitiveString)
            return s.equalsIgnoreCase(
                    ((CaseInsensitiveString) obj).s
            );
        if (obj instanceof String)
            return s.equalsIgnoreCase((String) obj);
        return false;
    }
}
```



대칭성을 위배하는 모습을 볼 수 있다.

```java
CaseInsensitiveString cis = new CaseInsensitiveString("Polish");
String s = "polish";
System.out.println(cis.equals(s));	// true
System.out.println(s.equals(cis));	// false
```



이번에는 `CaseInsensitiveString` 을 컬렉션에 넣어보자. 

➜ 즉, 메서드를 잘못 재정의 하면 대상 클래스가 규약을 준수한다고 가정하는 클래스 `ArrayList`, `HashMap`, `HashSet` 등을 오작동하게 만들 수 있다.

```java
List<CaseInsensitiveString> list = new ArrayList<>();
list.add(cis);
System.out.println(list.contains(s));	// false
```



## 공부하다 말음. p.51 언젠가 마무리하기

---

* 이펙티브 자바 p.52
* java document

