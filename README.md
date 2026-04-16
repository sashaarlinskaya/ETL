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
INSERT INTO customers (first_name, last_name, email, phone, registration_date)
SELECT
    first_name,
    last_name,
    -- уникальный email: имя.фамилия + случайное число + домен
    lower(first_name) || '.' || lower(last_name) || floor(random() * 10000)::text || '@' || 
    (array['gmail.com', 'yandex.ru', 'mail.ru', 'example.com', 'bk.ru'])[floor(random()*5)+1] AS email,
    -- телефон в формате +7 XXX XXX-XX-XX
    '+7 ' || (100 + floor(random()*900))::text || ' ' ||
    (100 + floor(random()*900))::text || '-' ||
    (10 + floor(random()*90))::text || '-' ||
    (10 + floor(random()*90))::text AS phone,
    -- дата регистрации за последние 3 года
    CURRENT_DATE - (random() * 1095)::int * interval '1 day' AS registration_date
FROM (
    SELECT 
        (array['Александр','Алексей','Андрей','Артём','Владимир','Дмитрий','Евгений','Иван','Максим','Михаил','Николай','Павел','Роман','Сергей','Юрий',
                'Анна','Елена','Мария','Ольга','Татьяна','Наталья','Ирина','Светлана','Екатерина','Юлия']) [floor(random()*25)+1] AS first_name,
        (array['Иванов','Смирнов','Кузнецов','Попов','Васильев','Петров','Соколов','Михайлов','Новиков','Фёдоров','Морозов','Волков','Алексеев','Лебедев','Семёнов',
                'Козлов','Михайлова','Новикова','Морозова','Волкова','Алексеева','Лебедева','Семёнова','Егорова','Павлова']) [floor(random()*25)+1] AS last_name
    FROM generate_series(1, 5000)
) AS names
ON CONFLICT (email) DO NOTHING;  -- на случай редкого совпадения email
```
