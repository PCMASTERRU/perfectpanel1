Решение тестовых заданий для Перфектных Панелей.

Задание 1
Mysql

Имеется три таблицы:
users
id first_name last_name birthday
1 Ivan Ivanov 2005-01-01
2 Marina Ivanova 2011-03-01

books
id name author
1 Romeo and Juliet William Shakespeare
2 War and Peace Leo Tolstoy

user_books
id user_id book_id get_date return_date
1 1 2 2022-01-01 2022-02-01
2 2 1 2021-01-01 2022-01-01

Необходимо написать запрос выборки данных из представленных таблиц, который найдет и выведет всех посетителей библиотеки, возраст которых попадает в диапазон от 7 и до 17 лет, которые  взяли две книги одного автора (взяли всего 2 книги и они одного автора), книги были у них в руках не более двух календарных недель (не просрочили 2-х недельный срок пользования).
Формат вывода:
ID, Name (first_name  last_name), Author, Books (Book 1, Book 2, ...) 
1; Ivan Ivanov; Leo Tolstoy; Book 1, Book 2

Решение.

Выберем пользователей с возрастом с 7 до 17 лет.

SELECT * FROM users
WHERE DATE_ADD(birthday, INTERVAL 7 YEAR) <= CURDATE()
AND DATE_ADD(birthday, INTERVAL 17 YEAR) >= CURDATE();

Добавим посетителей, которые взяли две книги одного автора.

SELECT * FROM users u
WHERE DATE_ADD(u.birthday, INTERVAL 7 YEAR) <= CURDATE()
AND DATE_ADD(u.birthday, INTERVAL 17 YEAR) >= CURDATE()
AND (
    SELECT COUNT(DISTINCT ub.book_id)
    FROM user_books ub
    WHERE ub.user_id = u.id
) == 2;

Или добавим посетителей, которые взяли всего две книги.

SELECT * FROM users u
WHERE DATE_ADD(u.birthday, INTERVAL 7 YEAR) <= CURDATE()
AND DATE_ADD(u.birthday, INTERVAL 17 YEAR) >= CURDATE()
AND (
    SELECT COUNT(*)
    FROM user_books ub
    WHERE ub.user_id = u.id
) == 2;

