# M-8: Добавить API для получения информации по этажу
#feature #API 

> [!TODO] Задание
> - Просмотр информации по этажу - выборка по id. 
> - Характеристики этажа
> - Информацию о квартирах:
> 	- количество 
>	- список id

> [!example]- Пример
> > Для реализации API, который позволяет просматривать информацию о этаже с выборкой по ID, а также характеристики этажа и информацию о квартирах.
> 
> **1\) Создание модели `Floor`:**
> 
> Если у вас еще нет модели `Floor`, создайте её в файле `models.py` вашего приложения (например, `properties/models.py`). В этой модели будет храниться информация о этажах и связанных с ними квартирах.
> 
> ```python
>from django.db import models 
>
>class Flat(models.Model):     
>	title = models.CharField(max_length=100, verbose_name='Название')    
>	price = models.DecimalField(max_digits=10, decimal_places=2, verbose_name='Цена')    
>	area = models.DecimalField(max_digits=6, decimal_places=2, verbose_name='Площадь (м²)')    
>	rooms = models.PositiveIntegerField(verbose_name='Количество комнат')
>	address = models.CharField(max_length=255, verbose_name='Адрес')    
>	description = models.TextField(blank=True, verbose_name='Описание')    
>	created_at = models.DateTimeField(auto_now_add=True, verbose_name='Дата создания')    
>	updated_at = models.DateTimeField(auto_now=True, verbose_name='Дата обновления')
>	def __str__(self):
>		return self.title 
>
>class Floor(models.Model):     
>	number = models.PositiveIntegerField(verbose_name='Номер этажа')
>	flats = models.ManyToManyField(Flat, related_name='floors', verbose_name='Квартиры')
>	def __str__(self):        return f'Этаж {self.number}'     
>	class Meta:     
>		verbose_name = 'Этаж'
>		verbose_name_plural = 'Этажи'
>```
> 
> **2\) Создание сериализатора для `Floor`:**
> 
> Создайте или обновите файл `serializers.py` в вашем приложении (например, `properties/serializers.py`) и добавьте сериализатор для модели `Floor`:
> ```python
> 
> from rest_framework import serializers 
> from .models import Flat, Floor 
> 
> class FlatSerializer(serializers.ModelSerializer):     
> 	class Meta:        
> 		model = Flat        
> 		fields = ['id', 'title', 'price', 'area', 'rooms', 'address'] 
> class FloorSerializer(serializers.ModelSerializer):     
> 	 flats_count = serializers.IntegerField(source='flats.count', read_only=True)   
> 	 flats_ids = serializers.ListField(child=serializers.IntegerField()
> 	 source='flats.values_list', read_only=True)     
> 	class Meta:        
> 		model = Floor        
> 		fields = ['id', 'number', 'flats_count', 'flats_ids']`
>```
> 
> **Объяснение полей:**
> 
> - **flats_count**: Считает количество квартир на этаже.
> - **flats_ids**: Возвращает список ID квартир на этаже.
> 
> **3\) Создание представлений (views):**
> 
> В вашем файле `views.py` (например, `properties/views.py`) создайте представление для получения информации о этаже:
> ``` python
> 
> from rest_framework import generics 
> from .models import Floor 
> from .serializers import FloorSerializer 
> 
> class FloorDetailView(generics.RetrieveAPIView):     
> 	queryset = Floor.objects.all()    
> 	serializer_class = FloorSerializer`
> ```
> **4\) Настройка URL-адресов:**
> 
> Обновите файл `urls.py` вашего приложения (например, `properties/urls.py`) и добавьте маршрут для получения информации о этаже:
> ```python
> from django.urls import path 
> from .views import FlatListView, FlatDetailView, FloorDetailView 
> 
> urlpatterns = [     
> 	path('flats/', FlatListView.as_view(), name='flat-list'),    
> 	path('flats/<int:pk>/', FlatDetailView.as_view(), name='flat-detail'),
> 	path('floors/<int:pk>/', FloorDetailView.as_view(), name='floor-detail'),  # Новый маршрут для получения информации о этаже 
> 	]
> ```
> **5\) Тестирование API:**
> 
> Запустите сервер:
> ```bash
> python manage.py runserver
> ```
> 
> **6) Примеры запросов:**
> - **Получение информации о этаже**:
>     
>     - URL: `http://127.0.0.1:8000/api/floors/<id>/`
>     - Метод: GET
>     - Замените `<id>` на фактический идентификатор этажа.

> [!NOTE] Дальше
> - [09-M Добавить секцииазвания](09-M%20Добавить%20секцииазвания.md)