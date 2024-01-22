# DDL Commands:
# Create a Database:
Task: Create a new database named "SchoolDB" with an appropriate character set.
# Create Tables:

Task: Design and create a table named "Students" with the following columns:
# Classrooms:

classroom_id (Primary Key)
classroom_name
teacher_id

#  Students:
student_id (Primary Key)
student_name
classroom_id (Foreign Key referencing classroom_id in the "Classrooms" table)
grade

# Modify Table Structure:

Task: Add a new column named "Gender" (VARCHAR, Maximum 10 characters) to the "Students" table.

# DML Commands:
Insert Data:

Task: Insert at least 5 records into the "Students" table.
# Update Records:

Task: Update the "Age" of students who are in the "Class" 10 to 16.
# Delete Records:

Task: Delete all records of students whose "Age" is less than 10.
# DQL Command:
# Select Data:

Task: Write a SELECT query to retrieve the "FirstName," "LastName," and "Age" of all students in the "SchoolDB."
# Aggregate Functions:

Task: Use aggregate functions to find the average age of all students.

# Primary Key and Foreign Key:
# Define Primary Key:

Task: Explain the concept of a primary key and its significance. Also, ensure that the "StudentID" column in the "Students" table is a primary key.
# Create Foreign Key:

Task: Create a new table named "Courses" with columns: "CourseID" (Integer, Primary Key) and "CourseName" (VARCHAR, Maximum 50 characters). Then, add a foreign key constraint in the "Students" table referencing the "CourseID" in the "Courses" table.


Write SQL queries to perform the following tasks:

# List all students with their respective classrooms and teacher names.

# Show the average grade for each classroom. Display the classroom name and the average grade.

# Find the names of students who belong to a classroom taught by a specific teacher (you can choose a teacher_id).

# List all classrooms along with the number of students in each classroom.

# Show the details of classrooms that have an average grade above a certain value (you can choose a specific grade).





CREATE Database SchoolDB;

CREATE TABLE Classrooms (classroom_id INT PRIMARY KEY, classroom_name VARCHAR(100), teacher_id INT);

CREATE TABLE Students (student_id INT PRIMARY KEY, student_first_name VARCHAR(100), student_last_name VARCHAR(100), 
					  classroom_id INT, FOREIGN KEY (classroom_id) REFERENCES Classrooms(classroom_id));
					  
ALTER TABLE Students ADD gender VARCHAR(10);

CREATE TABLE Teachers (teacher_id INT PRIMARY KEY, teacher_first_name VARCHAR(100), teacher_last_name VARCHAR(100));

ALTER TABLE Classrooms ADD FOREIGN KEY(teacher_id) REFERENCES Teachers(teacher_id);

INSERT INTO Teachers VALUES
	(1001, 'Richard', 'Lionheart'),
	(1002, 'Paul', 'Wagner'),
	(1003, 'Saul', 'Williamson'),
	(1004, 'William', 'Paulson'),
	(1005, 'Steffi', 'Stiletto'),
	(1006, 'Maxemilian', 'Strongman');

SELECT * FROM Teachers;

INSERT INTO Classrooms VALUES
	(101, 'Red Room', 1006),
	(102, 'Blue Room', 1001),
	(103, 'Pink Room', 1002),
	(104, 'Black Room', 1003),
	(105, 'Green Room', 1005),
	(106, 'White Room', 1004);

SELECT * FROM Classrooms;

INSERT INTO Students VALUES
	(1, 'Peter', 'Alexander', 101, 'male'),
	(2, 'Birgit', 'Rothaus', 101, 'female'),
	(3, 'Kevin', 'Costner', 101, 'male'),
	(4, 'Vanilla', 'Aroma', 102, 'female'),
	(5, 'Sabrina', 'Setlur', 102, 'female'),
	(6, 'Roland', 'Ritter', 103, 'male');
	
	
SELECT * FROM Students;

ALTER TABLE Students ADD age INT;

UPDATE Students SET age = 16 WHERE classroom_id IN (101);

UPDATE Students SET age = 12 WHERE classroom_id IN (102);

UPDATE Students SET age = 8 WHERE classroom_id IN (103);

DELETE FROM Students WHERE age < 10;

SELECT student_first_name, student_last_name, age FROM Students;

SELECT ROUND(AVG(age), 2) FROM Students;

CREATE TABLE Courses (course_id INT PRIMARY KEY, course_name VARCHAR(50));

INSERT INTO Courses VALUES
	(3001, 'Math'),
	(3002, 'History'),
	(3003, 'English'),
	(3004, 'German'),
	(3005, 'Sports'),
	(3006, 'Art');
	
SELECT * FROM Courses;

ALTER TABLE Students ADD course_id INT;

ALTER TABLE Students ADD FOREIGN KEY(course_id) REFERENCES Courses(course_id);

UPDATE Students SET course_id = 3003 WHERE classroom_id = 101;

UPDATE Students SET course_id = 3006 WHERE classroom_id = 102;

SELECT s.student_id, s.student_first_name, s.student_last_name, c.classroom_name, t.teacher_first_name, 
		t.teacher_last_name FROM Students s
		JOIN Classrooms c ON s.classroom_id = c.classroom_id
		JOIN Teachers t ON c.teacher_id = t.teacher_id;
		
SELECT s.student_first_name, s.student_last_name FROM Students s
	JOIN Classrooms c ON s.classroom_id = c.classroom_id
		WHERE c.teacher_id = 1001;

SELECT c.classroom_id, c.classroom_name, COUNT(s.student_id) AS number_of_students 
	FROM Classrooms c
	LEFT JOIN Students s ON c.classroom_id = s.classroom_id
	GROUP BY c.classroom_id, c.classroom_name
	ORDER BY c.classroom_id;
