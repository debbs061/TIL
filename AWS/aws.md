### 방법 1. 메시지 큐를 제공하는 SQS

* [API Gateway 에서 SQS 연동](https://www.icatpark.com/106)
* 같은 리전 안에서는 데이터 전송이 무료
* SQS 는 메시지를 손실 없이 정확하게 처리할 때 필요한 시스템이다.
  * 적용 사례 ) 아마존닷컴의 경우 대규모 서비스이기 때문에 상품을 보여주는 웹 서버, 상품 정보를 저장하는 데이터베이스, 결제를 처리하는 서버, 배송을 처리하는 서버 등 여러 부분으로 분리되어 있다. 사용자가 상품을 주문하고 결제한 뒤 웹 서버가 중단되더라도 결제 정보는 잃어버리지 않고 정확히 처리되어야 한다. 
  * [지그재그](https://devblog.croquis.com/ko/2017-05-13-1-aws-serverless-1/)
* 사용자에게 결과를 빨리 보여줘야 하는 작업과 시간이 오래 걸리는 작업을 분리할 때, 중요한 작업과 중요하지 않은 작업을 분리할 때 SQS 큐를 유용하게 사용한다.



### 방법 2. AWS monitoring 연동해서 API Gateway 호출건수랑 캐시 hit 인지, 캐시 miss 인지 다 볼 수 있음

* API 키를 사용하여 API 클라이언트를 식별 가능

* 설계

  * client api 호출 -> gateway는 해당 lambda 권한 부여자 호출 -> 이 권한 부여자는 호출자의 자격 증명을 입력으로 받아서 IAM 정책을 출력으로 반환

  * lambda 권한 부여자는 두가지 유형이 있는데 *토근 기반 lambda 권한 부여자* 이용 예정

  * 다음 다이어그램은 Lambda 권한 부여자 인증 워크플로우

    ![](https://docs.aws.amazon.com/ko_kr/apigateway/latest/developerguide/images/custom-auth-workflow.png)

3. Calling out to an OAuth provider to get an OAuth access token.
