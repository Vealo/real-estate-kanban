# M-5: Написать API для получения квартир
#everyone #tech #API   
>[!TODO] Задание:
> - Написать API для получения списка квартир 
> - Написать API для получения получения одной квартиры
> 
> 	**Требования:**
>- Ориентируемся на [django-styleguide](https://github.com/HackSoftware/Django-Styleguide)

> [!example]- Пример:
> ``` python
> from rest_framework import serializers 
> from .models import Flat  
> 
> class FlatSerializer(serializers.ModelSerializer): 
> 	class Meta: model = Flat 
> 	fields = '__all__' # Или укажите конкретные поля, например: ['id', 'title', 'price', 'area', 'rooms', 'address']
> ```

> [!INFO]+  Материалы:
> - [API](../library/Django/API.md)
> - [REST](../library/Django/REST.md)
> - [Django REST Framework](../library/Django/Django%20REST%20Framework.md)

> [!NOTE] Дальше:
> 1. [06-M Добавить swagger](06-M%20Добавить%20swagger.md)