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

Bonus Task 1: Repeatable Read (REPEATABLE READ):

Транзакція 1:

SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;

BEGIN;
UPDATE accounts SET balance = 0 WHERE id = 1;
COMMIT;
END;

Транзакція 2:

SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;

BEGIN;
SELECT * FROM accounts WHERE id = 1;

Починаємо транзакцію 2, резульата - баланс = 1000. Починаємо транзакцію 1, змінюємо баланс на 0, комітимо зміни, але не завершуємо транзакцію. Якщо транзакція 2 ще раз прочитає ці дані, то баланс залишиться незмінним (1000) поки транзакція 1 не завершиться.

![image](https://github.com/user-attachments/assets/35b53609-ffc1-4017-ad41-96ab0013f4c3)

![image](https://github.com/user-attachments/assets/87ca513e-1083-4318-805a-962030dddebf)

Bonus Task 3: Deadlock:

INSERT INTO accounts VALUES (2, 'Mike', 10);

Транзакція 1:

START TRANSACTION;
UPDATE accounts SET balance = 50 WHERE id = 2;
UPDATE accounts SET balance = 150 WHERE id = 1;
COMMIT;

Транзакція 2:

START TRANSACTION;
UPDATE accounts SET balance = 50 WHERE id = 1;
UPDATE accounts SET balance = 150 WHERE id = 2;
COMMIT;

Починаємо транзакцію 1, змінюємо баланс на акаунті з id = 2, починаємо транзакцію 2, змінюємо баланс, де id = 2, намагаємося змінити першою транзакцією баланс, де id = 1, але ці дані заблоковані транзакцією 2 і отримуємо помилку:

![image](https://github.com/user-attachments/assets/693c81ca-b17f-417c-826c-4c24bb25cfed)










