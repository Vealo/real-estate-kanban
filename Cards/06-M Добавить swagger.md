# M-6: Добавить swagger
#everyone #tech #swagger #DRF #drf_spectacular

> [!TODO] Задание:
> - Добавить swagger 
> 
> 	**Требование:**
> - Использовать drf_spectacular

> [!INFO]+ Материалы:
> - [Django REST Framework](../library/Django/Django%20REST%20Framework.md)
> - [Что такое сериализатор в Django REST Framework?](../library/Django/Что%20такое%20сериализатор%20в%20Django%20REST%20Framework?.md)

## Пример:
### 1\) Установим пакет `drf-spectacular` с помощью pip:
``` bush
pip install drf-spectacular
```
### 2\) Обновление `settings.py`  
2.1) Добавьте `drf_spectacular` в `INSTALLED_APPS`:

```python
INSTALLED_APPS = [     ...    'drf_spectacular', ]
```

2.2) Настройте класс схемы в настройках REST Framework:

```python
REST_FRAMEWORK = {     'DEFAULT_SCHEMA_CLASS': drf_spectacular.openapi.AutoSchema', }
```

2.3) Опционально, вы можете настроить дополнительные параметры для `SPECTACULAR_SETTINGS`:


```python
SPECTACULAR_SETTINGS = {
	'TITLE': 'API для квартир',
	'DESCRIPTION': 'Документация API для управления квартирами',
	'VERSION': '1.0.0', }
```

 ### 3\) Настройка URL-адресов

В вашем основном файле `urls.py` (например, `realty/urls.py`) добавьте маршруты для генерации схемы и Swagger UI:

```python
from django.contrib import admin
from django.urls import path, include 
from drf_spectacular.views import SpectacularAPIView, SpectacularSwaggerView, SpectacularRedocView 

urlpatterns = [
	path('admin/', admin.site.urls),    
	path('api/schema/', 
		 SpectacularAPIView.as_view(), name='schema'), # Схема API
	path('api/schema/swagger-ui/', 
		  SpectacularSwaggerView.as_view(url_name='schema'), 
		  name='swagger-ui'),  # Swagger UI    
	path('api/schema/redoc/',
		  SpectacularRedocView.as_view(url_name='schema'), 
		  name='redoc'),  # Redoc UI    
	path('api/', include('properties.urls')), # Подключение вашего приложения ]
```

### 3) Тестирование документации

Теперь вы можете запустить сервер:

``` bash
python manage.py runserver
```

### 4) Доступ к документации

- **Swagger UI**: Перейдите по адресу `http://127.0.0.1:8000/api/schema/swagger-ui/`
- **Redoc**: Перейдите по адресу `http://127.0.0.1:8000/api/schema/redoc/`

Эти страницы будут содержать автоматически сгенерированную документацию для вашего API.

### 5) Использование декораторов для настройки схемы (опционально)

Вы можете использовать декораторы из `drf-spectacular` для более точной настройки вашей схемы. Например, если у вас есть представление, вы можете добавить описание к эндпоинту:

```python
from drf_spectacular.utils import extend_schema @extend_schema(     responses={200: FlatSerializer(many=True)}, ) class FlatListView(generics.ListAPIView):     queryset = Flat.objects.all()    serializer_class = FlatSerializer
```

Это позволит вам более детально описать поведение ваших API-эндпоинтов. Теперь у вас есть полностью настроенная документация API с использованием `drf-spectacular`. Вы можете настраивать и расширять её по мере необходимости!

## Дальше:
- [07-M Добавить этажи](07-M%20Добавить%20этажи.md)
