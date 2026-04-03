# Лабораторная работа №3. Комплексное ETL-решение (Вариант 3)

## Цель работы
Разработать решение для интеграции данных из PostgreSQL (база розничных магазинов), Excel (поставки) и CSV (основной склад) в целевое хранилище MySQL, с последующим построением аналитической витрины (View).

## Задание (Вариант 3: Склад)
**Задача:** Создать систему учета остатков. Объединить остатки склада и магазинов, выявить расхождения с поставками. 
**Источники:**
1. PostgreSQL: Розничные магазины (1 000 000 строк).
2. CSV: Основной склад (100 000 строк).
3. Excel: Поставки (100 000 строк).

**Условие фильтрации:** 
Так как целевая таблица MySQL не должна получить весь миллион записей, в процессе ETL (Pentaho) применяется фильтрация: выбираются только записи с критически низким остатком в магазинах (`store_balance < 50`). Это позволяет сократить объем загружаемых данных примерно на 90% (оставив ~100 000 строк), что соответствует заданию. Вычисляется поле `discrepancy` (расхождение): `(store_balance + warehouse_balance) - delivery_qty`.

---

## 1. Архитектура решения
Схема архитектуры разработана с использованием трех слоев:
- **Source Layer:** PostgreSQL, файлы CSV и Excel.
- **Storage Layer:** Staging Area (Pentaho Data Integration) и целевая БД MySQL.
- **Business Layer:** Аналитическое представление (View) в MySQL.



<img width="980" height="500" alt="image" src="https://github.com/user-attachments/assets/c4c77216-1c40-4db6-a4c3-400c31f0b444" />

---

## 2. Подготовка данных

### Шаг 2.1. Генерация данных для PostgreSQL
Был подготовлен скрипт (в файле `sql_scripts.sql`), который создает таблицу `retail_stores` и с помощью функции `generate_series` генерирует **1 000 000 строк** случайных данных (ID магазина, ID продукта, остаток, дата и категория товара).

Создание БД:

<img width="902" height="453" alt="image" src="https://github.com/user-attachments/assets/f56b5878-673f-4682-8945-b1be8155a090" />


Наполнение БД:

<img width="685" height="531" alt="image" src="https://github.com/user-attachments/assets/c27f462c-547e-449c-94c9-cb3dfe59ec87" />

<img width="844" height="698" alt="image" src="https://github.com/user-attachments/assets/13c875f0-71ca-47dc-ab85-84fb3043dd4f" />

### Шаг 2.2. Генерация файлов Excel
Чтобы создать файлы с **100 000 строк**, используется скрипт `generate_data.py` (написан на Python с использованием библиотек `pandas` и `numpy`).
- `deliveries.xlsx` - содержит информацию о количестве поставленного товара (`delivery_qty`).

### Шаг 2.3. Сооздание данных MySQL (исх): Основной склад.

<img width="910" height="843" alt="image" src="https://github.com/user-attachments/assets/a6d21289-cf0a-46fa-9e53-ce7dbf977f42" />

Выполнение скрипта по генерации данных:

<img width="872" height="759" alt="image" src="https://github.com/user-attachments/assets/8bde6c29-8d2f-4f73-ba10-40d35828d25e" />
---

## 3. ETL-процесс в Pentaho (Spoon)
Общий вид:

<img width="1145" height="442" alt="image" src="https://github.com/user-attachments/assets/09ba6dde-483a-4a7d-b711-4c4d3196389c" />

**Table Input:** Чтение данных из PostgreSQL (1 000 000 строк).
<img width="995" height="706" alt="image" src="https://github.com/user-attachments/assets/f1c7451a-8bdd-4e4b-a852-c19c9f1e3744" />

**Table Input:** Чтение данных из MySQL (по 100 000 строк).
<img width="978" height="707" alt="image" src="https://github.com/user-attachments/assets/e925eb79-4bd1-4307-9c0d-f7a297acc514" />


**Excel Input:** Чтение данных из файлов (по 100 000 строк).
<img width="1200" height="761" alt="image" src="https://github.com/user-attachments/assets/e4e65777-7cf9-4dbb-9fc5-b08d37c5503b" />



**Stream Lookup:** Обогащение данных из базы данных данными из файлов (присоединение по `product_id`).
<img width="1669" height="564" alt="image" src="https://github.com/user-attachments/assets/73e79646-937e-4084-89f0-64c6f50d8378" />

**Filter Rows:** Фильтрация данных по бизнесс-логике: `store_balance < 50`. Отсеивает около 900 000 строк без критической необходимости в проверке поставок, тем самым защищая MySQL от перегрузки.
<img width="745" height="391" alt="image" src="https://github.com/user-attachments/assets/1c2daf6e-395a-4cf7-b26c-b9aaac4f5440" />

**Calculator:** Расчет расхождения (`discrepancy` = `store_balance` + `warehouse_balance` - `delivery_qty`).
<img width="1544" height="465" alt="image" src="https://github.com/user-attachments/assets/4513cd2d-f9e6-451d-864c-2f45235d7635" />

**Table Output:** Сохранение оставшихся ~100 000 итоговых строк в таблицу `inventory_analysis` в MySQL.
<img width="942" height="882" alt="image" src="https://github.com/user-attachments/assets/1cf90296-d87e-4c45-b198-ffc9996dee6c" />

---
## 4. Создание JOB и запуск:

<img width="1173" height="550" alt="image" src="https://github.com/user-attachments/assets/53993db2-5076-4de9-bb01-131cb81ec1ff" />


## 5. Бизнес-уровень (Витрина данных)
В MySQL (на основе загруженной таблицы) создается View `view_analytics_report`, которое группирует данные по категориям и показывает общие суммы остатков, поставок, а также общее отклонение и количество проблемных записей.

БД в MYSQL:
<img width="1544" height="902" alt="image" src="https://github.com/user-attachments/assets/ba231a23-4a94-4f0c-886e-61fa70b8da93" />


Создание VIEW:
```sql
CREATE OR REPLACE VIEW view_analytics_report AS
SELECT 
    category,
    SUM(store_balance) AS total_store_balance,
    SUM(warehouse_balance) AS total_warehouse_balance,
    SUM(delivery_qty) AS total_delivered,
    SUM(discrepancy) AS total_discrepancy,
    COUNT(*) AS records_analyzed_count
FROM inventory_analysis
GROUP BY category;
```
<img width="1526" height="895" alt="image" src="https://github.com/user-attachments/assets/ed12bda4-74cd-47f2-8c7d-a71bf5677b01" />

Запрос к VIEW:
Категории с наибольшим абсолютным расхождением (discrepancy)
```sql
SELECT 
    category,
    total_discrepancy,
    total_delivered,
    ROUND(total_discrepancy / NULLIF(total_delivered, 0) * 100, 2) AS discrepancy_pct,
    records_analyzed_count
FROM view_analytics_report
WHERE total_delivered > 0
ORDER BY total_discrepancy DESC;
```
<img width="978" height="474" alt="image" src="https://github.com/user-attachments/assets/673467c5-bfe6-499c-a41a-48d721d7418e" />


## 6. Вывод
В ходе работы была спроектирована и реализована ETL-архитектура, объединяющая данные из трех различных источников (СУБД и файлы). Была учтена проблема большого объема данных (1 млн записей) путем введения этапа фильтрации в Pentaho, что позволило выделить наиболее приоритетные данные для аналитики и избежать лишней нагрузки на хранилище DWH. Созданы аналитические Business-витрины для пользователей продукта.

## 🔗 Доступ к коду проекта

Все скрипты, ноутбуки и результаты выполнения данной лабораторной работы доступны в репозитории GitHub по следующей ссылке:

👉 [**Перейти в папку проекта lab_03 на GitHub**](https://github.com/sashaarlinskaya/ETL/tree/main/lab_03)
