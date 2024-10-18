# Что такое сериализатор в Django REST Framework?
#JSON #XML #DRF #Сериализатор  
**Сериализаторы** позволяют преобразовывать сложные данные, такие как наборы запросов querysets и экземпляров моделей, в собственные типы данных Python, которые затем можно легко преобразовать в форматы **JSON**, **XML** или в другие типы содержимого. Сериализаторы также обеспечивают десериализацию, позволяя преобразовывать проанализированные данные обратно в сложные типы, после предварительной проверки входящих данных.

**Сериализаторы в DRF** работают очень похоже на классы Django `Form` и `ModelForm`.

Давайте рассмотрим следующий код. Чтобы создать базовый сериализатор, необходимо импортировать класс сериализаторов из `rest_framework` и определить поля для сериализатора так же, как при создании формы или модели в Django.

```python
from rest_framework import serializers
 
# create a serializer
class CommentSerializer(serializers.Serializer):
    # initialize fields
    email = serializers.EmailField()
    content = serializers.CharField(max_length = 200)
    created = serializers.DateTimeField()
```

Теперь можно использовать `CommentSerializer` для сериализации комментария или списка комментариев. Опять же, использование класса `Serializer` очень похоже на использование класса `Form`.

Сериализатор `CommentSerializer`, который мы рассматриваем, отвечает сейчас только за конвертацию данных в **JSON-формат** и обратно. Однако, хорошей практикой считается, когда здесь же в сериализаторе определяется алгоритм сохранения или изменения данных в БД. Этот функционал лучше создать в сериализаторе. Для этого каждый класс сериализаторов имеет два специальных метода:

- `**create(**self, validated_data**)**` – для добавления (создания) записи (данных);
- `**update(**self, instance, validated_data**)**` – для изменения данных (записи).

То есть чтобы создать эти методы в нашем сериализаторе необходимо создать эти функции.

```python
class CommentSerializer(serializers.Serializer):
    email = serializers.EmailField()
    content = serializers.CharField(max_length=200)
    created = serializers.DateTimeField()

    def create(self, validated_data):
        return Comment(**validated_data)

    def update(self, instance, validated_data):
        instance.email = validated_data.get('email', instance.email)
        instance.content = validated_data.get('content', instance.content)
        instance.created = validated_data.get('created', instance.created)
        return instance
```

Более подробно о работе сериализатора вы можете почитать на [странице документации](https://www.django-rest-framework.org/api-guide/serializers/).

**Django REST Framework** предлагает класс `Serializer`, который предоставляет вам мощный, универсальный способ управления выводом ваших ответов, но также у него есть класс `ModelSerializer`, который предоставляет еще более полезный и простой способ для создания сериализаторов.

Класс `ModelSerializer` аналогичен обычному классу `Serializer`, за исключением того, что: 

- Он автоматически сгенерирует для вас набор полей на основе модели.
- Он автоматически сгенерирует валидаторы для сериализатора, такие как валидаторы `unique_together`.
- Он включает простые реализации по умолчанию `.create()` и `.update()`.

Давайте создадим его, для этого в папке `blog_api` создадим файл `serializers.py` следующего содержания:

```python
from rest_framework import serializers
from blog.models import Post


class PostSerializer(serializers.ModelSerializer):
    class Meta:
        fields = (
            "id",
            "author",
            "title",
            "body",
            "created"
        )
        model = Post
```

Укажем в качестве базового класса `ModelSerializer` для нашего сериализатора `PostSerializer` и тогда уже нет необходимости вручную прописывать атрибуты для связи с соответствующими атрибутами модели `Post`, а также методы `create()` и `update()`. А вместо всего этого пропишем вложенный класс `Meta`.