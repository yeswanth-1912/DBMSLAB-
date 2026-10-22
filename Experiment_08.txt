CREATE DATABASE IF NOT EXISTS NormalizationLabDB;

USE NormalizationLabDB;

CREATE TABLE EnrollmentRaw (
StudentID INT, StudentName VARCHAR(40),
CourseList VARCHAR(100)
);

INSERT INTO EnrollmentRaw VALUES
(1,'Anu','C101,C102'),(2,'Bala','C101'),
(3,'Chitra','C102');

SELECT * FROM EnrollmentRaw ORDER BY StudentID;

CREATE TABLE Enrollment1NF (
StudentID INT, StudentName VARCHAR(40),
CourseID VARCHAR(8), CourseName VARCHAR(40),
InstructorID VARCHAR(8), InstructorName VARCHAR(40),
Grade CHAR(1), PRIMARY KEY(StudentID,CourseID)
);

INSERT INTO Enrollment1NF VALUES
(1,'Anu','C101','DBMS','I10','Rao','A'),
(1,'Anu','C102','Networks','I20','Sen','B'),
(2,'Bala','C101','DBMS','I10','Rao','B'),
(3,'Chitra','C102','Networks','I20','Sen','A');

SELECT StudentID,CourseID,Grade FROM Enrollment1NF
ORDER BY StudentID,CourseID;

CREATE TABLE Students2NF (
StudentID INT PRIMARY KEY, StudentName VARCHAR(40) NOT NULL
);

CREATE TABLE Courses2NF (
CourseID VARCHAR(8) PRIMARY KEY, CourseName VARCHAR(40),
InstructorID VARCHAR(8), InstructorName VARCHAR(40)
);

CREATE TABLE Registrations2NF (
StudentID INT, CourseID VARCHAR(8), Grade CHAR(1),
PRIMARY KEY(StudentID,CourseID),
FOREIGN KEY(StudentID) REFERENCES Students2NF(StudentID),
FOREIGN KEY(CourseID) REFERENCES Courses2NF(CourseID)
);

INSERT INTO Students2NF VALUES (1,'Anu'),(2,'Bala'),(3,'Chitra');

INSERT INTO Courses2NF VALUES
('C101','DBMS','I10','Rao'),('C102','Networks','I20','Sen');

INSERT INTO Registrations2NF VALUES
(1,'C101','A'),(1,'C102','B'),
(2,'C101','B'),(3,'C102','A');

CREATE TABLE Instructors3NF (
InstructorID VARCHAR(8) PRIMARY KEY,
InstructorName VARCHAR(40) NOT NULL
);

CREATE TABLE Courses3NF (
CourseID VARCHAR(8) PRIMARY KEY,
CourseName VARCHAR(40) NOT NULL,
InstructorID VARCHAR(8) NOT NULL,
FOREIGN KEY(InstructorID) REFERENCES Instructors3NF(InstructorID)
);

INSERT INTO Instructors3NF VALUES ('I10','Rao'),('I20','Sen');

INSERT INTO Courses3NF VALUES
('C101','DBMS','I10'),('C102','Networks','I20');

SELECT r.StudentID,s.StudentName,c.CourseName,
i.InstructorName,r.Grade
FROM Registrations2NF r
JOIN Students2NF s ON s.StudentID=r.StudentID
JOIN Courses3NF c ON c.CourseID=r.CourseID
JOIN Instructors3NF i ON i.InstructorID=c.InstructorID
ORDER BY r.StudentID,c.CourseID;

CREATE TABLE StudentInterestRaw (
StudentID INT, Hobby VARCHAR(30), Language VARCHAR(30),
PRIMARY KEY(StudentID,Hobby,Language)
);

INSERT INTO StudentInterestRaw VALUES
(1,'Chess','English'),(1,'Chess','Tamil'),
(1,'Music','English'),(1,'Music','Tamil');

SELECT * FROM StudentInterestRaw ORDER BY Hobby,Language;

CREATE TABLE StudentHobby4NF (
StudentID INT, Hobby VARCHAR(30),
PRIMARY KEY(StudentID,Hobby)
);

CREATE TABLE StudentLanguage4NF (
StudentID INT, Language VARCHAR(30),
PRIMARY KEY(StudentID,Language)
);

INSERT INTO StudentHobby4NF VALUES
(1,'Chess'),(1,'Music');

INSERT INTO StudentLanguage4NF VALUES
(1,'English'),(1,'Tamil');

SELECT h.StudentID,h.Hobby,l.Language
FROM StudentHobby4NF h JOIN StudentLanguage4NF l
ON h.StudentID=l.StudentID
ORDER BY h.Hobby,l.Language;

CREATE TABLE SupplyRaw (
SupplierID VARCHAR(8), PartID VARCHAR(8),
ProjectID VARCHAR(8),
PRIMARY KEY(SupplierID,PartID,ProjectID)
);

INSERT INTO SupplyRaw VALUES
('S1','P1','J1'),('S1','P2','J1'),
('S2','P1','J1');

CREATE TABLE SupplierPart5NF AS
SELECT DISTINCT SupplierID,PartID FROM SupplyRaw;

CREATE TABLE SupplierProject5NF AS
SELECT DISTINCT SupplierID,ProjectID FROM SupplyRaw;

CREATE TABLE PartProject5NF AS
SELECT DISTINCT PartID,ProjectID FROM SupplyRaw;

ALTER TABLE SupplierPart5NF ADD PRIMARY KEY(SupplierID,PartID);

ALTER TABLE SupplierProject5NF ADD PRIMARY KEY(SupplierID,ProjectID);

ALTER TABLE PartProject5NF ADD PRIMARY KEY(PartID,ProjectID);

SELECT sp.SupplierID,sp.PartID,sj.ProjectID
FROM SupplierPart5NF sp
JOIN SupplierProject5NF sj
ON sj.SupplierID=sp.SupplierID
JOIN PartProject5NF pj
ON pj.PartID=sp.PartID AND pj.ProjectID=sj.ProjectID
ORDER BY sp.SupplierID,sp.PartID,sj.ProjectID;

SELECT COUNT(*) AS OriginalRows FROM SupplyRaw;

SELECT COUNT(*) AS ReconstructedRows FROM (
SELECT sp.SupplierID,sp.PartID,sj.ProjectID
FROM SupplierPart5NF sp
JOIN SupplierProject5NF sj USING (SupplierID)
JOIN PartProject5NF pj
ON pj.PartID=sp.PartID AND pj.ProjectID=sj.ProjectID
) AS Reconstructed;

WITH Reconstructed AS (
SELECT sp.SupplierID,sp.PartID,sj.ProjectID
FROM SupplierPart5NF sp
JOIN SupplierProject5NF sj USING (SupplierID)
JOIN PartProject5NF pj
ON pj.PartID=sp.PartID AND pj.ProjectID=sj.ProjectID
)
SELECT 'Original only' AS DifferenceType, o.*
FROM SupplyRaw o LEFT JOIN Reconstructed r
ON r.SupplierID=o.SupplierID AND r.PartID=o.PartID
AND r.ProjectID=o.ProjectID
WHERE r.SupplierID IS NULL
UNION ALL
SELECT 'Reconstructed only',r.*
FROM Reconstructed r LEFT JOIN SupplyRaw o
ON r.SupplierID=o.SupplierID AND r.PartID=o.PartID
AND r.ProjectID=o.ProjectID
WHERE o.SupplierID IS NULL;
