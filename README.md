# hive_practice
Задание на знакомство с СУБД Hive в рамках курса "Инженер данных"

## Из чего состоит практическая работа:
1. Python скрипт для обработки сырых данных с помощью библиотеки pandas: <code>[transform_data.py](https://github.com/AlexeyAnanchenko/hive_practice/blob/main/transform_data.py)</code>.

2. Сырые данные, которые необходимо будет обработать этим скриптом: <code>[csv_files/raw/](https://github.com/AlexeyAnanchenko/hive_practice/blob/main/csv_files/raw)</code>.

3. Запросы к БД на HiveQL для создания партиционированных и бакетированнх таблиц из csv файлов на HDFS: <code>[query.sql](https://github.com/AlexeyAnanchenko/hive_practice/blob/main/query.sql)</code>.

4. План запроса по созданном на основе csv файлов итоговой витрине: <code>[query_plan.pdf](https://github.com/AlexeyAnanchenko/hive_practice/blob/main/query_plan.pdf)</code>.

### Как запустить?

1. Для начала надо скачать docker-compose файл для запуска приложений экосистемы hadoop. Ниже ссылка на репозиторий:
```sh
git clone git@github.com:tech4242/docker-hadoop-hive-parquet.git
```

2. Запускаем docker-compose из этого репозитория:
```sh
cd docker-hadoop-hive-parquet/ && docker-compose up -d && cd ..
```

3. Далее запускаем скрипт по обработке сырых csv-файлов:
```sh
python transform_data.py
```

4. Копируем отработанные файлы datanode:
```sh
docker cp csv_files/processed/ docker-hadoop-hive-parquet_datanode_1:csv_files/
```

5. Заходим в контейнер в datanode и запускаем bash:
```sh
docker exec -it docker-hadoop-hive-parquet_datanode_1 bash
```

6. Предварительно заходим по адресу http://localhost:8888/ в приложение Hue и создаём пользователя
 
7. Возвращаемся в терминал и копируем данные из datanode в hdfs ("hadoop" - это имя созданног Вами user-а):
```sh
hadoop fs -copyFromLocal ./csv_files/ /user/hadoop/
```

8. Теперь в Hue можно последовательно запускать запросы из файла query.sql в приложении Hive