# Simple-JSON Web Token Authentication for Django REST Framework

Django REST Frameworkで非常に簡単なJWT認証を実装してみた。

# インストール

```
pip install django
pip install djangorestframework
pip install djangorestframework-simplejwt
django-admin startproject backend .
py manage.py migrate //データベースを作成する上では必要不可欠。
```

# ディレクトリ

```
│  .gitignore
│  db.sqlite3
│  manage.py
│  README.md
│  
└─backend
    │  asgi.py
    │  settings.py
    │  urls.py
    │  wsgi.py
    │  __init__.py
    │
    └─__pycache__
            settings.cpython-310.pyc
            urls.cpython-310.pyc
            wsgi.cpython-310.pyc
            __init__.cpython-310.pyc
```

# 基本設定

`backend/settings.py`で、以下のコードを追加する。

```py
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': {
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    }
}
```

`backend/urls.py`にアクセスして、以下のプログラムを書いてDjangoに簡単なJWT認証を実装できるように命令する。

```py
# backend/urls.py
from django.contrib import admin
from django.urls import path
from rest_framework_simplejwt.views import (
    TokenObtainPairView,
    TokenRefreshView,
)

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
    path('api/token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
]
```

再度`backend/settings.py`にアクセスして、変数`INSTALL_APPS`に`'rest_framework_simplejwt'`を追加する。

```py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework_simplejwt', #追加
]
```

以下のコマンドをターミナルで入力。

```
py manage.py createsuperuser
```

これで管理者(`superuser`)を作成して、仮想サーバを立ち上げる。

```
py manage.py runserver
```

あとは、`http:localhost:8000/api/token`にアクセスして`username`と`password`を入力しておけばTokenが発行され、簡単にJWT認証を実装できる。


# 開発環境

* Python 3.10.1
* Django 4.0
* Django REST Framework 3.13
* Visual Studio Code 1.63
* djangorestframework-simplejwt 5.0.0