>Django REST Framework поставляется с набором типовых представлений и миксинов, которые можно использовать для разработки представлений API. Они предоставляют функциональность извлечения, создания, обновления или удаления модельных объектов. Все типовые поставляемые с фреймворком REST миксинные классы и представления находятся по адресу [https://www.django-rest-framework.org/api-guide/generic-views/](https://www.django-rest-framework.org/api-guide/generic-views/).

# APIView

Класс `APIView` - это низкоуровневая реализация (фундамент) для всех остальных DRF-представлений на основе классов. От него будут наследоваться все остальные DRF-представления на основе классов, и он предоставляет им базовую функциональность.

Использование класса `APIView` во многом аналогично использованию класса `View`: как и в нём, входящий запрос отправляется соответствующему методу-обработчику, например `.get()` или `.post()`, которые мы можем переопределить в своём представлении на основе класса `APIView`. Кроме того, в классе может быть установлен ряд атрибутов, которые управляют различными аспектами политики API.

### Создание представлений с помощью APIView

Давайте посмотрим, как создавать представления с помощью `APIView`.

Добавим в файл `views.py` приложения `blog_api`, следующий код:

```python
from django.http import Http404

from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

from blog.models import Post
from .serializers import PostSerializer


class PostList(APIView):
    def get(self, request, format=None):
        posts = Post.objects.all()
        serializer = PostSerializer(posts, many=True)
        return Response(serializer.data)

    def post(self, request, format=None):
        serializer = PostSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data,
                            status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)


```

Помимо методов `get()` и `post()` можно прописывать и другие для обработки соответствующих типов запросов:

- `**get()**` – вызывается при GET-запросах;
- `**post()**` – вызывается при POST-запросах;
- `**put()**` – вызывается при PUT-запросах;
- `**patch()**` – вызывается при PATCH-запросах;
- **`delete()`** – вызывается при DELETE-запросах.

Рассмотрим их при создании детального отображения постов, добавим в файл `views.py` приложения `blog_api`, следующий код:

```python
class PostDetail(APIView):
    def get_object(self, pk):
        try:
            return Post.objects.get(pk=pk)
        except Post.DoesNotExist:
            raise Http404

    def get(self, request, pk, format=None):
        post = self.get_object(pk)
        serializer = PostSerializer(post)
        return Response(serializer.data)

    def put(self, request, pk, format=None):
        post = self.get_object(pk)
        serializer = PostSerializer(post, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def patch(self, request, pk, format=None):
        post = self.get_object(pk)
        serializer = PostSerializer(post,
                                    data=request.data,
                                    partial=True)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def delete(self, request, pk, format=None):
        queryset = self.get_object(pk)
        queryset.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```

  
Давайте проверим работу, для этого перейдем по адресу [http://127.0.0.1:8000/api/](http://127.0.0.1:8000/api/):

![](https://ucarecdn.com/96a05212-facc-4d8f-80b9-4d3b9d0fa3e4/)

  
Давайте попробуем создать новую запись, для этого опустимся в самый низ страницы и введем тестовую запись:

![](https://ucarecdn.com/5f1369eb-51af-42d1-9856-152d66dc222a/)

  
После нажатия кнопки `POST` мы видим успешное выполнение данной операции, а именно функции `post` в классе `PostList`:

![](https://ucarecdn.com/2f35d14d-9d71-4b86-8b85-8ac2e819a1c8/)

Мы видим успешный ответ сервера `HTTP 201 Created`, и нашу запись.

  
Вернемся на главную страницу нашего API [http://127.0.0.1:8000/api/](http://127.0.0.1:8000/api/) и проверим наш список постов:

![](https://ucarecdn.com/bfdceba6-1738-49fc-81c5-e5869673ac48/)

  
Далее давайте перейдем к просмотру нашей записи через API, в моем случае у записи `id=6`, поэтому перейдем по адресу [http://127.0.0.1:8000/api/6/](http://127.0.0.1:8000/api/6/).

![](https://ucarecdn.com/a64d568d-daf2-421d-b5f1-194183049077/)

На скриншоте выше мы видим что у нас есть кнопки `PUT`, `PATCH` и `DELETE`. Это именно те функции которые мы добавили в нашем представлении `PostDetail`.

У вас здесь может возникнуть вопрос, а как одно и то же представление для одного и того же URL-адреса «понимает», какой из методов следует вызывать, когда приходит какой-либо запрос от клиента? В действительности, все просто. Когда клиент формирует запрос, то в его заголовке прописывается тип этого запроса: GET, POST, DELETE, PUT и т.п. Затем, алгоритм, заложенный в представления DRF автоматически вызывает метод, связанный с поступившим запросом. Если нужный метод не находится, то клиенту возвращается JSON-строка с сообщением, что метод не разрешен.