# new String 과 "" 의 차이

* "" (리터럴로 문자열을 생성했을 경우)
  * 같은 내용의 문자열들은 모두 하나의 String 인스턴스를 참조한다.
  * 어차피 String 인스턴스가 저장하고 있는 문자열은 **변경할 수 없기 때문에** 아무런 문제가 없다.
  * String 리터럴들은 컴파일 시에 클래스파일에 저장된다.
    * 모든 클래스 파일(*.class) 에는 `constant pool` 이라는 상수 목록이 있어서, 여기에 클래스 내에서 사용되는 모든 리터럴과 상수들이 저장되어 있다.
    * 메모리 측면에서 String 보다 효율적이다.

* new String
  * new 연산자에 의해서 메모리 할당이 이루어지기 때문에 항상 새로운 String 인스턴스가 생성된다. 

```
String strVal1 = new String("홍길동");
String strVal2 = "홍길동";
String strVal3 = "홍길동";
```



![](/Users/kong/Library/Mobile Documents/com~apple~CloudDocs/CS 정리본/CS 정리본/Java/StringEquals.jpeg)





----

참조

남궁성, **『자바의 정석** 2nd Edition』, 도우출판(2010), p376

