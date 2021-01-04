# Django-Girls-Tutorial
Django Girls Tutorial을 따라하면서 공부한 내용을 정리합니다.

## 인터넷은 어떻게 작동할까요?
* 웹페이지란 동영상, 음악, 사진같은 파일 + HTML로 구성된 문서
  * 웹사이트는 웹페이지들의 의미있는 묶음
  * 웹페이지는 웹서버에 저장됨
  * 즉, 내가 보고있는 모든 웹페이지들이 어딘가에 있는 웹서버에 저장되어있음
  * 그렇다면 어딘가에 있는 웹서버에 어떻게 접근할까?
  
* 인터넷은 수 많은 기계(서버)들이 연결된 하나의 네트워크
  * 특정한 기계에 도달하기 위해서는 수 많은 다른 기계들을 통과해야함
  * 즉, 수 많은 서버들을 통과해서 내가 원하는 웹페이지가 저장된 서버에 접근함

* 주소창에 https://djangogirls.org을 입력하는 행위는
  * '장고걸즈 웹사이트를 보여주세요!'라고 편지를 보내고 답장을 기다리는 것과 같음
  * 이 편지는 여러 곳의 우체국을 거쳐서 최종 목적지에 도착하게 됨
    * 편지 = 데이터 패킷
    * 우체국 = 라우터
    * 편지를 받는 사람의 주소 = IP주소
    * 편지를 보내는 방법 = HTTP 프로토콜

* 즉, 주소창에 http://djangogirls.org를 입력하면
  1. 컴퓨터는 DNS에 djangogirls.org의 IP주소를 알아냄
  2. 해당 IP주소로 데이터 패킷을 전송
  3. 이 데이터 패킷은 수많은 라우터를 거쳐서 djangogirls.org가 저장된 웹서버인 최종 목적지에 도달
  4. 웹서버에서는 저장된 djangogirls.org를 전송

* 장고는 4번 과정에서 모든 사람이 같은 내용을 받는 것이 아니라, 받는 사람마다 각각 다른 내용을 받도록하는 역할!
 
## 장고란 무엇인가요?
* 장고는 파이썬으로 만들어진 무료 오픈소스 웹 어플리케이션 프레임워크
* 쉽고 빠르게 웹사이트를 개발할 수 있도록 돕는 구성요소로 이루어진 웹 프레임워크
* 웹사이트를 구축할때는 비슷한 유형의 요소들이 항상 필요함 ex) 회원가입, 로그인, 로그아웃과 같은 사용자 인증
* 이러한 요소들을 바로 구성할 수 있게 만들어두었다
* 웹 서버가 요청에 맞는 응답을 보낼때 장고가 응답의 내용을 만드는 역할

* 웹 서버에 요청이 오면 장고로 전달됨
* 장고 urlresolver는 웹 페이지의 주소를 가져와서 무엇을 할지 확인함
* urlresolver는 패턴 목록을 가져와 URL과 맞는지 처음부터 하나씩 대조해서 식별
* 만약 일치하는 패턴이 있으면 장고는 해당 요청을 관련된 함수(view)에 넘겨줌
* 집배원이 주소와 번지가 일치하는 집에 편지를 배달하듯이! urlresolver = 집배원
* view함수에서 요청을 처리하고 응답을 생성하면 장고는 그 응답을 사용자의 웹 브라우저에 보내주는 역할을 한다

## 1. 가상환경에 django 설치하기
 ```
 # 가상환경 생성 후 활성화
 python -m venv venv
 source vevn/bin/active
 
 # django 설치
 pip install django
 ```
 
## 2. 장고 프로젝트 시작하기
* 장고의 기본 골격을 만들어주는 스크립트를 실행
* 장고에서는 디렉토리와 파일명이 매우 중요함!! (파일명을 변경X, 이동X)

```django-admin startproject mysite .```
* django-admin.py는 스크립트로 디렉토리와 파일들을 생성

```
- manage.py
- mysite
  - settings.py
  - urls.py
  - wsgi.py
  - __init__.py
```

* manage.py는 사이트 관리를 도와주는 역할
  * 이 스크립트로 데이터베이스를 생성 -> python manage.py migrate
  * 이 스크립트로 컴퓨터에서 웹 서버 구동 -> python manage.py runserver
* settings.py는 웹사이트 설정이 있는 파일
  * TIME_ZONE = 'Asia/Seoul'로 변경 가능
  * 정적 파일 경로 추가
  * 호스트 설정
  * 데이터베이스 설정
* urls.py는 urlresolver가 사용하는 패턴 목록을 포함하고 있음

## 장고 모델
* 객체(Object)란 속성(properties)과 행위(method)을 모아놓은 것

* 장고 안의 모델은 객체의 특별한 종류
* 이 모델을 저장하면 그 내용이 데이터베이스에 저장됨

* 어플리케이션 만들기
  * 프로젝트 내부에 blog라는 별도의 어플리케이션 생성
``` python manage.py startapp blog```
* blog 디렉토리의 구조
```
- blog
  - migrations
    - __init__.py
  - __init__.py
  - admin.py
  - models.py
  - tests.py
  - views.py
```
* 애플리케이션을 생성한 후 장고에게 사용한다고 알려줘야함
* mysite/settings.py에 INSTALLED_APPS에 blog를 추가

* 블로그 글 모델 만들기
  * blog/models.py 파일에 선언하여 Model 객체를 생성
* 데이터베이스에 모델을 위한 테이블 만들기 (Post 모델을 추가)
  * 장고 모델에 몇가지 변화가 생겼다는 것을 알려줌 -> python manage.py makemigrations blog
  * 이 변화를 데이터베이스에 반영 -> python manage.py migrate blog
  
## 장고 관리자
* 모델링한 글들을 장고 관리자에서 추가, 수정, 삭제 가능
* 모든 권한을 가지는 슈퍼 사용자 생성 -> python manage.py createsuperuser

## 장고 urls
* url은 웹 주소
* 인터넷의 모든 페이지는 고유한 URL을 가지고 있음
* URLconf는 장고에서 URL과 일치하는 뷰를 찾기 위한 패턴들의 집합
* mysite/urls.py에 include('blog.urls') 추가
* blog/urls.py에 urlpattern 추가

## 장고 뷰
* 뷰는 애플리케이션의 로직을 넣는 곳
* 모델에서 필요한 정보를 받아와서 템플릿에 전달하는 역할
* 장고 템플릿 양식은 HTML을 사용
