# 튜토리얼 따라하기 - 설문조사 
----------------
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
$ python manage.py startapp polls
```

## 뷰 만들기
<b>polls/views.py</b>
```
from django.http import HttpResponse

def index(request):
    return HttpResponse("votes index")
```

<b>polls/urls.py</b><br>
polls 폴더에 urls.py를 create
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

<b>config/urls.py</b>
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```

서버 실행
```
$ python manage.py runserver
```
127.0.0.1:8000/polls 접속<br>
polls/views.py에서 작성한 index를 확인할 수 있음.<br>
<img width="80%" alt="image" src="https://user-images.githubusercontent.com/23449575/220257342-0bd1c49e-8381-4f7f-9ae0-1d12a9eb1e36.png">

## 모델, 데이터베이스 생성
데이터베이스 생성 및 초기화
```
$ python manage.py migrate
```

<b>polls/models.py</b><br>
모델 생성
```
from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

class Choice(models.Model):
    quesiton = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

<b>config/settings.py</b><br>
'polls.apps.PollsConfig'추가
```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'polls.apps.PollsConfig',
]
```

데이터베이스에 적용
```
$ python manage.py makemigrations polls
```

<b>polls/models.py</b><br>
모델에 메서드 추가
```
import datetime

from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

    # __str__ 메서드는 관리자 화면이나 쉘에서 객체를 출력할 때 나타날 내용을 결정함.
    def __str__(self):
        return self.question_text

    def was_published_recently(self):
        return self.pub_date >= datetime.timezone.now() - datetime.timedelta(days=1)

class Choice(models.Model):
    quesiton = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

    # __str__ 메서드는 관리자 화면이나 쉘에서 객체를 출력할 때 나타날 내용을 결정함.
    def __str__(self):
        return self.choice_text
```

<b>polls/admin.py</b><br>
관리자 페이지에 Question모델 등록
```
from django.contrib import admin
from .models import Question

admin.site.register(Question)
```
