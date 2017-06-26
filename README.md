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
