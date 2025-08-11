# Домашнее задание

Работа с уровнями изоляции транзакции в PostgreSQL

# Цель

научиться управлять уровнем изоляции транзации в PostgreSQL и понимать особенность работы уровней read commited и repeatable read;

# Описание/Пошаговая инструкция выполнения домашнего задания

- создать новый проект в Яндекс облако или на любых ВМ, докере
- далее создать инстанс виртуальной машины с дефолтными параметрами
- добавить свой ssh ключ в metadata ВМ
- зайти удаленным ssh (первая сессия), не забывайте про ssh-add
- поставить PostgreSQL
- зайти вторым ssh (вторая сессия)
- запустить везде psql из под пользователя postgres
- выключить auto commit
- сделать
  - в первой сессии новую таблицу и наполнить ее данными
  ```sql
  create table persons(id serial, first_name text, second_name text);
  insert into persons(first_name, second_name) values('ivan', 'ivanov');
  insert into persons(first_name, second_name) values('petr', 'petrov'); commit;
  ```
  - посмотреть текущий уровень изоляции:
  ```sql
  show transaction isolation level
  ```
  - начать новую транзакцию в обоих сессиях с дефолтным (не меняя) уровнем изоляции
  - в первой сессии добавить новую запись
  ```sql
  insert into persons(first_name, second_name) values('sergey', 'sergeev');
  ```
  - сделать select from persons во второй сессии
  - видите ли вы новую запись и если да то почему?
  - завершить первую транзакцию - commit;
  - сделать select from persons во второй сессии
  - видите ли вы новую запись и если да то почему?
  - завершите транзакцию во второй сессии
  - начать новые но уже repeatable read транзации
  ```sql
  set transaction isolation level repeatable read;
  ```
  - в первой сессии добавить новую запись
  ```sql
  insert into persons(first_name, second_name) values('sveta', 'svetova');
  ```
  - сделать во второй сессии
  ```sql
  select * from persons
  ```
  - видите ли вы новую запись и если да то почему?
  - завершить первую транзакцию - commit;
  - сделать select from persons во второй сессии
  - видите ли вы новую запись и если да то почему?
  - завершить вторую транзакцию
  - сделать во второй сессии
  ```sql
  select * from persons
  ```
  - видите ли вы новую запись и если да то почему?

# Адрес проекта

<https://github.com/nvv2020/otus_pg>

# Решение

Решение расположено в файле answer.md.