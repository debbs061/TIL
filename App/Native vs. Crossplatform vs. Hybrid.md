

## 네이티브 앱 vs 크로스 플랫폼 앱 vs 하이브리드 앱

#### Ios/android 앱을 만드려고 하면 이렇게 3가지 옵션이 있다.

## (1) Hybrid app

* 하이브리드 앱이란 웹 뷰를 감싸고 있는 웹사이트이다.

  * 웹 뷰 = 브라우저의 윈도우인데, 내비게이션이 없다.

    <img src="/Users/kong/Library/Application Support/typora-user-images/image-20210426114646136.png" alt="image-20210426114646136" style="zoom: 33%;" />

  

  * 그래서 하이브리드 앱을 만드려면  js,html,css 를 가지고 웹 뷰를 감싼 후에, 그걸 스토어로 보내는 개념이다.

* 하이브리드 앱을 만들고 싶다면, apache cordova 이나 phonegap 을 이용하자.

  * html, 웹뷰를 감싸는 기능을 제공

## (2) CrossPlatform app

* **네이티브 코드가 아닌 것으로 코딩을 한 후** , 나중에 ios/android 가 이해할 수 있는 코드로 변환이 된다.
  * 예를 들면, 리액트 네이티브. (ex. 인스타그램)
    * 일단 리액트로 코딩  ➜ 이는 자바스크립트 코드로 컴파일 되고 ➜ 이후 ios/android 가 이를 각각의 자바스크립트 엔진으로 실행시킴
  * flutter의 경우
    * Dart 코드로 코딩 ➜ 이는 C, C++ 로 컴파일 됨 ➜ ios/android 둘 다 이해할 수 있는 언임
* 장점: 하나의 코딩으로 ios/android 앱 둘다 개발 가능
* 단점: 퍼포먼스 이슈가 생길 수 있음. 

## (3) Native app

* 말 그대로 android/ios 코드를 써서 개발
  * ios 라면 swift를 배워야 하고 (xcode)
  * android 라면 java나 kotlin 을 배워야 한다. (android studio)
* 장점: 폰이 갖고있는 파워를 그대로 갖게 됨. 가능한 어떤 API든 쓸 수 있다. 내 코드와 휴대폰 사이의 중간단계가 없으니깐
* 단점: 2개 플랫폼의 언어를 배워야 한다.

----

#### 출처 

* https://www.youtube.com/watch?v=ksz_mSninEY&t=10s