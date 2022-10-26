## Postgre
## work_1
## выключить auto commit
сделать в первой сессии новую таблицу и наполнить ее данными 
 * create table persons(id serial, first_name text, second_name text); 
 * insert into persons(first_name, second_name) values('ivan', 'ivanov'); 
 * insert into persons(first_name, second_name) values('petr', 'petrov'); 
 * commit;

посмотреть текущий уровень изоляции: show transaction isolation level

**-> read committed**

## начать новую транзакцию в обоих сессиях с дефолтным (не меняя) уровнем изоляции
в первой сессии добавить новую запись insert into persons(first_name, second_name) values('sergey', 'sergeev');
сделать select * from persons во второй сессии
видите ли вы новую запись и если да то почему?
**-> нет не вижу так как первая транзакция не завершена**
завершить первую транзакцию - commit;
сделать select * from persons во второй сессии
видите ли вы новую запись и если да то почему?
**-> да вижу, так как транзакция завершена**
завершите транзакцию во второй сессии

##начать новые но уже repeatable read транзации - set transaction isolation level repeatable read;
в первой сессии добавить новую запись insert into persons(first_name, second_name) values('sveta', 'svetova');
сделать select * from persons во второй сессии
видите ли вы новую запись и если да то почему?
**-> нет не вижу так как первая транзакция не завершена**
завершить первую транзакцию - commit;
сделать select * from persons во второй сессии
видите ли вы новую запись и если да то почему?
**-> нет не вижу, так как я смотрю данные на момент начала транзакции**
завершить вторую транзакцию
сделать select * from persons во второй сессии
видите ли вы новую запись и если да то почему?
**-> да вижу, так как предыдущий коммит завершил транзакцию и мы смотрим данные с нового отрезка времени**
