# М-2: Добавить DRF в проект
#everyone #tech #DRF

1\) Устанавливаем в [виртуальное окружение](../library/Виртуальное%20окружение.md) DRF  
``` python
pip install djangorestframework
```
2\) Обновляем файл: `settings.py`  
2.1) Добавляем фреймворм в установленные апп:  
``` python
INSTALLED_APPS = [ 
		...,
		'rest_framework', ]
```
2.2) Добавляем новую переменную ниже:  
``` python
REST_FRAMEWORK = { 'DEFAULT_AUTHENTICATION_CLASSES': [ 'rest_framework.authentication.BasicAuthentication', 'rest_framework.authentication.SessionAuthentication', ], 'DEFAULT_PERMISSION_CLASSES': [ 'rest_framework.permissions.IsAuthenticated', ], }
```