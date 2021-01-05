# Django-Girls-Tutorial
Django Girls Tutorial을 따라하면서 장고의 개념 위주로 정리한 내용입니다.

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
   - 장고는 받는 사람마다 각각 다른 내용을 받도록하는 역할
 
## 장고란 무엇인가요?
* 파이썬으로 만들어진 무료 오픈소스 웹 어플리케이션 프레임워크
* 장고를 사용하면 웹사이트를 구축할때 필요한 요소를 쉽고 빠르게 구축할 수 있음

* 장고가 하는 일
  1. 웹서버에 요청이 오면 장고로 전달됨
  2. 장고의 urlresolver는 패턴 목록을 가져와서 요청 URL과 하나씩 대조해서 식별
  3. 만약 일치하는 패턴이 있으면 장고는 해당 요청을 관련 함수(view)로 넘겨줌
  4. view함수에서 해당 요청을 처리하고 응답을 생성
  5. 장고가 해당 응답을 사용자의 웹브라우저로 보내줌


## 1. 가상환경에 django 설치하기
 ```
 # 가상환경 생성 후 활성화
 python -m venv venv
 source vevn/bin/active
 
 # django 설치
 pip install django
 ```
 
## 2. 장고 프로젝트 만들기
```django-admin startproject mysite .```
* 장고의 기본 골격을 만들어주는 스크립트인 django-admin.py 실행
* 장고에서는 디렉토리와 파일명이 매우 중요함!! (파일명을 변경X, 이동X)

* 장고 프로젝트의 구조
```
- manage.py # 웹사이트 관리 담당 ex) DB 생성, 웹서버 구동
- mysite
  - settings.py # 웹사이트 설정 ex) 호스트, DB설정
  - urls.py # urlresolver가 사용하는 패턴 목록 포함
  - wsgi.py
  - __init__.py
```

## 3. 장고 어플리케이션 만들기
* 프로젝트안에 어플리케이션을 만들어줌
* 여기서 어플리케이션이란 어떤 동작을 하는 웹 어플리케이션을 의미

* blog라는 애플리케이션 생성
``` python manage.py startapp blog```
* blog의 구조 
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
  - mysite/settings.py에 INSTALLED_APPS에 blog를 추가


## 4. Model 만들기
* 장고에서 model은 파이썬의 객체(property + method)와 같음
* 하나의 model 클래스는 데이터베이스에서 하나의 테이블에 해

* blog/models.py에 필요한 model 객체를 선언
```python
from django.conf import settings
from django.db import models
from django.utils import timezone

class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
            default=timezone.now)
    published_date = models.DateTimeField(
            blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()
```
* 데이터베이스에 새로운 model 반영
```
python manage.py makemigrations blog
python manage.py migrate blog
```

## 5. 장고 관리자 설정
* 관리자 페이지에서 모델을 추가하거나 수정, 삭제 가능
* blog/admin.py에 관리자 페이지에서 보려는 모델 추가
```
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```python manage.py createsuperuser # 슈퍼사용자 생성
```

## 6. 장고 urls
* urls.py에서 요청 URL과 패턴이 일치하는 view 함수를 매칭
* mysite/urls.py에서 blog.urls를 가져오는 행 추가
* blog/urls.py에 url pattern 추가
```
urlpatterns = [
    # 장고에게 누군가 웹사이트에 'http://127.0.0.1:8000/' 주소로 들어오면 views.post_list를 보여줘!라는 뜻
    path('', views.post_list, name='post_list'),
]
```

## 7. 장고 뷰(View)
* 뷰는 어플리케이션의 로직을 넣는 곳
* 모델에서 필요한 정보를 받아와서 템플릿(html)에 전달하는 역할

* post_list라는 뷰 선언
```
# post 모델에서 필요한 정보를 받아와서 post_list.html에 전달
def post_list(request):
    return render(request, 'blog/post_list.html', {})
```

## 8. 장고 ORM과 쿼리셋(QuerySets)
* 쿼리셋(QuerySet)은 전달받은 모델의 객체 목록
* 데이터베이스로부터 데이터를 읽고, 필터를 걸거나 정렬을 할 수 있음

1. 모든 객체 조회하기```Post.objects.all()```
2. 객체 생성하기 ```Post.objects.create(title='Sample title', text='Test')```
3. 필터링하기 ```Post.objects.filter(title__contains='title')```
4. 정렬하기 ```Post.objects.order_by('created_date')```
5. 쿼리셋 연결하기 ```Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')```

