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

Запускаємо першу, вона змінює баланс, запускаємо другу вона читає, що баланс 0, потім rollback першу  і друга знову читає і баланс вже 1000. Друга транзакція читає різні дані одним запитом, ще не закомічені, тобто можливо не достовірні, тому це dirty read. Рівень ізоляції read uncommitted це дозволяє. Тому при цьому рівні ізоляції паралельні транзакції можуть отримувати некоректні дані, тому що ці дані ще не закомічені.

![image](https://github.com/user-attachments/assets/3bc9e563-7486-4907-98de-3f5a1912f240)

![image](https://github.com/user-attachments/assets/16d20a8d-66c3-4590-a0fc-536b90255032)

Task 2: Non-Repeatable Read (READ COMMITTED):

Транзакція 1:

SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;

BEGIN;
UPDATE accounts SET balance = 0 WHERE id = 1;
COMMIT;

Транзакція 2:

SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;

BEGIN;
SELECT * FROM accounts WHERE id = 1;

Запускаємо другу, вона читає баланс = 1000, запускається перша, змінює баланс і комітить результат. Друга знову читає знову і має інший результат. Це read committed, транзакція може отримати різні результати, якщо під час її робати якась інша транзакція змінила дані і закомітила результат. 

![image](https://github.com/user-attachments/assets/98b4d0aa-d58b-4cb4-9526-b4e4ac8a30df)

![image](https://github.com/user-attachments/assets/65654955-a028-4f62-b1c0-69855864ce14)







