## Django REST Framework

[Django REST Framework](https://www.django-rest-framework.org/) (DRF) — это широко используемая полнофункциональная платформа API, предназначенная для создания RESTful API с помощью Django. По своей сути DRF интегрируется с основными функциями Django — моделями, представлениями и URL-адресами — что упрощает создание RESTful API.

DRF состоит из следующих компонентов:

1. `**Сериализаторы**` используются для преобразования Django QuerySets и экземпляров моделей в (сериализация) и из (десериализация) JSON (и ряд других форматов рендеринга данных, таких как XML и YAML).
    
2. `**Представления**` (наряду с наборами представлений), которые похожи на традиционные представления Django, обрабатывают HTTP-запросы и ответы RESTful. Само представление использует сериализаторы для проверки входящих полезных данных и содержит необходимую логику для возврата ответа. Наборы представлений связаны с маршрутизаторами , которые сопоставляют представления с открытыми URL-адресами.
    
3. `**Маршрутизаторы**`: определяют URL-адреса, которые будут предоставлять доступ к каждому представлению.
    

![Django REST Framework](_attachments/Django%20REST%20FrameWork.png)

### Установка Django REST Framework

Работать с данным фреймворком мы будем на основе нашего блога. Мы прекрасно понимаем что вряд ли блогу когда-то понадобится API функции, но так вам будет проще разобраться с DRF, так как в блоге уже реализованы функции постов, регистрация и авторизация и тд.

Давайте установим DRF введя в консоли следующий текст:

```bash
pip install djangorestframework
```

  
Затем добавьте его в раздел `INSTALLED_APPS` нашего файла `mysite/settings.py`.

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog.apps.BlogConfig',
    'taggit',
    'django.contrib.sites',
    'django.contrib.sitemaps',
    'django.contrib.postgres',
    'accounts.apps.AccountsConfig',
    'social_django',
    'django_bootstrap5',
    # API new
    'rest_framework',
]

REST_FRAMEWORK = {
    "DEFAULT_PERMISSION_CLASSES": [
        "rest_framework.permissions.AllowAny",
    ],
}
```

По умолчанию Django REST Framework настроен на `AllowAny`, чтобы облегчить нам локальную разработку, однако это не безопасно. Мы разберем эту настройку разрешений в следующих разделах.

  
Следующим шагом создадим новое приложение, назовем его `blog_api`.

```bash
python manage.py startapp blog_api
```

  
Не забываем зарегистрировать наше приложение в `settings.py`:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog.apps.BlogConfig',
    'taggit',
    'django.contrib.sites',
    'django.contrib.sitemaps',
    'django.contrib.postgres',
    'accounts.apps.AccountsConfig',
    'social_django',
    'django_bootstrap5',
    # API new
    'rest_framework',
    'blog_api.apps.BlogApiConfig', # new
]
```

  
А в файл `mysite/urls.py` добавим новый маршрутизатор:

```python
path("api/", include("blog_api.urls")),
```

  
Хорошей практикой является постоянное версионирование API, поскольку при внесении больших изменений может возникнуть некоторая задержка, прежде чем различные потребители API также смогут обновиться.

Например создание маршрутов типа:

```python
path("api/v1/", include("blog_api_v1.urls")), # API версия 1
path("api/v2/", include("blog_api_v2.urls")), # API версия 2
```

 Поможет вам поддерживать v1 API в течение определенного периода времени, одновременно запуская новую, обновленную v2, и избегая поломки других приложений, которые полагаются на бэкэнд вашего API.

  
Теперь помощью DRF мы сделаем маршруты вывода нашего списка постов с возможностью доступа к каждому посту. Для этого в папке `blog_api` создадим `urls.py` следующего содержания:

```python
from django.urls import path
from .views import PostList, PostDetail

urlpatterns = [
    path("<int:pk>/", PostDetail.as_view(), name="post_detail"),
    path("", PostList.as_view(), name="post_list"),
]
```

В верхней части файла мы импортировали два представления - `PostList` и `PostDetail` - о которых мы напишем в файле `views.py`, и они соответствуют списку всех постов блога по пустой строке, `""`, что означает по адресу `api/`.

Отдельные подробные посты будут находиться по их первичному ключу `pk`, так что первая запись блога будет будет находиться по адресу `api/1/`, вторая - по адресу `api/2/`, и так далее.