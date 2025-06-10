# IDT-Assignment3

Task 1: Dirty Read (READ UNCOMMITTED):

Транзакція 1:

CREATE DATABASE assign3;
USE assign3;

CREATE TABLE accounts (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    balance INT
);

INSERT INTO accounts VALUES (1, 'Alice', 1000);

SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

BEGIN;
UPDATE accounts SET balance = 0 WHERE id = 1;
ROLLBACK;

Транзакція 2:
USE assign3;

SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

BEGIN;
SELECT * FROM accounts WHERE id = 1;

Запускаємо першу, вона змінює баланс, запускаємо другу вона читає, що баланс 0, потім rollback першу  і друга знову читає і баланс вже 1000. Друга транзакція читає різні дані одним запитом, ще не закомічені, тобто можливо не достовірні, тому це dirty read.

![image](https://github.com/user-attachments/assets/3bc9e563-7486-4907-98de-3f5a1912f240)

![image](https://github.com/user-attachments/assets/16d20a8d-66c3-4590-a0fc-536b90255032)




