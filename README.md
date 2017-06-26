# my_jango
## django tutorial

### $ipython manage.py runserver

### app <= project

### app 생성 (top-level)
$python manage.py startapp polls

-> polls/views.py
$from django.http import HttpResponse
$def index(request):
$   return HttpResponse("Hello, world. You're at the polls index.")

-> polls/urls.py
$from django.conf.urls import url
$from . import views
$urlpatterns = [
$    url(r'^$', views.index, name='index'),
$]

-> mysite/urls.py
$from django.conf.urls import include, url
$from django.contrib import admin
$urlpatterns = [
$    url(r'^polls/', include('polls.urls')),
$    url(r'^admin/', admin.site.urls),
$]

### DB 생성 (SQLite)

이 어플리케이션들은 일반적인 경우에 사용하기 편리하도록 기본으로 제공됩니다.

이러한 기본 어플리케이션들 중 몇몇은 최소한 하나 이상의 데이터베이스 테이블을 사용하는데, 그러기 위해서는 데이터베이스에서 테이블을 미리 만들 필요가 있습니다. 이를 위해, 다음의 명령을 실행해봅시다.

$ python manage.py migrate
migrate 명령은 INSTALLED_APPS 의 설정을 탐색하여, mysite/settings.py 의 데이터베이스 설정과 app 과 함께 제공되는 데이터베이스 migrations(나중에 설명하겠습니다) 에 따라, 필요한 데이터베이스 테이블을 생성합니다. 이 명령을 수행하면 각 migration 이 적용되는 메세지가 화면에 출력되는 것을 확인할 수 있습니다.

### 모델 생성
mysite/settings.py
INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
이제, Django 는 polls app 이 포함된 것을 알게 되었습니다. 다른 명령을 내려봅시다.

$ python manage.py makemigrations polls
다음과 비슷한 것이 보일겁니다.:

Migrations for 'polls':
  polls/migrations/0001_initial.py:
    - Create model Choice
    - Create model Question
    - Add field question to choice
makemigrations 을 실행시킴으로서, 당신이 모델을 변경시킨 사실과(이 경우에는 새로운 모델을 만들었습니다) 이 변경사항을 migration 으로 저장시키고 싶다는 것을 Django 에게 알려줍니다.

migration 은 Django가 모델(즉, 데이터베이스 스키마를 포함한)의 변경사항을 저장하는 방법으로써, 디스크상의 파일로 존재합니다. 원한다면, polls/migrations/0001_initial.py 파일로 저장된 새 모델에 대한 migration 을 읽어볼 수 있습니다. 걱정하지 마십시요, Django 가 migration 을 만들때마다 직접 읽어보실 필요는 없습니다만, 수동으로 Django 의 변경점을 조정하고 싶을때 사람이 직접 변경할 수 있도록 설계되어 있습니다.

당신을 위해 migration 들을 실행시켜주고, 자동으로 데이터베이스 스키마를 관리해주는 migrate 라는 명령어가 존재합니다. 이 명령을 알아보기 전에 migration 이 내부적으로 어떤 SQL 문장을 실행하는지 살펴봅시다. sqlmigrate 명령은 migration 이름을 인수로 받아, 실행하는 SQL 문장을 보여줍니다.

mysite/settings.py
INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
이제, Django 는 polls app 이 포함된 것을 알게 되었습니다. 다른 명령을 내려봅시다.

$ python manage.py makemigrations polls
다음과 비슷한 것이 보일겁니다.:

Migrations for 'polls':
  polls/migrations/0001_initial.py:
    - Create model Choice
    - Create model Question
    - Add field question to choice
makemigrations 을 실행시킴으로서, 당신이 모델을 변경시킨 사실과(이 경우에는 새로운 모델을 만들었습니다) 이 변경사항을 migration 으로 저장시키고 싶다는 것을 Django 에게 알려줍니다.

migration 은 Django가 모델(즉, 데이터베이스 스키마를 포함한)의 변경사항을 저장하는 방법으로써, 디스크상의 파일로 존재합니다. 원한다면, polls/migrations/0001_initial.py 파일로 저장된 새 모델에 대한 migration 을 읽어볼 수 있습니다. 걱정하지 마십시요, Django 가 migration 을 만들때마다 직접 읽어보실 필요는 없습니다만, 수동으로 Django 의 변경점을 조정하고 싶을때 사람이 직접 변경할 수 있도록 설계되어 있습니다.

당신을 위해 migration 들을 실행시켜주고, 자동으로 데이터베이스 스키마를 관리해주는 migrate 라는 명령어가 존재합니다. 이 명령을 알아보기 전에 migration 이 내부적으로 어떤 SQL 문장을 실행하는지 살펴봅시다. sqlmigrate 명령은 migration 이름을 인수로 받아, 실행하는 SQL 문장을 보여줍니다.

$ python manage.py sqlmigrate polls 0001

이제, migrate 를 실행시켜 데이터베이스에 모델과 관련된 테이블을 생성해봅시다.

$ python manage.py migrate

잠깐만요, <Question: Question object> 는 이 객체를 설명하는데에 정말 하나도 도움이 안되네요. (polls/models.py 파일의) Question 모델을 수정하여 __str__() 메소드를 Question 과 Choice 에 추가해 봅시다.

polls/models.py
from django.db import models
from django.utils.encoding import python_2_unicode_compatible

@python_2_unicode_compatible  # only if you need to support Python 2
class Question(models.Model):
    # ...
    def __str__(self):
        return self.question_text

@python_2_unicode_compatible  # only if you need to support Python 2
class Choice(models.Model):
    # ...
    def __str__(self):
        return self.choice_text
당신의 모델에 __str__() 메소드를 추가하는것은 객체의 표현을 대화식 프롬프트에서 편하게 보려는 이유 말고도, Django 가 자동으로 생성하는 관리 사이트 에서도 객체의 표현이 사용되기 때문입니다.

이것들은 모두 보통의 Python 메소드입니다. 예시를 위해 수정된 메소드를 추가해 보겠습니다:

polls/models.py
import datetime

from django.db import models
from django.utils import timezone


class Question(models.Model):
    # ...
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)
import datetime 은 Python 의 표준 모듈인 datetime 모듈이며, from django.utils import timezone 은 Django 의 시간대 관련 유틸리티인 django.utils.timezone 을 의미합니다.

### 관리자 생성
$ python manage.py createsuperuser
