# Checked Exception 과 Unchecked Exception



|     구분      |          Checked Exception          |           Unchecked Exception            |
| :-----------: | :---------------------------------: | :--------------------------------------: |
|   확인 시점   |        컴파일(Compile) 시점         |           런타임(Runtime) 시점           |
|   처리 여부   |     반드시 예외 처리해야 한다.      |       명시적으로 하지 않아도 된다.       |
| 트랜잭션 처리 |      예외 발생시 롤백하지 않음      |         예외 발생시 롤백해야 함          |
|     종류      | IOException, ClassNotFoundException | NullPointerException, ClassCastException |

