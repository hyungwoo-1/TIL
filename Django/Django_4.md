# Django

---

## 목차

* The Django authentication system

* HTTP Cookies

* Authentication in Web requests

* Authentication with User

* Limiting access to logged-in users

---

## The Django authentication system

---

> ### 개요

* Django authentication system(인증 시스템)은 <mark>인증(Authenticaiton)</mark>과 <mark>권한(Authorization)</mark> 부여를 함께 제공(처리)하며, 이러한 기능을 일반적으로 인증 시스템이라고 함

* 필수 구성은 settings.py에 이미 포함되어 있으며 INSTALLED_APPS에서 확인 가능
  
  * django.contrib.auth

* **Authentication (인증)**
  
  * 신원 확인
  
  * 사용자가 자신이 누구인지 확인하는 것

* **Authorization (권한, 허가)**
  
  * 권한 부여
  
  * 인증된 사용자가 수행할 수 있는 작업을 결정

> ### 사전 설정

* 두번째 app acounts 생성 및 등록

![](Django_4_assets/2022-09-07-16-12-41-image.png)

* url 분리 및 매핑

![](Django_4_assets/2022-09-07-16-13-00-image.png)

![](Django_4_assets/2022-09-07-16-13-09-image.png)

---

## Substituting a custom User model

---

> ### 개요

* "Custom User Model로 대체하기"

* 기본 User Model을 필수적으로 Custom User model로 대체하는 이유 이해하기

* Django는 기본적인 인증 시스템과 여러 가지 필드가 포함된 User Model을 제공, 대부분의 개발 환경에서 기본 User Model을 Custom User Model로 대체함

* 개발자들이 작성하는 일부 프로젝트에서는 django에서 제공하는 built - in User model의 기본 인증 요구사항이 적절하지 않을 수 있음
  
  * 예를 들어, 내 서비스에서 회원가입 시 username 대신 email을 식별 값으로 사용하는 것이 더 적합한 사이트인 경우,
    
    Django의 User Model은 기본적으로 username를 식별 값으로 사용하기 때문에 적합하지 않음

* Django는 현재 프로젝트에서 사용할 User Model을 결정하는 AUTH_USER_MODEL 설정 값으로 Default User Model을 재정의(override)할 수 있도록 함

> ### AUTH_USER_MODEL

* 프로젝트에서 User를 나타낼 때 사용하는 모델

* 프로젝트가 진행되는 동안 (모델을 만들고 마이그레이션 한 후) 변경할 수 없음

* 프로젝트 시작 시 설정하기 위한 것이며, 참조하는 모델은 첫 번째 마이그레이션에서 사용할 수 있어야 함
  
  * 즉, 첫번째 마이그레이션 전에 확정 지어야 하는 값

* 다음과 같은 기본 값을 가지고 있음

![](Django_4_assets/2022-09-07-16-18-43-image.png)

> ### [참고] settings의 로드 구조

* AUTH_USER_MODEL은 settings.py에서 보이지 않는데 어디에 기본 값이 작성되어 있는 걸까?
  
  * 우리가 작성하는 settings.py는 사실 global_settings.py를 상속받아 재정의하는 파일임

---

## How to substituting a custom User model

---

> ### 개요

* "custom User model로 대체하기"

* 대체하는 과정을 외우기 어려울 경우 공식문서를 보며 순서대로 진행하는 것을 권장
  
  * https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#substituting-a-custom-user-model

> ### 대체하기

* AbstractUser를 상속받는 커스템 User 클래스 작성

* 기존 User 클래스도 AbstractUser를 상속받기 때문에 커스텀 User 클래스도 완전히 같은 모습을 가지게 됨

![](Django_4_assets/2022-09-07-16-31-03-image.png)

* Django 프로젝트에서 User를 나타내는데 사용하는 모델을 방금 생성한 커스템 User 모델로 지정

![](Django_4_assets/2022-09-07-16-33-13-image.png)

* admin.py에서 커스텀 User 모델을 등록
  
  * 기본 User 모델이 아니기 때문에 등록하지 않으면 admin site에 출력되지 않음

![](Django_4_assets/2022-09-07-16-34-04-image.png)

> ### [참고] User 모델 상속 관계

![](Django_4_assets/2022-09-08-01-04-13-image.png)

> ### [참고] AbstractUser

* "관리자 권한과 함께 완전한 기능을 가지고 있는 User model을 구현하는 추상 기본 클래스"

* Abstract base classes (추상 기본 클래스)
  
  * 몇 가지 공통 정보를 여러 다른 모델에 넣을 때 사용하는 클래스
  
  * 데이터베이스 테이블을 만드는 데 사용되지 않으며, 대신 다른 모델의 기본 클래스로 사용되는 경우 해당 필드가 하위 클래스의 필드에 추가 됨
  
  * https://docs.python.org/3/library/abc.html

> ### [주의] 프로젝트 중간에 AUTH_USER_MODEL 변경하기

* 모델 관계에 영향을 미치기 때문에 훨씬 더 어려운 작업이 필요
  
  * 예를 들면 변경사항이 자동으로 수행될 수 없기 때문에 DB 스키마를 직접 수정하고, 이전 사용자 테이블에서 데이터를 이동하고, 일부 마이그레이션을 수동으로 다시 적용해야 하는 등..

* 결론은 중간 변경은 권장하지 않음 (프로젝트 처음에 진행하기)

> ### 데이터베이스 초기화

* 수업 진행을 위한 데이터베이스 초기화 후 마이그레이션 (프로젝트 중간일 경우)
1. migrations 파일 삭제
   
   * migrations 폴더 및 `__init__.py`는 삭제하지 않음
   
   * 번호가 붙은 파일만 삭제

2. db.sqlite3 삭제

3. migrations 진행
   
   * makemigrations
   
   * migrate

> ### custom User로 변경된 테이블 확인

* 이제 auth_user 테이블이 아니라 accounts_user 테이블을 사용하게 됨

![](Django_4_assets/2022-09-08-01-11-57-image.png)

> ### 반드시 User 모델을 대체해야 할까?

* Django는 새 프로젝트를 시작하는 경우 비록 기본 User 모델이 충분 하더라도 커스텀 User 모델을 설정하는 것을 강력하게 권장(highly recommended)

* 커스텀 User 모델은 기본 User 모델과 동일하게 작동 하면서도 필요한 경우 나중에 맞춤 설정할 수 있기 때문
  
  * 단, User 모델 대체 작업은 프로젝트의 모든 migrations 혹은 첫 migrate를 실행하기 전에 이 작업을 마쳐야 함

---

## HTTP Cookies

---

> ### 개요

* 로그인과 로그아웃을 이해하기 전 반드시 알아야하는 HTTP Cookies에 대해 먼저 알아보기

---

## HTTP

---

> ### HTTP

* Hyper Text Transfer Protocol

* HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜(규칙, 규약)

* 웹(www)에서 이루어지는 모든 데이터 교환의 기초

* 클라이언트 - 서버 프로토콜이라고도 부름

> ### 요청과 응답

* 요청(requests)
  
  * 클라이언트(브라우저)에 의해 전송되는 메시지

* 응답(response)
  
  * 서버에서 응답으로 전송되는 메시지

> ### HTTP 특징

1. 비 연결 지향(connectionless)
   
   * 서버는 요청에 대한 응답을 보낸 후 연결을 끊음
     
     * 예를 들어 우리가 네이버 메인 페이지를 보고 있을 때 우리는 네이버 서버와 연결되어 있는 것이 아님
     
     * 네이버 서버는 우리에게 메인 페이지를 응답하고 연결을 끊은 것

2. 무상태(stateless)
   
   * 연결을 끊는 순간 클라이언트와 서버 간의 통신이 끝나며 상태 정보가 유지되지 않음
   
   * 클라이언트와 서버가 주고받는 메시지들은 서로 완전히 독립적

> ### 어떻게 로그인 상태를 유지할까?

* 그런데 우리가 로그인을 하고 웹 사이트를 사용할 때 페이지를 이동해도 로그인 "상태"가 유지됨

* 서버와 클라이언트 간 지속적인 상태 유지를 위해 "쿠키와 세션"이 존재

---

## 쿠키(Cookie)

---

> ### 개요

* HTTP 쿠키는 상태가 있는 세션을 만들도록 해 줌

> ### 개념

* 서버가 사용자의 웹 브라우저에 전송하는 작은 데이터 조각이다.

* 사용자가 웹사이트를 방문할 경우 해당 웹사이트의 서버를 통해 사용자의 컴퓨터에 설치되는 작은 기록 정보 파일
  
  1. 브라우저(클라이언트)는 쿠키를 로컬에 KEY-VALUE의 데이터 형식으로 저장
  
  2. 이렇게 쿠키를 저장해 놓았다가, <mark>동일한 서버에 재요청 시 저장된 쿠키를 함께 전송</mark>

* 쿠키는 두 요청이 동일한 브라우저에서 들어왔는지 아닌지를 판단할 때 주로 사용됨
  
  * 이를 이용해 사용자의 로그인 상태를 유지할 수 있음
  
  * 상태가 없는(stateless) HTTP 프로토콜에서 상태 정보를 기억 시켜 주기 때문

* 즉, 웹 페이지에 접속하면 웹 페이지를 응답한 서버로부터 쿠키를 받아 브라우저에 저장하고, 클라이언트가 같은 서버에 재요청 시마다 요청과 함께 저장해 두었던 쿠키도 함께 전송

> ### 쿠키 사용 예시

![](Django_4_assets/2022-09-08-01-24-04-image.png)

![](Django_4_assets/2022-09-08-01-24-11-image.png)

![](Django_4_assets/2022-09-08-01-24-18-image.png)

> ### 쿠키 사용 목적

1. **세션 관리**
   
   * **로그인, 아이디 자동완성, 공지 하루 안보기, 팝업 체크, 장바구니 등의 정보 관리**

2. 개인화
   
   * 사용자 선호, 테마 등의 설정

3. 트래킹
   
   * 사용자 행동을 기록 및 분석

> ### 쿠키를 이용한 장바구니 예시

* 장바구니에 상품 담기

![](Django_4_assets/2022-09-08-01-26-10-image.png)

* 개발자 도구 -Network 탭 - cartView.pang 확인

* 서버는 응답과 함께 Set-Cookie 응답 헤더를 브라우저에게 전송

* 이 헤더는 클라이언트에게 쿠키를 저장하라고 전달

![](Django_4_assets/2022-09-08-01-26-56-image.png)

* Cookie 데이터 자세히 확인

![](Django_4_assets/2022-09-08-01-27-13-image.png)

* 메인 페이지 이동 - 장바구니 유지 상태 확인

* 서버로 전송되는 모든 요청에 브라우저는 Cookie HTTP 헤더를 사용해 서버로 이전에 저장했던 모든 쿠키들을 함께 전송 (장바구니 정보를 매 요청마다 보내는 것)

![](Django_4_assets/2022-09-08-01-28-06-image.png)

* 개발자 도구 - Application 탭 - Cookies

* 마우스 우측 버튼 - Clear 후 새로고침

![](Django_4_assets/2022-09-08-01-28-39-image.png)

* 빈 장바구니로 변경된 것을 확인

![](Django_4_assets/2022-09-08-01-28-56-image.png)

> ### 세션 (Session)

* 사이트와 특정 브라우저 사이의 "state(상태)" 를 유지시키는 것

* 클라이언트가 서버에 접속하면 서버가 특정 session id를 발급하고, 클라이언트는 session id를 쿠키에 저장
  
  * 클라이언트가 다시 동일한 서버에 접속하면 요청과 함께 쿠키(session id가 저장된)를 서버에 전달
  
  * 쿠키는 요청 때마다 서버에 함께 전송 되므로 서버에서 session id를 확인해 알맞은 로직을 처리

* session id는 세션을 구별하기 위해 필요하며, 쿠키에는 session id만 저장

> ### 쿠키 LIfetime (수명)

1. Session Cookie
   
   * 현재 세션이 종료되면 삭제됨
   
   * 브라우저 종료와 함께 세션이 삭제됨

2. Persistent cookies
   
   * Expires 속성에 지정된 날짜 혹은 Max-Age 속성에 지정된 기간이 지나면 삭제됨

> ### Session in Django

* Django는 database-backed sessions 저장 방식을 기본 값으로 사용
  
  * session 정보는 Django DB의 django_session 테이블에 저장
  
  * 설정을 통해 다른 저장방식으로 변경 가능

* Django는 특정 session id를 포함하는 쿠키를 사용해서 각각의 브라우저와 사이트가 연결된 session을 알아냄

* Django는 우리가 session 메커니즘(복잡한 동작원리)에 대부분을 생각하지 않게끔 많은 도움을 줌

---

## Authentication in Web requests

---

> ### 개요

* Django가 제공하는 인증 관련 built-in forms 익히기

---

## Login

---

> ### 개요

* 로그인은 Session을 Create하는 과정

> ### AuthenticationForm

* 로그인을 위한 built-in form
  
  * 로그인 하고자 하는 사용자 정보를 입력 받음
  
  * 기본적으로 username과 password를 받아 데이터가 유효한지 검증

* request를 첫번째 인자로 취함

> ### 로그인 페이지 작성

![](Django_4_assets/2022-09-12-21-29-21-image.png)

![](Django_4_assets/2022-09-12-21-29-28-image.png)

![](Django_4_assets/2022-09-12-21-29-33-image.png)

> ### login()

* login(request, user, backend=None)

* 인증된 사용자를 로그인 시키는 로직으로 view 함수에서 사용됨

* 현재 세션에 연결하려는 인증 된 사용자가 있는 경우 사용

* HttpRequest 객체와 User 객체가 필요

> ### 로그인 로직 작성

* 로그인 페이지 작성

* view 함수 login과의 충돌을 방지하기 위해 import한 login 함수 이름을 auth-login으로 변경해서 사용

![](Django_4_assets/2022-09-12-21-31-28-image.png)

> ### get_user()

* AuthenticationForm의 인스턴스 메서드

* 유효성 검사를 통과했을 경우 로그인 한 사용자 객체를 반환

> ### 세션 데이터 확인하기

* 로그인 후 개발자 도구와 DB에서 django로부터 발급받은 세션 확인
  
  (로그인은 관리자 계정을 만든 후 진행)
1. django_session 테이블에서 확인

![](Django_4_assets/2022-09-12-21-44-14-image.png)

2. 브라우저에서 확인
   
   개발자도구 - Application - Cookies

![](Django_4_assets/2022-09-12-21-44-44-image.png)

> ### 로그인 링크 작성

* 실습 편의를 위해 base 템플릿에 로그인 페이지로 이동할 수 있는 하이퍼 링크 작성

![](Django_4_assets/2022-09-12-21-45-28-image.png)

---

## Authentication with User

---

> ### 개요

* 템플릿에서 인증 관련 데이터를 출력하는 방법

* User Object와 User CRUD에 대한 이해
  
  * 회원 가입, 회원 탈퇴, 회원정보 수정, 비밀번호 변경

> ### 현재 로그인 되어있는 유저 정보 출력하기

* 템플릿에서 인증 관련 데이터를 출력하는 방법

![](Django_4_assets/2022-09-12-21-48-36-image.png)

* 어떻게 base 템플릿에서 context 데이터 없이 user 변수를 사용할 수 있는 걸까?
  
  => settings.py의 context processors 설정 값 때문

![](Django_4_assets/2022-09-12-21-49-27-image.png)

> ### context processors

* 템플릿이 렌더링 될 때 호출 가능한 컨테스트 데이터 목록

* 작성된 컨텍스트 데이터는 기본적으로 템플릿에서 사용 가능한 변수로 포함됨

* 즉, django에서 자주 사용하는 데이터 목록을 미리 템플릿에 로드 해 둔 것

![](Django_4_assets/2022-09-12-21-54-49-image.png)

* 현재 user 변수를 담당하는 프로세서는 django.contrib.auth.context_processors.auth

* 이외에 더 많은 Built-in template context processors들은 공식문서를 참고

![](Django_4_assets/2022-09-12-21-56-20-image.png)

> ### django.contrib.auth.context_processors.auth

* 현재 로그인한 사용자를 나타내는 User 클래스의 인스턴스가 템플릿 변수 
  
  `{{ user }}` 에 저장됨
  
  * 클라이언트가 로그인하지 않은 경우 AnonymousUser 클래스의 인스턴스로 생성

![](Django_4_assets/2022-09-12-21-57-56-image.png)

![](Django_4_assets/2022-09-12-21-58-06-image.png)

---

## Logout

---

> ### 개요

* 로그아웃은 Session을 Delete하는 과정

> ### logout()

* logout(request)

* HttpRequest 객체를 인자로 받고 반환 값이 없음

* 사용자가 로그인하지 않은 경우 오류를 발생시키지 않음

* 다음 2가지 일을 처리한다.
  
  1. 현재 요청에 대한 session data를 DB에서 삭제
  
  2. 클라이언트 쿠키에서도 sessionid를 삭제
     
     * 이는 다른 사람이 동일한 웹 브라우저를 사용하여 로그인하고, 이전 사용자의 세션 데이터에 액세스하는 것을 방지하기 위함

> ### 로그아웃 로직 작성하기

![](Django_4_assets/2022-09-12-22-01-18-image.png)

![](Django_4_assets/2022-09-12-22-01-26-image.png)

![](Django_4_assets/2022-09-12-22-01-30-image.png)

> ### 로그아웃 출력 확인 및 테스트

![](Django_4_assets/2022-09-12-22-02-09-image.png)

---

## 회원 가입

---

> ### 개요

* 회원가입은 User를 Create하는 것이며 UserCreationForm built-in form을 사용

> ### UserCreationForm

* 주어진 username과 password로 권한이 없는 새 user를 생성하는 ModelForm

* 3개의 필드를 가짐
  
  1. username (from the user model)
  
  2. password 1
  
  3. password 2

> ### 회원 가입 페이지 작성

![](Django_4_assets/2022-09-12-22-05-32-image.png)

![](Django_4_assets/2022-09-12-22-05-56-image.png)

![](Django_4_assets/2022-09-12-22-06-03-image.png)

> ### 회원가입 링크 작성 후 페이지 확인

![](Django_4_assets/2022-09-12-22-06-23-image.png)

> ### 회원가입 로직 작성

![](Django_4_assets/2022-09-12-22-06-37-image.png)

> ### 회원가입 진행 후 에러 페이지를 확인

* 회원가입에 사용하는 UserCreationForm이 우리가 대체한 커스텀 유저 모델이 아닌 기존 유저 모델로 인해 작성된 클래스이기 때문

![](Django_4_assets/2022-09-12-22-07-27-image.png)

![](Django_4_assets/2022-09-12-22-07-32-image.png)

---

## Custom user & Built-in auth forms

---

> ### 개요

* Custom user와 기존 Built-in auth forms 간의 관계

* Custom user로 인한 Built-in auth forms 변경

> ### AbstractBaseUser의 모든 subclass와 호환되는 forms

* 아래 Form 클래스는 User 모델을 대체하더라도 커스텀 하지 않아도 사용가능
  
  1. AuthenticationForm
  
  2. SetPasswordForm
  
  3. PasswordChangeForm
  
  4. AdminPasswordChangeForm

* 기존 User 모델을 참조하는 Form이 아니기 때문

> ### 커스텀 유저 모델을 사용하려면 다시 작성하거나 확장해야 하는 forms

1. UserCreationForm

2. UserChangeForm
* 두 form 모두 class Meta: model = User 가 등록된 form이기 때문에 반드시 커스텀(확장)해야 함

> ### UserCreationForm() 커스텀 하기

![](Django_4_assets/2022-09-12-22-15-26-image.png)

> ### get_user_model()

* "현재 프로젝트에서 활성화된 사용자 모델(active user model)"을 반환

* 직접 참조하지 않는 이유
  
  * 예를 들어 기존 User 모델이 아닌 User 모델을 커스텀 한 상황에서는
    
    커스텀 User 모델을 자동으로 반환해주기 때문

* Django는 User 클래스를 직접 참조하는 대신 get_user_model()을 사용해 참조해야 한다고 강조하고 있음

* User  model 참조에 대한 자세한 내용은 추후 모델 관계 수업에서 다룰 예정

> ### CustomUserCreationForm() 으로 대체하기

![](Django_4_assets/2022-09-12-22-20-02-image.png)

> ### 회원가입 진행 후 테이블 확인

![](Django_4_assets/2022-09-12-22-20-29-image.png)

> ### 회원가입 후 곧바로 로그인 진행

![](Django_4_assets/2022-09-12-22-20-57-image.png)

> ### [참고] UserCreationForm의 save 메서드

* user를 반환하는 것을 확인

![](Django_4_assets/2022-09-12-22-25-49-image.png)

---

## 회원 탈퇴

---

> ### 개요

* 회원 탈퇴하는 것은 DB에서 유저를 Delete하는 것과 같음

> ### 회원 탈퇴 로직 작성

![](Django_4_assets/2022-09-12-22-26-34-image.png)

![](Django_4_assets/2022-09-12-22-26-40-image.png)

![](Django_4_assets/2022-09-12-22-26-46-image.png)

> ### [참고] 탈퇴 하면서 해당 유저의 세션 정보도 함께 지우고 싶을 경우

* "탈퇴(1) 후 로그아웃(2)"의 순서가 바뀌면 안됨
  
  * 먼저 로그아웃 해버리면 해당 요청 객체 정보가 없어지기 때문에 탈퇴에 필요한 정보 또한 없어지기 때문

![](Django_4_assets/2022-09-12-22-27-43-image.png)

---

## 회원정보 수정

---

> ### 개요

* 회원정보 수정은 User를 Update 하는 것이며 UserChangeForm built-in form을 사용

> ### UserChangeForm

* 사용자의 정보 및 권한을 변경하기 위해 admin 인터페이스에서 사용되는 ModelForm

* UserChangeForm 또한 ModelForm이기 때문에 instance 인자로 기존 user 데이터 정보를 받는 구조 또한 동일함

* 이미 이전에 CustomUserChangeForm으로 확장했기 때문에 CustomUserChangeForm을 사용하기 

> ### 회원정보 수정 페이지 작성

![](Django_4_assets/2022-09-12-22-30-50-image.png)

![](Django_4_assets/2022-09-12-22-30-59-image.png)

![](Django_4_assets/2022-09-12-22-31-06-image.png)

> ### 회원정보 수정 페이지 링크 작성

![](Django_4_assets/2022-09-12-22-31-22-image.png)

> ### 회원정보 수정 페이지 확인

![](Django_4_assets/2022-09-12-22-31-37-image.png)

> ### UserChangeForm 사용 시 문제점

* 일반 사용자가 접근해서는 안 될 정보들(fields)까지 모두 수정이 가능해짐
  
  * admin 인터페이스에서 사용되는 ModelForm이기 때문

* 따라서 UserChangeForm을 상속받아 작성해 두었던 서브 클래스
  
  CustomUserChangeForm에서 접근 가능한 필드를 조정해야 함

> ### CustomUserChangeForm fields 재정의

* User 모델의 fields명은 어떻게 알 수 있을까?

![](Django_4_assets/2022-09-12-22-33-39-image.png)

> ### User model 상속 구조 살펴보기

![](Django_4_assets/2022-09-12-22-34-26-image.png)

> ### CustomUserChangeForm fields 재정의

* 수정하고자 하는 필드 작성 후 출력 변화 확인

![](Django_4_assets/2022-09-12-22-34-54-image.png)

![](Django_4_assets/2022-09-12-22-35-01-image.png)

> ### 회원정보 수정 로직 작성

* 작성 후 실제 회원정보가 수정되었는지 확인

![](Django_4_assets/2022-09-12-22-35-29-image.png)

---

## 비밀번호 변경

---

> ### PasswordChangeForm

* 사용자가 비밀번호를 변경할 수 있도록 하는 Form

* 이전 비밀번호를 입력하여 비밀번호를 변경할 수 있도록 함

* 이전 비밀번호를 입력하지 않고 비밀번호를 설정할 수 있는 SetPasswordForm을 상속받는 서브 클래스

* 회원정보 수정 페이지에서 비밀번호 변경 form 주소를 확인해보기
  
  * /accounts/password/

![](Django_4_assets/2022-09-12-22-37-55-image.png)

> ### 비밀번호 변경 페이지 작성

![](Django_4_assets/2022-09-12-22-40-34-image.png)

![](Django_4_assets/2022-09-12-22-40-48-image.png)

![](Django_4_assets/2022-09-12-22-40-58-image.png)

> ### [참고] SetPasswordForm 살펴보기

* PasswordChangeForm은 SetPasswordForm의 하위 클래스이기 때문에 SetPasswordForm을 확인

![](Django_4_assets/2022-09-12-22-41-46-image.png)

> ### 비밀번호 변경 로직 작성

* 작성 후 비밀번호 변경 확인
  
  * 변경 후 로그인 상태가 지속되지 못하는 문제 발생

![](Django_4_assets/2022-09-12-22-42-22-image.png)

> ### 암호 변경 시 세션 무효화 방지하기

* 비밀번호가 변경되면 기존 세션과의 회원 인증 정보가 일치하지 않게 되어 버려 로그인 상태가 유지되지 못함

* 비밀번호는 잘 변경되었으나 비밀번호가 변경 되면서 기존 세션과의 회원 인증 정보가 일치하지 않기 때문

> ### update_session_auth_hash()

* update_session_auth_hash(request, user)

* 현재 요청(current request)과 새 session data가 파생 될 업데이트 된 사용자 객체를 가져오고, session data를 적절하게 업데이트해줌

* 암호가 변경되어도 로그아웃 되지 않도록 새로운 password의 session data로 session을 업데이트

> ### update_session_auth_hash()작성

![](Django_4_assets/2022-09-12-22-57-23-image.png)

---

## Limiting access to logged-in users

---

> ### 개요

* 로그인 사용자에 대한 접근 제한하기

* 로그인 사용자에 대해 접근을 제한하는 2가지 방법
1. The raw way
   
   * is_authenticated attribute
   
   * The login_required decorator

> ### is_authenticated

* User model의 속성 중 하나

* 사용자가 인증 되었는지 여부를 알 수 있는 방법

* 모든 User 인스턴스에 대해 항상 True인 읽기 전용 속성
  
  * AnonymousUser에 대해서는 항상 False

* 일반적으로 request.user에서 이 속성을 사용 (request.user.is_authenticated)

* 권한과는 관련이 없으며, 사용자가 활성화 상태이거나 유효산 세션을 가지고 있는지도 확인하지 않음

> ### [참고] is_authenticated 코드 살펴보기

![](Django_4_assets/2022-09-12-23-29-52-image.png)

> ### is_authenticated 적용하기

* 로그인과 비로그인 상태에서 출력되는 링크를 다르게 설정하기

![](Django_4_assets/2022-09-12-23-30-58-image.png)

* 인증된 사용자만 게시글 작성 링크를 볼 수 있도록 처리하기

* 하지만 아직 비 로그인 상태로도 URL을 직접 입력하면 게시글 작성 페이지로 갈 수 있음

![](Django_4_assets/2022-09-12-23-32-17-image.png)

* 인증된 사용자라면 로그인 로직을 수행할 수 없도록 처리

![](Django_4_assets/2022-09-12-23-32-39-image.png)

> ### login_required

* login_required decorator

* 사용자가 로그인 되어 있으면 정상적으로 view 함수를 실행

* 로그인 하지 않은 사용자의 경우 settings.py의 LOGIN_URL 문자열 주소로 redirect
  
  * [참고] LOGIN_URL의 기본 값은 / accounts/login/
  
  * 두번째 app 이름을 accounts 로 했던 이유 중 하나

* 로그인 상태에서만 글을 작성/수정/삭제 할 수 있도록 변경

![](Django_4_assets/2022-09-12-23-39-42-image.png)

> ### login_required 적용 확인하기

* /articles/create/ 로 강제 접속 시도 해보기

* 로그인 페이지로 리다이렉트 후 /accounts/login/?next=/articles/create/
  
  url확인하기

* 인증 성공 시 사용자가 redirect 되어야하는 경로는 "next"라는 쿼리 문자열 매개 변수에 저장됨
  
  * 예시 ) /accounts/login/?next=/articles/create/

> ### "next" query string parameter

* 로그인이 정상적으로 진행되면 이전에 요청했던 주소로 redirect 하기 위해 Django가 제공해주는 쿼리 스트링 파라미터

* 해당 값을 처리할지 말지는 자유이며 별도로 처리 해주지 않으면 view에 설정한 redirect 경로로 이동하게 됨

> ### "next" query string parameter 대응

![](Django_4_assets/2022-09-12-23-43-08-image.png)

> ### "next" query string parameter 주의사항

* 만약 login 템플릿에서 form action이 작성되어 있다면 동작하지 않음

* 해당 action 주소 next 파라미터가 작성 되어있는 현재 url이 아닌 /accounts/login/으로 요청을 보내기 때문

![](Django_4_assets/2022-09-12-23-44-45-image.png)

> ### 두 데코레이터로 인해 발생하는 구조적 문제

1. 먼저 비로그인 상태로 detail 페이지에서 게시글 삭제 시도

2. delete view 함수의 @login_required로 인해 로그인 페이지로 리다이렉트
   
   * http://127.0.0.1:8000/accounts/login/?next=/articles/1/delete/

3. redirect로 이동한 로그인 페이지에서 로그인 진행

4. delete view 함수의 @require_POST로 인해 405 상태 코드를 받게 됨
   
   * 405(Method Not Allowed) status code 확인
* 로그인 성공 이후 GET method로 next 파라미터 주소에 리다이렉트 되기 때문

![](Django_4_assets/2022-09-12-23-47-28-image.png)

![](Django_4_assets/2022-09-12-23-47-34-image.png)

* 두 가지 문제가 발생한 것
  
  1. redirect 과정에서 POST 요청 데이터의 손실
  
  2. redirect로 인한 요청은 GET 요청 메서드로만 요청됨

* 해결방안
  
  * `@login_required`는 GET request method를 처리할 수 있는 View 함수  에서만 사용해야함

> ### 두 데코레이터로 인해 발생하는 구조적 문제 해결

* POST method만 허용하는 delete 같은 함수는 내부에서는 is_authenticated 속성 값을 사용해서 처리

![](Django_4_assets/2022-09-12-23-49-55-image.png)

> ### accounts view 함수에 모든 데코레이터 및 속성 값 적용해보기

![](Django_4_assets/2022-09-12-23-51-49-image.png)

---

## 마무리

---

> ### 마무리

* The Django authentication system
  
  * User 모델 대체하기

* HTTP Cookies
  
  * 상태가 있는 세션 구성

* Authentication in Web requests
  
  * Auth built-in form 사용하기

* Authentication with User
  
  * User Object와 User CRUD
