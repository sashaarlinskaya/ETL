# Лабораторная работа 2. Динамические соединения с базами данных
## Титульный лист

**Дисциплина:** Проектный практикум по разработке ETL-решений
**Тема:** Установка и настройка ETL-инструмента. Создание конвейеров данных

**Вариант:** 3  

**Выполнила:** Арлинская Александра Викторовна  
**Проверил:** Босенко Тимур Муртазович  
**Курс обучения:** 4  
**Форма обучения:** очная  

**Институт цифрового образования**  
**Департамент информатики, управления и технологий**  
**Московский городской педагогический университет**  
**Москва 2025**

## Цель работы

Получить практические навыки создания сложного ETL-процесса, включающего динамическую загрузку файлов по HTTP, нормализацию базы данных, обработку дубликатов и настройку обработки ошибок с использованием Pentaho Data Integration (PDI).


**Описание входных данных.** 

Файл представляет собой таблицу в формате CSV (значения, разделенные точкой с запятой). Каждая строка — это отдельная позиция (товар) в заказе. Если один заказ состоит из нескольких товаров, он будет представлен несколькими строками.

---

### Шаг 1. Запуск Pentaho Spoonо
cd ~/Downloads/lab_etl/pdi-ce-9.4.0.0-343/data-integration

./spoon.sh

<img width="725" height="172" alt="image" src="https://github.com/user-attachments/assets/a68a7ebc-fb40-4884-ad73-a3641fb6361d" />


### Шаг 2. ETL Трансформации

Job

<img width="964" height="619" alt="image" src="https://github.com/user-attachments/assets/8cdc8e0f-1a7e-4723-bd4d-6c918fd23dad" />


Загрузка таблицы orders
<img width="935" height="337" alt="image" src="https://github.com/user-attachments/assets/3b317649-c896-48a7-b2b5-eba90a6b19f1" />

Фильтр orders (Сегмент: только Consumer)

<img width="1215" height="495" alt="image" src="https://github.com/user-attachments/assets/9062f7b6-0b9e-413f-9aeb-13a348460f92" />



Загрузка таблицы products

<img width="936" height="316" alt="image" src="https://github.com/user-attachments/assets/52794afc-5bc3-4851-a82b-6dba0e9c4374" />


Загрузка таблицы customers

<img width="694" height="311" alt="image" src="https://github.com/user-attachments/assets/01421e89-0126-41ef-bae7-727706f6392a" />


**Шаг 2.1. Настройка phpMyAdmin**

Создали таблицы
<img width="263" height="278" alt="image" src="https://github.com/user-attachments/assets/75a6d954-fa02-421e-bb6e-bc0298c17a2e" />


**Шаг 3. Загрузка данных**
Видим, что данные загрузились

<img width="1440" height="865" alt="image" src="https://github.com/user-attachments/assets/c933c5a4-222e-48e6-95d6-dccdea8080d4" />

<img width="1469" height="927" alt="image" src="https://github.com/user-attachments/assets/bace54c6-337b-4adf-9048-ab1378f047d2" />

<img width="1520" height="781" alt="image" src="https://github.com/user-attachments/assets/01a33f68-c925-42a8-b848-8928941f5420" />

### Задание по варианту 3

<img width="943" height="30" alt="image" src="https://github.com/user-attachments/assets/4a2406be-a453-4e55-8026-7154c537c305" />

**Сегмент только consumer**

Фильтр orders (Сегмент: только Consumer)

<img width="1215" height="495" alt="image" src="https://github.com/user-attachments/assets/9062f7b6-0b9e-413f-9aeb-13a348460f92" />

Проверяем 

<img width="1314" height="825" alt="image" src="https://github.com/user-attachments/assets/806bdece-8779-45e0-b241-e95caf345aed" />

**Отчет по скидкам**

<img width="958" height="341" alt="image" src="https://github.com/user-attachments/assets/81662da2-56da-4efe-a39f-8b43fc515055" />

<img width="665" height="554" alt="image" src="https://github.com/user-attachments/assets/0eb62f2e-2187-4ffa-9351-3cf77f74cf57" />

<img width="1167" height="496" alt="image" src="https://github.com/user-attachments/assets/a4d455fd-2455-43cf-a46e-6b3b990407c4" />


**Анализ по штатам**

<img width="597" height="221" alt="image" src="https://github.com/user-attachments/assets/bbf1d8f1-7f31-44dd-911c-48984a659428" />

<img width="1163" height="661" alt="image" src="https://github.com/user-attachments/assets/a0d76a33-d791-4987-8c83-edc7b2c00240" />

<img width="1326" height="657" alt="image" src="https://github.com/user-attachments/assets/49c946eb-5cda-4ecb-9f44-54e4b04043d1" />

<img width="520" height="310" alt="image" src="https://github.com/user-attachments/assets/717af3d8-441a-40c7-9215-020b6311f557" />

<img width="517" height="190" alt="image" src="https://github.com/user-attachments/assets/be5c4d9f-8a90-440d-a053-fc7457faeafe" />


