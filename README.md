# ETL
## Лабораторная работа 1. Изучение методов хранения данных на основе NoSQL
*  ([Отчет 1](https://github.com/sashaarlinskaya/ETL/blob/main/lw1.md))
## Лабораторная работа 2. Динамические соединения с базами данных
*  ([Отчет 2](https://github.com/sashaarlinskaya/ETL/blob/main/lw2.md))
## Лабораторная работа 3. Интеграция данных из нескольких источников. Обработка и согласование данных из разных источников
*  ([Отчет 3](https://github.com/sashaarlinskaya/ETL/blob/main/lab_03/lab_03.md))
## Лабораторная работа 4. Анализ данных с помощью Dask и визуализация графов (DAG)
*  ([Отчет 4](https://github.com/sashaarlinskaya/ETL/blob/main/lab_04/lab_04.md))
*  ([Коллаб_ссылка на блокнот](https://colab.research.google.com/drive/1Zqctus8hUvO7uLWD9P57veDkvpqje1K5#scrollTo=U_wfL4uVAMph))


https://docs.google.com/document/d/1sckpqLsWptZ-25oGAJXBD-xJqaYDNZH0GKKzlGs2o-w/edit?tab=t.0

```
-- Генерация 5000 товаров
INSERT INTO products (name, category, price, stock_quantity, supplier_id)
SELECT
    -- Название товара: случайное слово + число
    initcap((array['Ноутбук','Смартфон','Планшет','Монитор','Клавиатура','Мышь','Наушники','Колонки','Принтер','Сканер','Телевизор','Холодильник','Микроволновка','Чайник','Пылесос','Фен','Утюг','Стул','Стол','Диван','Кровать','Шкаф','Лампа','Часы','Сумка','Футболка','Джинсы','Кроссовки','Пальто','Шарф'])[floor(random()*30)+1] 
    || ' ' || (floor(random()*900)+100)::text AS name,
    
    -- Категория
    (array['Электроника','Бытовая техника','Мебель','Одежда','Аксессуары','Спорт','Игрушки','Книги','Продукты','Косметика'])[floor(random()*10)+1] AS category,
    
    -- Цена от 100 до 50000 рублей с двумя десятичными знаками
    round((random() * 49900 + 100)::numeric, 2) AS price,
    
    -- Остаток на складе от 0 до 500
    floor(random() * 501)::int AS stock_quantity,
    
    -- supplier_id от 1 до 100 (пусть будет 100 поставщиков)
    floor(random() * 100 + 1)::int AS supplier_id
    
FROM generate_series(1, 5000);

```
