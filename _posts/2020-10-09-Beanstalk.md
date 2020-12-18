---
title: " [Django Project] Beanstalk (1) 개발환경 세팅 "
author:
    박혜성

categories:
 - Django_Project
---


### Beanstalk 는 DRF 와 리액트를 이용해 만들 커뮤니티이다. (리눅스도 배우는 중이니 나중엔 리눅스 환경에서 개발할 예정이다.)

프로젝트를 만든 뒤 requirements.txt 파일을 생성

```shell
(venv) C:\123\Beanstalk> pip freeze > requirements.txt
#pip 로 인스톨된 앱들을 텍스트 파일에 저장함
(venv) C:\123\Beanstalk> pip install -r requirements.txt
# 텍스트 파일에 저장된 앱 리스트들을 인스톨함
```

아직 잘은 모르겠지만 일단 포스팅과 계정 앱을 만들자.
```shell
(venv) C:\123\Beanstalk>django-admin startproject Beanstalk .

(venv) C:\123\Beanstalk>python manage.py startapp post

(venv) C:\123\Beanstalk>python manage.py startapp account
```
앱을 만들었으니 settings.py 에 추가하자.
```python
#Beanstalk/settings/py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'post',
    'account',
]
```
