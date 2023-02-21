# 튜토리얼 따라하기 - 설문조사 

## 프로젝트 생성
장고 설치
```
$ pip install django
```
장고 프로젝트 생성
```
$ django-admin startproject config .
```
앱 생성
```
$ python manage.py startapp votes
```

## 뷰 만들기
votes/views.py
```
from django.http import HttpResponse

def index(request):
    return HttpResponse("votes index")
```

votes 폴더에 urls.py를 create<br>
votes/urls.py
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

config/urls.py
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('votes/', include('votes.urls')),
    path('admin/', admin.site.urls),
]
```

서버 실행
```
$ python manage.py runserver
```
127.0.0.1:8000/votes 접속<br>
votes/views.py에서 작성한 index를 확인할 수 있음.<br>
<img width="80%" alt="image" src="https://user-images.githubusercontent.com/23449575/220257342-0bd1c49e-8381-4f7f-9ae0-1d12a9eb1e36.png">
