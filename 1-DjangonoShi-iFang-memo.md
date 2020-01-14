# 管理画面

## データベースの定義

### データベースを調べるコマンド
      manage.py dbshell

### テーブルの定義を調べるコマンド
      manage.py inspectdb

### データベースの設定

#### project/settings.py

```python
DATABASES = {
    default': {
    'ENGINE': 'django.db.backends.postgresql',
    'NAME': 'nagaoka_dev',
    'USER': 'js',
    'PASSWORD': 'tangerine3',
    'HOST': '127.0.0.1',
    'PORT': '5433',
    }
}
```

### アプリケーションの設定

#### project/apps.py

``` python
from django.apps import AppConfig

class TestappConfig(AppConfig):
    #nameにprojectを追加する
    name = 'testprj.testapp'
```

#### project/settings.py

``` python
INSTALLED_APPS = [
    'testprj.testapp.apps.TestappConfig',
]
```

## URLの設定

#### project/urls.py

``` python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```

## モデルの作成

#### testapp/models/app_user.py

``` python
#djangoにUserというモデル名は避けるべき
from django.db import models

class AppUser(models.Model):
    id = models.BigAutoField(primary_key=True)
    created_at = models.DateTimeField()
    updated_at = models.DateTimeField()
    name = models.CharField(max_length=256)

class Meta:
    managed = False
    db_table = 'users'

def __str__(self):
  return self.name
```

#### app/models/\_\_init.py__

``` python
from django.contrib.auth.models import *
```

#### project/admin.py

``` python
from django.contrib import admin
from .models.app_user import AppUser

class UserAdmin(admin.ModelAdmin):
    list_display = ('id', 'name', 'created_at', 'updated_at', )
    list_display_links = ('name', )

admin.site.register(AppUser, UserAdmin)
```
