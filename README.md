# mgruniv_sql

The following are the instructions for introduction to SQL session held during placement training at MGR University, Chennai

### 1. Creating the Database
To create a new database called `StudentDB`:

```sql
CREATE DATABASE StudentDB;
```

This statement tells the SQL server to create a new database with the name `StudentDB`.

### 2. Using the Database
To switch to the newly created database so that all subsequent operations are performed within it:

```sql
USE StudentDB;
```

This statement sets `StudentDB` as the active database.

### 3. Creating Tables

#### Student Table

syntax:
```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
   ....
);
```

```sql
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Age INT,
    Gender CHAR(1)
);
```

- `CREATE TABLE Students`: Creates a new table named `Students`.
- `StudentID INT PRIMARY KEY`: Defines `StudentID` as an integer and the primary key, ensuring it is unique and not null.
- `FirstName VARCHAR(50)`: Defines `FirstName` as a variable character string with a maximum length of 50.
- `LastName VARCHAR(50)`: Same as above for the last name.
- `Age INT`: Defines `Age` as an integer.
- `Gender CHAR(1)`: Defines `Gender` as a character with a length of 1 (e.g., 'M' or 'F').

#### Courses Table

```sql
CREATE TABLE Courses (
    CourseID INT PRIMARY KEY,
    CourseName VARCHAR(100)
);
```

- Similar to the `Students` table, but with fields specific to courses.

#### Enrollments Table

```sql
CREATE TABLE Enrollments (
    EnrollmentID INT PRIMARY KEY,
    StudentID INT,
    CourseID INT,
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);
```

- This table keeps track of which students are enrolled in which courses.
- `FOREIGN KEY (StudentID) REFERENCES Students(StudentID)`: Ensures that `StudentID` in `Enrollments` corresponds to an existing `StudentID` in the `Students` table.
- `FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)`: Ensures that `CourseID` in `Enrollments` corresponds to an existing `CourseID` in the `Courses` table.

### 4. Primary Key and Foreign Key
- **Primary Key**: Uniquely identifies each record in a table.
- **Foreign Key**: Links records in one table to records in another table.

### 5. Alter Table
Syntax:
```sql
ALTER TABLE table_name
ADD column_name datatype;
```

Example:
```sql
ALTER TABLE Students
ADD Email varchar(255);
```

### 5. Inserting Data

To insert data into the `Students` table:

```sql
INSERT INTO Students (StudentID, FirstName, LastName, Age, Gender) VALUES (1, 'John', 'Doe', 20, 'M');
```

This inserts a new student record with the given details.

To insert data into the `Courses` table:

```sql
INSERT INTO Courses (CourseID, CourseName) VALUES (101, 'Database Systems');
```

To insert data into the `Enrollments` table:

```sql
INSERT INTO Enrollments (EnrollmentID, StudentID, CourseID) VALUES (1001, 1, 101);
```

### 6. Select Statement

syntax:
```sql
SELECT column1, column2, ...
FROM table_name;
```

To retrieve all records from the `Students` table:

```sql
SELECT * FROM Students;
```

- `SELECT *`: Selects all columns.
- `FROM Students`: Specifies the table to retrieve data from.

To retrieve specific columns:

```sql
SELECT FirstName, LastName FROM Students;
```

### 7. Update Table
```sql
UPDATE Customers
SET Email="johndoe@gmail.com"
WHERE lastName = doe;
```

### 7. Where Clause

To filter results based on a condition:

```sql
SELECT * FROM Students WHERE Age > 18;
```

This retrieves all students older than 18.

Using multiple conditions:

```sql
SELECT * FROM Students WHERE Age > 18 AND Gender = 'M';
```

This retrieves all male students older than 18.

### 8. Joins

To combine records from multiple tables:

#### Inner Join

```sql
SELECT Students.FirstName, Students.LastName, Courses.CourseName
FROM Enrollments
INNER JOIN Students ON Enrollments.StudentID = Students.StudentID
INNER JOIN Courses ON Enrollments.CourseID = Courses.CourseID;
```

This retrieves the first and last names of students along with the course names they are enrolled in.

#### Left Join

```sql
SELECT Students.FirstName, Students.LastName, Courses.CourseName
FROM Students
LEFT JOIN Enrollments ON Students.StudentID = Enrollments.StudentID
LEFT JOIN Courses ON Enrollments.CourseID = Courses.CourseID;
```

This retrieves all students, including those not enrolled in any courses.

### 9. Having Clause

To filter groups of data:

```sql
SELECT CourseID, COUNT(StudentID)
FROM Enrollments
GROUP BY CourseID
HAVING COUNT(StudentID) > 1;
```

This retrieves courses with more than one student enrolled.

### 10. Order By

To sort results:

```sql
SELECT * FROM Students ORDER BY LastName ASC;
```
This sorts the student records by last name in ascending order.


```sql
SELECT * FROM Students ORDER BY LastName DESC;
```

This sorts the student records by last name in descending order.

### 11. Multiple Queries

#### Nested Queries

```sql
SELECT * FROM Students
WHERE StudentID IN (SELECT StudentID FROM Enrollments WHERE CourseID = 101);
```

This retrieves all students enrolled in the course with `CourseID` 101.

#### Union

```sql
SELECT FirstName, LastName FROM Students WHERE Gender = 'M'
UNION
SELECT FirstName, LastName FROM Students WHERE Age > 20;
```


#### 12. Aggregate Functions
- **Examples**:
  ```sql
  SELECT COUNT(*) FROM Students;
  SELECT AVG(Age) FROM Students;
  SELECT MIN(Age), MAX(Age) FROM Students;
  ```

#### 13. Subqueries

- **Example**:
  ```sql
  SELECT FirstName, LastName 
  FROM Students 
  WHERE Age = (SELECT MAX(Age) FROM Students);
  ```

#### 14. Indexes 
- 
- **Creating an Index**:
  ```sql
  CREATE INDEX idx_lastname ON Students(LastName);
  ```

#### 15. Views 
- **Creating a View**:
  ```sql
  CREATE VIEW StudentCourses AS
  SELECT Students.FirstName, Students.LastName, Courses.CourseName
  FROM Enrollments
  INNER JOIN Students ON Enrollments.StudentID = Students.StudentID
  INNER JOIN Courses ON Enrollments.CourseID = Courses.CourseID;
  ```
- **Using the View**:
  ```sql
  SELECT * FROM StudentCourses;
  ```

- **Complex Queries**: Write queries that combine multiple concepts (joins, subqueries, aggregate functions).
  ```sql
  SELECT Students.FirstName, Students.LastName, COUNT(Enrollments.CourseID) AS CourseCount
  FROM Students
  LEFT JOIN Enrollments ON Students.StudentID = Enrollments.StudentID
  GROUP BY Students.StudentID, Students.FirstName, Students.LastName
  HAVING COUNT(Enrollments.CourseID) > 1
  ORDER BY CourseCount DESC;
  ```
