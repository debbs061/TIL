## 목차

1. INDEX
2. Clustered Index 와 Non-clustered Index

---

## 1장. INDEX

### 인덱스를 만든다는 건 테이블과 매핑된 또 다른 테이블이 하나 생성된다는 것이다.

인덱스에서 데이터를 먼저 찾은 다음에 그 테이블로 매핑된 곳을 가서 나머지 데이터를 꺼내오는 방식이다.

* `where` 절에 자주 등장하는 컬럼을 인덱스로 설정해주면 효율적
* `order by` 
  * 이미 인덱스는 소팅이 되어 있기 때문에 별도로 order by 를 수행할 필요 없이 인덱스에 바로 꺼내서 출력하면 됨



---

## 2장. Clustered Index vs Non-clustered Index

### Clustered Index

* PK 값이라고 보면 된다. 보통 PK 로 지정한다.
* 회원테이블에서 email 을 PK 로 잡을 경우 (clustered Index로 잡을 경우) , 성능에 이슈가 있을 수 있다. 왜?
  * a 로 시작하는 email 주소가 새로 들어올 경우 기존의 row들이 전부 뒤로 밀려나야 함
  * email 에는 UK 를 주고 PK 는 별도 시퀀스로 가져가는게 유리

### Non-clustered Index

* 이것 역시 INSERT 시 추가 작업 필요 (인덱스 생성하는 작업)
* 추가 저장 공간 필요 (약 10%)
* 인덱스를 지정할 때 평가 기준 ➜ Cardinality 
  * 카디널리티가 높을수록 인덱스로 지정하는 것을 고려해 볼 필요가 있다.



---

### 출처

https://www.youtube.com/watch?v=uO8tL0okg7Q

