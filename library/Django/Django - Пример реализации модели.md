# Пример реализации модели
> Для реализации модели `Flat` (квартира) в Django, следуйте приведенным ниже шагам. Эта модель будет включать основные поля, такие как цена, площадь, количество комнат и т.д.

## Шаг 1: Создание модели

1. **Откройте файл `models.py`** вашего приложения (`realty/models.py`).
2. **Добавьте следующую модель `Flat`:**

``` python
from django.db 
import models 

class Flat(models.Model):     
	title = models.CharField(max_length=100, 
				verbose_name='Название')
	price = models.DecimalField(max_digits=10, decimal_places=2,
			verbose_name='Цена')
	area = models.DecimalField(max_digits=6, decimal_places=2,
			verbose_name='Площадь (м²)')
	rooms = models.PositiveIntegerField(verbose_name='Количество комнат')
	address = models.CharField(max_length=255, verbose_name='Адрес')
	description = models.TextField(blank=True, verbose_name='Описание')
	created_at = models.DateTimeField(auto_now_add=True,
			verbose_name='Дата создания')
	updated_at = models.DateTimeField(auto_now=True, 
			verbose_name='Дата обновления')
	
	def __str__(self):
		return self.title
```
## Описание полей:

- **title**: Название квартиры.
- **price**: Цена квартиры (с двумя знаками после запятой).
- **area**: Площадь квартиры в квадратных метрах.
- **rooms**: Количество комнат в квартире.
- **address**: Адрес квартиры.
- **description**: Дополнительное описание квартиры (может быть пустым).
- **created_at**: Дата и время создания записи.
- **updated_at**: Дата и время последнего обновления записи.