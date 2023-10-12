<img width="854" alt="Untitled 2" src="https://github.com/limjoohyun2030/CS-study/assets/39722436/502c2f33-310d-47ac-a180-7da6c6acd6da"># XSS(Cross-Site Scripting)

## XSS란?

웹페이지에 악성 스크립트를 삽입해서 공격하는 기법

Clinet 의 개인정보 및 쿠키를 탈취해서 세션하이재킹 공격을 하거나, 좀비 PC 로 만드는 공격도 발생할 수 있습니다.

## XSS 의 공격방식

- Stored XSS
- Reflected XSS
- DOM based XSS

### Stored XSS (저장형 XSS)

1. 공격자가 악의적인 스크립트가 담긴 게시물을 등록
    
    ![Untitled](https://github.com/limjoohyun2030/CS-study/assets/39722436/19fa20c3-208f-4b9b-bf9c-8587e4e821ce)

2. DB string 에 해당 스크립트가 저장됨
3. 사용자가 해당 게시글을 읽음
4. 악성 스크립트가 실행됨

```jsx
게시글 내용
안녕은 스페인어로<script>alert('hi')</script>hola
```

### Reflected XSS (반사형 XSS)

![Untitled 1](https://github.com/limjoohyun2030/CS-study/assets/39722436/f0acb4d4-7a4b-4292-9a3d-8536b7418e56)


공격자가 URL 에 있는 Query String  에 악성 스크립트를 담아 전송을 하고,

서버가 필터링을 거치지 않고 쿼리가 포함된 스크립트를 응답페이지에 담아 전송했을때 발생

공격용 스크립트가 꼭 웹페이지에 있을 필요는 없고, 타 사이트 링크나, 이메일을 통해서 가능합니다.

Stored XSS 는 DB 에 스크립트가 저장되지는 않고, 응답 페이지를 바로 Clinet 에 전달한다.

### DOM-based XSS

- DOM : 문서 객체 모델. 웹페이지 내에 있는 모든 객체들을 조작, 관리할 수 있는 모델
- 클라이언트의 브라우저에서 DOM 환경을 수정하여, 공격 페이로드가 실행되는 방식

DOM 객체 생성 시 클라이언트 측 스크립트에 포함되는 것입니다.

Stored와 Reflected 가 서버측 어플리케이션 취약점으로 인해 공격하는 반면, DOM Based XSS 는 서버와 관계없이 브라우저에서 발생하는 것이 차이점이다

<img width="854" alt="Untitled 2" src="https://github.com/limjoohyun2030/CS-study/assets/39722436/31064103-997a-4b0e-9b1f-b6c7f82245c4">

![Untitled 3](https://github.com/limjoohyun2030/CS-study/assets/39722436/b6d73bf9-4035-4da3-aed0-f4e7e50bab5c)



### XSS 공격 사례

- 네이버 웹툰 댓글 테러 사건 (2011)

웹툰 댓글에 html을 태그할 수 있는 기능이 추가됨. 

이를 이용해 웹툰에서 각종 XSS 공격을 시행되었는데, 주로 image 태그를 통한 댓글 창 도배가 주를 이루었음.

이 후 네이버 웹툰은 html 태그 기능을 댓글 창에서 제외함

## XSS 공격 방지 방법

**1. XSS 취약점이 있는 innerHTML 대신 textContext, innerText 사용**

HTML5에서는 innerHTML 을 통해 주입한 스크립트는 실행되지 않습니다.

```jsx
<script>alert('hello');</script>
```

그러나 onerror 이벤트 속성을 통한 스크립트 주입은 여전히 가능합니다.

```jsx
<img src=x onerror=alert('xss attack')>
```

innerHTML 대신 textContext, innerText를 사용하면 스크립트가 주입 되지 않습니다.

**2. 쿠키에 HttpOnly 옵션을 활성화**

- 활성화 하지 않으면 스크립트를 통해 쿠키에 접근할 수 있어 Session Hijacking 취약점이 발생합니다.
    
    ### 세션 하이재킹 예시
    
    훔친 세션을 이용해 본인이 이용자인 척 사용하는 것을 세션하이재킹 이라고 합니다. 공격에 성공하면 해커는 이용자와 동일한 권한을 가지고 계정을 사용할 수 있게 됩니다.
    
    ```jsx
    2017년 국내의 숙박 예약 앱 ‘여기어때’에서 세션 하이재킹 등 사이버 공격으로 인한 
    개인정보 유출사고가 일어났다. 이에 여기어때가 응하지 않자 5천 명의 고객에게 
    ‘좋은 밤 보내셨냐’는 내용의 문자를 보내 개인정보 유출 사실을 공개했다.
    ```
    
    개인 사용자가 세션 하이재킹을 예방하는 방법은 의외로 간단한데, 웹페이지 사용 후, ‘로그아웃’ 을 하는겁니다.
    
    웹사이트 입장에서는 **쿠키에 HttpOnly 옵션을 활성화** 하는 방법을 통해 예방 가능합니다.
    
- httpOnly 속성은 클라이언트(브라우저 등) 에서 설정할 수 없는 옵션이며, 서버 단에서 설정할 수 있습니다.
- httpOnly는 쿠키를 사용할 경우에만 해당되므로 LocalStorage를 사용하는 경우에는 세션ID와 같은 민감한 정보를 저장하지 않도록 합니다.

**3. **XSS 특수문자 치환**
    
    입력문자열에서 HTML 코드로 인식되는 특수문자들을 일반 문자열로 치환합니다.
    
    주로 라이브러리를 이용해서 치환
    
    ```jsx
    & → &amp;
    < -> &lt;
    > -> &gt;
    * -> &quot;
    ' -> &#x27;
    / -> &#x2F;
    ```
    

참고링크

[https://www.youtube.com/watch?v=bSGqBoZd8WM](https://www.youtube.com/watch?v=bSGqBoZd8WM)

[https://velog.io/@kylexid/XSS-Cross-Site-Scripting](https://velog.io/@kylexid/XSS-Cross-Site-Scripting)

[https://www.boannews.com/media/view.asp?idx=114265](https://www.boannews.com/media/view.asp?idx=114265)

[https://javairus.tistory.com/7](https://javairus.tistory.com/7)

[https://maker5587.tistory.com/57](https://maker5587.tistory.com/57)
