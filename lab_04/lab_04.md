# Отчет по лабораторной работе №4
## Анализ данных с помощью Dask и визуализация графов (DAG)

**Студент:** Александра Арлинская  
**Вариант:** 3   


**Цель работы:** Получение практических навыков работы с библиотекой Dask для построения базовых ETL-конвейеров при обработке больших массивов данных, не помещающихся в оперативную память, изучение принципов «ленивых вычислений», управления памятью и визуализации ориентированных ациклических графов (DAG).

---

## Шаг 1. Подготовка окружения

Перед началом работы была установлена библиотека `dask` с полным набором зависимостей и `graphviz` для визуализации графов.

```python
!pip install "dask[complete]" graphviz
```

Далее был инициализирован локальный кластер Dask с оптимальными параметрами: 2 воркера, 2 потока на воркер, использование процессов для изоляции памяти.

```python
import dask.dataframe as dd
from dask.distributed import Client
from dask.diagnostics import ProgressBar

client = Client(n_workers=2, threads_per_worker=2, processes=True)
client
```

**Результат:** 

<img width="1346" height="776" alt="image" src="https://github.com/user-attachments/assets/eb587ea3-0211-4d56-8e93-24684f1a2409" />


---


## Шаг 2. Extract (Извлечение данных)

Для выполнения работы был выбран датасет **Parking_Violations_Issued_-_Fiscal_Year_2016.csv** (вариант 3).

Чтобы избежать автоматического определения типов, для всех столбцов был явно указан тип `object`.

```python
dtypes = {
    'Summons Number': 'object', 'Plate ID': 'object', 'Issue Date': 'object',
    'Violation Time': 'object', 'Violation Code': 'object', 'Vehicle Make': 'object',
    'Vehicle Color': 'object', 'Vehicle Body Type': 'object', 'Street Name': 'object',
    'Violation Precinct': 'object', 'Vehicle Year': 'object', 'Issuer Precinct': 'object',
    'House Number': 'object', 'Intersecting Street': 'object', 'Issuer Squad': 'object',
    'Time First Observed': 'object', 'Unregistered Vehicle?': 'object',
    'Violation Description': 'object', 'Violation Legal Code': 'object',
    'Violation Post Code': 'object', 'Date First Observed': 'object',
    'Feet From Curb': 'object', 'Issuer Code': 'object', 'Law Section': 'object',
    'Vehicle Expiration Date': 'object'
}

df = dd.read_csv('Parking_Violations_Issued_-_Fiscal_Year_2016.csv', dtype=dtypes)
df
```

**Результат:** 

<img width="701" height="551" alt="image" src="https://github.com/user-attachments/assets/e7685544-85a5-4273-a39c-b18eebf97e99" />

<img width="1372" height="628" alt="image" src="https://github.com/user-attachments/assets/ae794429-a917-436e-9575-f3038a54ea46" />

### 2.1. Transform (Трансформация и очистка)

Анализ качества данных и подсчет пропущенных значений:

```python
missing_values = df.isnull().sum()
mysize = df.index.size
missing_count = ((missing_values / mysize) * 100)

with ProgressBar():
    missing_count_percent = missing_count.compute()

print(missing_count_percent.sort_values(ascending=False).head(15))
```


```python
columns_to_drop = list(missing_count_percent[missing_count_percent > 55].index)
df_dropped = df.drop(columns=columns_to_drop)
additional_columns = ['Street Code1', 'Street Code2', 'Street Code3', 'Issuer Code', 
                     'Feet From Curb', 'Violation Post Code']
df_final = df_dropped.drop(columns=[c for c in additional_columns if c in df_dropped.columns])

df_final['Issue Date'] = dd.to_datetime(df_final['Issue Date'], errors='coerce')
```
**Результат:** 

<img width="1173" height="404" alt="image" src="https://github.com/user-attachments/assets/ca20b13f-e1e9-4962-887f-4bf3befe6af2" />

<img width="1542" height="565" alt="image" src="https://github.com/user-attachments/assets/6879c2aa-0ccc-49b3-bc78-3ac2a7b29860" />


## Шаг 3. Загрузка (сохранение результатов пайплайна)

```python
df_final.to_csv('CLEAR_Parking_Violations_Issued_2016.csv', single_file=True, index=False)
```

**Итог ETL:** Создан очищенный датасет без "тяжелых" столбцов.

<img width="1463" height="488" alt="image" src="https://github.com/user-attachments/assets/11963114-46ad-487c-a511-6758a3596b68" />

---

## Шаг 4. Визуализация направленных ациклических графов (DAG)

### 4.1. Простой DAG - Анализ среднего количества нарушений на участок

**Что анализирует:**  
Среднее количество нарушений правил парковки, приходящееся на один полицейский участок (precinct).

**Логика работы (три этапа):**

### Этап 1: `get_total_violations`
- **Действие:** Подсчитывает общее количество записей (строк) в очищенном датасете `df_final`
- **Что дает:** Общее число выписанных штрафов/нарушений

### Этап 2: `get_unique_precincts`
- **Действие:** Находит уникальные значения в колонке `Violation Precinct` (номер участка, где произошло нарушение)
- **Что дает:** Количество различных полицейских участков, которые фигурируют в данных

### Этап 3: `avg_violations_per_precinct`
- **Действие:** Делит общее количество нарушений на количество уникальных участков
- **Что дает:** Среднюю арифметическую нагрузку на один участок


```python
from dask import delayed
from IPython.display import Image

def get_total_violations():
    return len(df_final)

def get_unique_precincts():
    return df_final['Violation Precinct'].nunique().compute()

def avg_violations_per_precinct(total, unique):
    return round(total / unique, 2)

x = delayed(get_total_violations)()
y = delayed(get_unique_precincts)()
z = delayed(avg_violations_per_precinct)(x, y)

z.visualize(filename='simple_precinct_analysis.png')
display(Image('simple_precinct_analysis.png'))
print(f"Результат: {z.compute()}")
```

**Результат:** 

<img width="478" height="711" alt="image" src="https://github.com/user-attachments/assets/3c08b49e-1372-4e05-af39-d3d1229e59a2" />

<img width="327" height="111" alt="image" src="https://github.com/user-attachments/assets/dedb99b2-b300-4b03-aea0-c6979570656c" />


### 4.2. Сложный многоуровневый DAG.


**Что анализирует:** Процент нарушений в "часы пик" (8-10 утра) для каждого цвета автомобилей.

**Структура:**

**Layer 1 (Map):** Разделение по 8 цветам

**Layer 2:** Общее количество нарушений

**Layer 3:** Пиковые нарушения (08-10 утра)

**Layer 4 (Reduce):** Процент пиковых нарушений

```python
colors = ['BLACK', 'WHITE', 'SILVER', 'GRAY', 'RED', 'BLUE', 'GREEN', 'BROWN']

def get_color_data(color):
    return df_final[df_final['Vehicle Color'] == color]

def count_violations(color_data):
    return len(color_data) if color_data is not None else 0

def count_peak_hours(color_data):
    hours = color_data['Violation Time'].astype(str).str[:2]
    return len(hours[hours.isin(['08', '09', '10'])])

def calculate_peak_percentage(total, peak):
    return round((peak / total) * 100, 2) if total > 0 else 0

layer1 = [delayed(get_color_data)(c) for c in colors]
layer2 = [delayed(count_violations)(d) for d in layer1]
layer3 = [delayed(count_peak_hours)(d) for d in layer1]
layer4 = [delayed(calculate_peak_percentage)(t, p) for t, p in zip(layer2, layer3)]
results = delayed(list)(layer4)

results.visualize(filename='complex_color_analysis.png')
display(Image('complex_color_analysis.png'))
```

**Результаты:**

<img width="1739" height="620" alt="image" src="https://github.com/user-attachments/assets/db8333eb-0cf2-4147-8fac-a2ae0fefdbbc" />


## Шаг 5. Аналитика

---


