# M-5: Написать API для получения квартир
#everyone #tech #API   
>## Задание:
> - Написать API для получения списка квартир 
> - Написать API для получения получения одной квартиры
## Пример:
``` python
from rest_framework import serializers 
from .models import Flat  

class FlatSerializer(serializers.ModelSerializer): 
	class Meta: model = Flat 
	fields = '__all__' # Или укажите конкретные поля, например: ['id', 'title', 'price', 'area', 'rooms', 'address']
```
## Требований:
- Ориентируемся на [django-styleguide](https://github.com/HackSoftware/Django-Styleguide)
## Материалы:
- [API](../library/Django/API.md)
- [REST](../library/Django/REST.md)
- [Django REST Framework](../library/Django/Django%20REST%20Framework.md)
## Дальше:
1. [06-M Добавить swagger](06-M%20Добавить%20swagger.md)