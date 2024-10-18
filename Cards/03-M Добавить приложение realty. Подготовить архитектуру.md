# M-3: Добавить приложение realty. Подготовить архитектуру
#everyone #tech  
> Основное приложение с недвижимостью.  

## Задание:
### 1) Добавьте приложение  realty
**Пример команды**
``` bash
python manage.py startapp realty
```
**Результат**:
``` bash
app/ 
	manage.py 
	config/ 
		__init__.py 
		settings.py 
		urls.py 
		asgi.py 
		wsgi.py
	realty/
		migrations/
		__init__.py
		admin.py
		apps.py
		models.py
		tests.py
		views.py
```
### 2) Реализуйте классы моделей
​	Вспомогательный материалы:  
- [Django - Модель](../library/Django/Django%20-%20Модель.md)
- [Django - Пример реализации модели](../library/Django/Django%20-%20Пример%20реализации%20модели.md)

## Требования к классам моделей:
1) Подготовить архитектуру в соответствии с этими рекомендациями [https://github.com/HackSoftware/Django-Styleguide](https://github.com/HackSoftware/Django-Styleguide)
3) Классы моделей пока могут быть пустые

## Пример классов realty/models.py

Ниже приведены четыре возможных класса:

| \#  | Классы   | Описание |
| :-: | -------- | -------- |
|  1  | Building | Здание   |
|  2  | Section  | Секция   |
|  3  | Floor    | Этаж     |
|  4  | Flat     | Квартира |
## Дальше:
[04-M Добавить квартиры на сайт](04-M%20Добавить%20квартиры%20на%20сайт.md)