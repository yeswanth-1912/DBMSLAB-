CREATE DATABASE UniversityDB;

USE UniversityDB;

CREATE TABLE Student (
Student_ID INT PRIMARY KEY,
Student_Name VARCHAR(50) NOT NULL,
Department VARCHAR(20) NOT NULL,
Year INT NOT NULL, Age INT, Gender VARCHAR(10),
Marks DECIMAL(5,2), City VARCHAR(30)
);

DESC Student;

INSERT INTO Student
(Student_ID, Student_Name, Department, Year, Age, Gender, Marks, City)
VALUES
(101, 'Arun', 'CSE', 2, 19, 'Male', 85, 'Chennai'),
(102, 'Priya', 'ECE', 3, 20, 'Female', 92, 'Hyderabad'),
(103, 'Ravi', 'CSE', 2, 19, 'Male', 67, 'Vijayawada'),
(104, 'Sneha', 'AIML', 1, 18, 'Female', 88, 'Bengaluru'),
(105, 'Kiran', 'EEE', 4, 21, 'Male', 56, 'Chennai'),
(106, 'Divya', 'CSE', 3, 20, 'Female', 94, 'Tirupati'),
(107, 'Rahul', 'MECH', 2, 19, 'Male', 72, 'Nellore'),
(108, 'Anjali', 'AIML', 1, 18, 'Female', 81, 'Hyderabad'),
(109, 'Naveen', 'ECE', 4, 21, 'Male', 63, 'Guntur'),
(110, 'Kavya', 'CSE', 3, 20, 'Female', 90, 'Chennai');

SELECT * FROM Student ORDER BY Student_ID;

SELECT Student_ID, Student_Name, Department FROM Student ORDER BY Student_ID;

SELECT Student_ID, Student_Name, Department FROM Student WHERE Department='CSE' ORDER BY
Student_ID;

SELECT Student_Name, Department, Marks FROM Student WHERE Department='CSE' AND Marks>80 ORDER
BY Student_ID;

SELECT Student_Name, Department FROM Student WHERE Department='AIML' OR Department='ECE' ORDER
BY Student_ID;

SELECT DISTINCT Department FROM Student ORDER BY Department;

SELECT Student_Name, Marks FROM Student ORDER BY Marks ASC;

SELECT Student_Name, Marks FROM Student ORDER BY Marks DESC;

SELECT Student_Name, Marks FROM Student WHERE Marks BETWEEN 70 AND 90 ORDER BY Marks ASC;

SELECT Student_ID, Student_Name FROM Student WHERE Student_Name LIKE 'A%' ORDER BY Student_ID;

SELECT Student_Name, Department FROM Student WHERE Department IN ('CSE','ECE','AIML') ORDER BY
Student_ID;

SELECT Student_ID, Student_Name, City FROM Student WHERE City IS NULL;

SELECT Student_Name, Department, Marks FROM Student ORDER BY Marks DESC, Student_ID ASC LIMIT
3;

SELECT COUNT(*) AS Total FROM Student;

SELECT COUNT(DISTINCT Department) AS Departments FROM Student;

SELECT MAX(Marks) AS Highest, MIN(Marks) AS Lowest FROM Student;
