CREATE DATABASE IF NOT EXISTS LibraryExp5;

USE LibraryExp5;

CREATE TABLE Books (
book_id INT PRIMARY KEY, title VARCHAR(80) NOT NULL,
category VARCHAR(30) NOT NULL, price DECIMAL(8,2) NOT NULL,
copies INT NOT NULL, CHECK(price >= 0), CHECK(copies >= 0)
);

CREATE TABLE Members (
member_id INT PRIMARY KEY, member_name VARCHAR(60) NOT NULL,
department VARCHAR(20) NOT NULL
);

CREATE TABLE Loans (
loan_id INT PRIMARY KEY, book_id INT NOT NULL,
member_id INT NOT NULL, loan_date DATE NOT NULL,
FOREIGN KEY(book_id) REFERENCES Books(book_id),
FOREIGN KEY(member_id) REFERENCES Members(member_id)
);

INSERT INTO Books VALUES
(1,'SQL Basics','Database',400,5),
(2,'Advanced SQL','Database',650,3),
(3,'Python Guide','Programming',550,4),
(4,'Java Handbook','Programming',700,2),
(5,'Data Science','Analytics',900,2),
(6,'Statistics 101','Analytics',450,6),
(7,'Web Design','Web',300,5),
(8,'Cloud Computing','Cloud',800,3);

INSERT INTO Members VALUES
(101,'Asha','CSE'),(102,'Bala','ECE'),
(103,'Charan','CSE'),(104,'Deepa','AIML'),
(105,'Esha','MECH');

INSERT INTO Loans VALUES
(201,1,101,'2026-09-01'),(202,3,101,'2026-09-02'),
(203,2,102,'2026-09-03'),(204,5,104,'2026-09-04'),
(205,4,103,'2026-09-05');

SELECT book_id,title FROM Books
WHERE category IN ('Database','Analytics')
ORDER BY book_id;

SELECT book_id,title,price FROM Books
WHERE price > ANY
(SELECT price FROM Books WHERE category='Database')
ORDER BY book_id;

SELECT book_id,title,price FROM Books
WHERE price > ALL
(SELECT price FROM Books WHERE category='Database')
ORDER BY book_id;

SELECT m.member_id,m.member_name FROM Members m
WHERE EXISTS (SELECT 1 FROM Loans l
WHERE l.member_id=m.member_id)
ORDER BY m.member_id;

SELECT m.member_id,m.member_name FROM Members m
WHERE NOT EXISTS (SELECT 1 FROM Loans l
WHERE l.member_id=m.member_id)
ORDER BY m.member_id;

SELECT book_id,title FROM Books
WHERE book_id IN (SELECT book_id FROM Loans)
ORDER BY book_id;

SELECT member_name FROM Members WHERE department='CSE'
UNION
SELECT member_name FROM Members WHERE department='AIML'
ORDER BY member_name;

SELECT member_id FROM Members WHERE department='CSE'
INTERSECT
SELECT member_id FROM Loans
ORDER BY member_id;

SELECT title FROM Books WHERE price < 500
UNION
SELECT title FROM Books WHERE price > 750
ORDER BY title;

SELECT book_id FROM Books WHERE category='Programming'
INTERSECT
SELECT book_id FROM Loans
ORDER BY book_id;

SELECT b.book_id,b.title FROM Books b
WHERE NOT EXISTS (SELECT 1 FROM Loans l
WHERE l.book_id=b.book_id)
ORDER BY b.book_id;

INSERT INTO Books VALUES
(9,'Invalid Price','Test',-50,2);

INSERT INTO Loans VALUES
(206,999,101,'2026-09-06');

SELECT (SELECT COUNT(*) FROM Books) AS books,
(SELECT COUNT(*) FROM Members) AS members,
(SELECT COUNT(*) FROM Loans) AS loans;
