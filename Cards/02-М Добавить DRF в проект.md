# М-2: Добавить DRF в проект
#everyone #tech #DRF
>[!TODO] Задание:
>Установить DRF

> [!INFO]- Пример
> 1\) Устанавливаем в [виртуальное окружение](../library/Виртуальное%20окружение.md) DRF  
> 
> ``` python
> pip install djangorestframework
> ```
> 2\) Обновляем файл: `settings.py`  
> 2.1) Добавляем фреймворм в установленные апп:  
> 
> ``` python
> INSTALLED_APPS = [ 
> 		...,
> 		'rest_framework', ]
> ```
> 2.2) Добавляем новую переменную ниже:  
> ``` python
> REST_FRAMEWORK = { 
> 	'DEFAULT_AUTHENTICATION_CLASSES':
>         ['rest_framework.authentication.BasicAuthentication',
>          'rest_framework.authentication.SessionAuthentication', ],
>     'DEFAULT_PERMISSION_CLASSES': 
>      	['rest_framework.permissions.IsAuthenticated', ], }
> ```

> [!NOTE]+ Дальше:
> - [03-M Добавить приложение realty. Подготовить архитектуру](03-M%20Добавить%20приложение%20realty.%20Подготовить%20архитектуру.md)