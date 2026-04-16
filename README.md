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
INSERT INTO products (name, category, price, stock_quantity, supplier_id)
SELECT
    -- Название товара (без псевдонима)
    initcap((array['Ноутбук','Смартфон','Планшет','Монитор','Клавиатура','Мышь','Наушники','Колонки','Принтер','Сканер','Телевизор','Холодильник','Микроволновка','Чайник','Пылесос','Фен','Утюг','Стул','Стол','Диван','Кровать','Шкаф','Лампа','Часы','Сумка','Футболка','Джинсы','Кроссовки','Пальто','Шарф'])[floor(random()*30)+1]) 
    || ' ' || (floor(random()*900)+100)::text,
    
    -- Категория
    (array['Электроника','Бытовая техника','Мебель','Одежда','Аксессуары','Спорт','Игрушки','Книги','Продукты','Косметика'])[floor(random()*10)+1],
    
    -- Цена
    round((random() * 49900 + 100)::numeric, 2),
    
    -- Остаток
    floor(random() * 501)::int,
    
    -- Поставщик
    floor(random() * 100 + 1)::int
    
FROM generate_series(1, 5000);

```
