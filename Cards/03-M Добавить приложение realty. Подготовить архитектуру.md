# M-3: Добавить приложение realty. Подготовить архитектуру
#everyone #tech  
> Основное приложение с недвижимостью.  

``` bash
python manage.py startapp realty
```
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

Подготовить архитектуру в соответствии с этими рекомендациями [https://github.com/HackSoftware/Django-Styleguide](https://github.com/HackSoftware/Django-Styleguide)

Классы пока могут быть пустые

## Пример классов realty/models.py

Ниже приведены четыре возможных класса:

| \#  | Классы   | Описание |
| :-: | -------- | -------- |
|  1  | Building | Здание   |
|  2  | Section  | Секция   |
|  3  | Floor    | Этаж     |
|  4  | Flat     | Квартира |
