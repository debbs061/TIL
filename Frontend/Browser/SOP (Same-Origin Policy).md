# 1. SOP (Same-Origin Policy)

>  The **same-origin policy** is a critical security mechanism that restricts how a document or script loaded by one [origin](https://developer.mozilla.org/en-US/docs/Glossary/Origin) can interact with a resource from another origin.	*-mozilla-*

한 origin 으로부터 로드된 document 또는 script 가 **다른 origin 의 리소스**와 상호작용 할 수 있는 방법을 제한하는 중요한 보안 메커니즘이다. 브라우저가 document 를 받을 때, 이 document 의 출신이 어딘 지 꼬리표를 달아두는데 이것을 origin 이라 한다.



## 1-1. origin 

naver.com 에 들어가면 브라우저가 naver 서버로부터 html document를 다운로드 받고, 이 document 를 해석해서 브라우저 윈도우에 뿌려준다. 이때, 브라우저가 document 를 받을 때 이 document의 출신이 어딘지 꼬리표를 달아두는데 이걸 "origin" 이라 한다.

![image-20210629202329936](/Users/kong/Library/Mobile Documents/com~apple~CloudDocs/CS 정리본/Frontend/Browser/images/image-20210629202329936.png)



*  `document` : 브라우저가 해석한 document 객체 
* `document.location.origin` : document 의 출신인 origin 

document 내에서 외부리소스들과 상호작용 할 때, 리소스의 origin 이 document 의 origin 과 다른 경우에 뭔가 제한을 두겠다 라고 하는 것이 Same Origin Policy 이다.



## 1-2. 브라우저가 origin 을 판단하는 기준

* Scheme, Host, Port

![image-20210629134701578](/Users/kong/Library/Mobile Documents/com~apple~CloudDocs/CS 정리본/Frontend/Browser/images/CORS.png)



* 이 중에 same origin 을 갖는 url 은 어떤 것들일까?

![image-20210629134800153](/Users/kong/Library/Application Support/typora-user-images/image-20210629134800153.png)



* 첫 번째, 두 번째만 same-origin 을 갖는다.

![image-20210629134823525](/Users/kong/Library/Application Support/typora-user-images/image-20210629134823525.png)



## 1-3. SOP 가 적용되는 예

**document 가 다른 리소스와 상호작용을 할 때 SOP 가 적용된다고 했는데** , 어떤 상호작용을 할 때를 말하는 걸까?

![image-20210629203359470](/Users/kong/Library/Mobile Documents/com~apple~CloudDocs/CS 정리본/Frontend/Browser/images/image-20210629203359470.png)

* url 접속 시, 브라우저에서 document를 다운 받고 그 document 에는 위와 같은 내용들이 포함되어 있다.
* XMLHttpRequest 객체를 이용해서 api 서버로부터 json 데이터를 받아오려고 한다.
* 이때, sop 가 적용되어서 https://www.docydocy.com 과 JSON 데이터의 origin 인 https://api.dokydoky.com 을 비교한다.
* same-origin 이 아닐 경우에는 JSON 데이터를 읽을 수 없게 된다.



![image-20210629203802447](/Users/kong/Library/Mobile Documents/com~apple~CloudDocs/CS 정리본/Frontend/Browser/images/image-20210629203802447.png)

* window 객체를 이용해서 새로운 팝업창을 하나 띄우려고 한다.
* 이 새 window 에도 새로운 document 가 있을 것이다.
* 이 경우에도 sop가 적용되어 same-origin 이 아닐 경우에는 서로 간의 document 에 접근할 수 없고 극히 제한적인 객체에만 접근이 가능하다.



![image-20210629204045128](/Users/kong/Library/Mobile Documents/com~apple~CloudDocs/CS 정리본/Frontend/Browser/images/image-20210629204045128.png)

* iframe 의 경우에도 same-origin 이 아닌 경우에는 iframe 내부와 외부의 접근이 매우 제한적이다.

* **iframe이란** inline frame의 약자입니다. **iframe** 요소를 이용하면 해당 웹 페이지 안에 어떠한 제한 없이 또 다른 하나의 웹 페이지를 삽입할 수 있다.



![image-20210629204159300](/Users/kong/Library/Mobile Documents/com~apple~CloudDocs/CS 정리본/Frontend/Browser/images/image-20210629204159300.png)

* local Storage, session Storage 같은 웹 데이터베이스는 origin 마다 하나씩 생성이 된다.
* 이 생성된 데이터베이스에는 same-origin 을 갖는 도큐먼트나 스크립트만 접근이 가능하다. 



### 그럼 Cross-Origin 간에 데이터 교환이 필요할 때는? 

* `window.postMessage`  이 api 를 이용하자



## 1-4. SOP 가 없다면 무슨 일이 벌어질까?

### Case 1. iframe 을 이용하는 경우

* 해커가 일반인에게 링크를 던져주고 자신이 만든 서버로 접근하도록 유인한다.
* 유저가 해당 서버에 들어가면, 거기에 있는 document 가 유저의 브라우저에세 실행되게 된다.
* 이 document 가 하는 일을 살펴보자.
  * 우선 iframe 내에서 구글의 inbox 페이지를 로드해온다. 
  * 유저가 기존에 구글 메일에 로그인을 해 놓은 상태라면, 구글 메일 서버로 요청을 할 때 브라우저에 저장되어 있는 유저의 쿠키를 자동적으로 붙여서 요청을 한다.
  * 메일 서버는 유저의 이메일 내용을 return 해 주며, 이메일 내용은 iframe 내부의 document 에 들어가게 된다. 
  * `<script>`부분을 보자. iframe 에 load 가 됐을 때, iframe 의 document 를 읽어온다. 이 document 에는 유저의 메일정보가 들어 있다.
  * 이메일정보를 인코딩한 후에, 공격자의 서버로 전송한다.
    * [`fetch()` (en-US)](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch) 메서드를 이용하는 것으로 비동기 네트워크 통신을 알기쉽게 기술할 수 있습니다.

이렇게 구글 메일을 공격자에게 탈취당하는 일이 생겨버린다. 

![image-20210629204619110](/Users/kong/Library/Application Support/typora-user-images/image-20210629204619110.png)



#### SOP 가 있다면, 어떻게 방어가 가능한걸까?

* origin 이 달라도, inbox 페이지는 iframe 내부로 정상적으로 로드된다. 
* 하지만 상위 document 에서 ifreame 내부의 document 를 참조할 때, same-origin 이 아니므로 참조할 수가 없게 된다.
  * 참조하면 null 값이 return 됨.
* 즉, iframe 내부에 로드된 document 의 origin 이 다르다 보니깐, iframe 자체를 격리시켜 iframe 내부와 외부 사이에 참조할 수 없도록 한 것이다.

![image-20210629205937310](/Users/kong/Library/Mobile Documents/com~apple~CloudDocs/CS 정리본/Frontend/Browser/images/image-20210629205937310.png)



### Case 2. XMLHttpRequest 함수를 이용하는 경우

* 다른 서버에 자원을 요청할 때 ...

  * write 는 same-origin 이든 cross-origin 이든 일반적으로 가능함.

    * 이 요청이 preflight 요청일 때는 write 도 불가  

    * 이것 때문에 CSRF 공격이 가능한 것이다. (물론 브라우저에 사용자 정보를 담아두는 쿠키 때문이기도 하고)

      * 예를 들어서 구글 메일에 패스워드를 바꾸는 API 가 있다고 가정하자.  이 상황에 이 API 를 이용해서 패스워드를 바꾸는 요청을 해버린다면? ->  write 는 가능하다고 했으니까 요청은 할 수 있는 상황.. 그런데 메일 서버에서 CSRF 에 대한 아무런 대책이 없다면 유저의 패스워드를 그냥 바꿔버리게 된다.

        

![image-20210629211330610](/Users/kong/Library/Mobile Documents/com~apple~CloudDocs/CS 정리본/Frontend/Browser/images/image-20210629211330610.png)



### [정리] Cross-origin network access

* Cross-origin writes 는 일반적으로 가능 (preflight 제외)
* Cross-origin embedding 은 일반적으로 가능
  * < script src=''></script>
    * 스크립트 태그 이용해서 외부 CDN에 있는 스크립트를 다운로드 받기 가능
  * css: < link rel='stylesheet' href="...">
  * <img>, <video>, <audio>
  * < object>, <embed>
  * @font-face
  * < iframe>
* Cross-origin reads 는 일반적으로 불가능
  * 하지만, embedding 을 이용해 일부 정보 누출이 가능할 수도 있음
    * 페이스북 사진 중에, 로그인 한 상태에서 이 사진을 요청하면 300 픽셀 짜리 사진을 리턴 해주는데, 로그인 하지 않은 상태에서 사진을 요청하면 100 픽셀 짜리 사진을 리턴해준다고 가정하자. 그렇다면, < img> 를 이용해서 해당 사진을 embedding 할 경우 이미지의 크기를 보고서 이 사람이 지금 페이스북에 로그인 돼 있는 지 안 돼 있는 지를 판별 가능 → 이러한 정보 누출이 가능해짐.





## 1-5. Cross-origin 간의 통신이 필요한 경우는?

* CORS
  * 허용할 origin 만을 "Access-Control-Allow-Origin" 에 추가



---

## 출처

https://www.youtube.com/watch?v=6QV_JpabO7g















