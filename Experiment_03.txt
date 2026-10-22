CREATE DATABASE IF NOT EXISTS BankDB;

USE BankDB;

CREATE TABLE BankAccount (
Account_ID INT PRIMARY KEY,
Holder_Name VARCHAR(50) NOT NULL,
Branch VARCHAR(30) NOT NULL,
Balance DECIMAL(12,2) NOT NULL CHECK (Balance >= 0),
Account_Type VARCHAR(20) NOT NULL
) ENGINE=InnoDB;

INSERT INTO BankAccount VALUES
(201,'Anand','Chennai',50000,'Savings'),
(202,'Bhavya','Hyderabad',35000,'Savings'),
(203,'Charan','Vijayawada',20000,'Current'),
(204,'Deepa','Chennai',45000,'Savings'),
(205,'Eshan','Bengaluru',60000,'Current');

SELECT Account_ID, Holder_Name, Balance
FROM BankAccount ORDER BY Account_ID;

CREATE USER 'lab_teller3'@'localhost'
IDENTIFIED BY 'LabDemo_ChangeMe_2026!';

GRANT SELECT ON BankDB.BankAccount
TO 'lab_teller3'@'localhost';

SHOW GRANTS FOR 'lab_teller3'@'localhost';

SELECT Account_ID, Balance
FROM BankDB.BankAccount;

UPDATE BankDB.BankAccount
SET Balance = Balance + 100
WHERE Account_ID = 201;

REVOKE SELECT ON BankDB.BankAccount
FROM 'lab_teller3'@'localhost';

SHOW GRANTS FOR 'lab_teller3'@'localhost';

START TRANSACTION;

UPDATE BankAccount
SET Balance = Balance + 5000
WHERE Account_ID = 201;

COMMIT;

SELECT Account_ID, Balance FROM BankAccount
WHERE Account_ID = 201;

START TRANSACTION;

UPDATE BankAccount
SET Balance = Balance - 3000
WHERE Account_ID = 202;

ROLLBACK;

SELECT Account_ID, Balance FROM BankAccount
WHERE Account_ID = 202;

START TRANSACTION;

UPDATE BankAccount SET Balance = Balance + 1000
WHERE Account_ID = 203;

SAVEPOINT after_first_change;

UPDATE BankAccount SET Balance = Balance + 2000
WHERE Account_ID = 204;

ROLLBACK TO SAVEPOINT after_first_change;

COMMIT;

SELECT Account_ID, Balance FROM BankAccount
WHERE Account_ID IN (203,204) ORDER BY Account_ID;

START TRANSACTION;

UPDATE BankAccount SET Balance = Balance - 4000
WHERE Account_ID = 201 AND Balance >= 4000;

UPDATE BankAccount SET Balance = Balance + 4000
WHERE Account_ID = 202;

COMMIT;

SELECT Account_ID, Balance FROM BankAccount
WHERE Account_ID IN (201,202) ORDER BY Account_ID;

START TRANSACTION;

SAVEPOINT test_point;

RELEASE SAVEPOINT test_point;

ROLLBACK;

DROP USER 'lab_teller3'@'localhost';

SELECT Account_ID, Balance FROM BankAccount ORDER BY Account_ID;

SELECT SUM(Balance) AS Total_Balance FROM BankAccount;
