# Лабораторная работа 1.1 Установка и настройка ETL-инструмента. Создание конвейеров данных
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

Изучение основных принципов работы с ETL-инструментами на примере Pentaho Data Integration (PDI), настройка среды, создание конвейера обработки данных (фильтрация, очистка, замена значений) и выгрузка результатов в базу данных MySQL.


**Источник данных.** [[https://www.kaggle.com/datasets/usgs/earthquake-database](https://www.kaggle.com/datasets/mirichoi0218/insurance?resource=download)]

**Описание входных данных.** 

Это датасет с данными о медицинском страховании клиентов. Содержит информацию о 1338 клиентах и 7 характеристиках:

age — возраст клиента (18–64 года)

sex — пол (male/female)

bmi — индекс массы тела (от 15.96 до 53.13)

children — количество детей на иждивении (0–5)

smoker — статус курения (yes/no)

region — регион проживания (southeast, southwest, northeast, northwest)

charges — индивидуальные страховые взносы (от 1121 до 63770)

---

### Шаг 1. Запуск Pentaho Spoonо
cd ~/Downloads/lab_etl/pdi-ce-9.4.0.0-343/data-integration

./spoon.sh
<img width="725" height="172" alt="image" src="https://github.com/user-attachments/assets/a68a7ebc-fb40-4884-ad73-a3641fb6361d" />


### Шаг 2. Настройка среды
**Шаг 2.1. Настройка ETL-среды**

Загрузка файла
<img width="1272" height="794" alt="image" src="https://github.com/user-attachments/assets/07773dfd-5a9d-4a16-bd52-3d9e421a650c" />

Настройка фильтра
<img width="1213" height="279" alt="image" src="https://github.com/user-attachments/assets/7f4333c3-614c-4ee6-8c8c-9b273b66f446" />

Настройка подключения
<img width="1207" height="732" alt="image" src="https://github.com/user-attachments/assets/f57c849f-60da-45f9-a12a-f48e12aa6d45" />

Выход данных
<img width="948" height="607" alt="image" src="https://github.com/user-attachments/assets/36dc3d14-11f9-411f-bb31-95385e39c155" />

Общий вид

<img width="917" height="315" alt="image" src="https://github.com/user-attachments/assets/d43d4e2d-c8f0-414c-a85b-5b10435f998a" />

**Шаг 2.1. Настройка phpMyAdmin**

Создали таблицу
<img width="1278" height="369" alt="image" src="https://github.com/user-attachments/assets/75aa4acb-d13f-4776-82c8-09ff15b85960" />

**Шаг 3. Загрузка данных**
Видим, что данные загрузились
<img width="722" height="769" alt="image" src="https://github.com/user-attachments/assets/d5e37996-b5ba-4c3a-8e5b-a0f6fb40ddd1" />

Запрос для проверки данных
<img width="837" height="522" alt="image" src="https://github.com/user-attachments/assets/c7803376-27a0-4f6e-b92e-7f2442378eeb" />

