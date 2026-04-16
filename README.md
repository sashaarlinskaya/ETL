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
Шаг 1. Установка Java и зависимостей
sudo apt update
sudo apt install openjdk-11-jdk -y
java -version
Шаг 2. Исправление для библиотеки WebKitGTK
Так как PDI использует старую библиотеку libwebkitgtk, отсутствующую в репозиториях Ubuntu 22.04, необходимо добавить репозиторий bionic.

Откройте список источников:
sudo nano /etc/apt/sources.list
Добавьте строку в конец файла:
deb http://cz.archive.ubuntu.com/ubuntu bionic main universe
(Или: deb http://mirrors.kernel.org/ubuntu bionic main universe)
Сохраните (Ctrl+X, Y, Enter) и выполните команды:
sudo apt update
# Добавляем ключи, если ругается на подписи (код ключа взять из ошибки терминала)
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3B4FE6ACC0B21F32 
sudo apt update
sudo apt install libwebkitgtk-1.0-0 -y
Шаг 3. Установка драйвера MySQL (Важно!)
Для подключения Pentaho к MySQL 8+ требуется драйвер mysql-connector-j.

Скачайте .deb пакет драйвера (Platform Independent) с официального сайта или используйте команду:
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j_9.2.0-1ubuntu22.04_all.deb
Установите пакет и найдите .jar файл:
sudo dpkg -i mysql-connector-j_*.deb
ls /usr/share/java/mysql-connector-j-*.jar
Скопируйте драйвер в папку библиотек Pentaho (data-integration/lib):
# Путь может отличаться в зависимости от того, куда вы распаковали PDI
cp /usr/share/java/mysql-connector-j-9.2.0.jar ~/Downloads/data-integration/lib/
Настройте права (критично для запуска):
chmod 644 ~/Downloads/data-integration/lib/mysql-connector-j-9.2.0.jar
# Смените владельца на вашего пользователя (например, user или dba)
sudo chown $USER:$USER ~/Downloads/data-integration/lib/mysql-connector-j-9.2.0.jar
Шаг 4. Запуск Pentaho Spoon
cd ~/Downloads/data-integration/
chmod +x spoon.sh
./spoon.sh
```
