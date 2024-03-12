# Data Normalization and Entity-Relationship Diagramming

An assignment to normalize the structure of data and establish a set of Entity-Relationship Diagrams for the data.

The contents of this file will be deleted and replaced with the content described in the [instructions](./instructions.md)

## Original Data

| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |

## Why the data is not 4nf compliant

The table contains many multi-valued dependencies such as professor, due date and assignment topic, making it so a single professor or course can have multiple values for assignments, readings, and grades that do not depend on each other. Additionally, the original table does not seperate the entities into logical groupings which is nedded for higher normal forms. The table also includes many implicit relationships such as between assignments and grades that are not managed using foreign keys. There is also no clearly identified primary key that uniquely defines each record. However, in 4nf each table must include a primary key so that every data item is properly uniquely identified. 

## 4nf Compliant Versions

### Courses Table
PK = course_id

| course_id | course_name          | classroom |
|-----------|----------------------|-----------|
| 101       | Data Science Basics  | WWH 101   |
| 102       | Advanced Databases   | 60FA 314  |
| 103       | Intro to Python      | WWH 201   |
| ...       | ...                  | ...       |

### Professors Table
pk= professor_id

| professor_id | professor_name | professor_email    |
|--------------|----------------|--------------------|
| 1            | Melvin         | l.melvin@foo.edu   |
| 2            | Logston        | e.logston@foo.edu  |
| 3            | Nevarez        | i.nevarez@foo.edu  |
| ...          | ...            | ...                |

### Assignments Table
pk = assignment_id
fk = course_id
fk = professor_id

| assignment_id | section_id | due_date  | assignment_topic                 |
|---------------|------------|-----------|----------------------------------|
| 1             | 1          | 23.02.21  | Data normalization               |
| 2             | 2          | 18.11.21  | Single table queries             |
| 3             | 3          | 05.05.21  | Python and pandas                |
| 4             | 4          | 04.07.21  | Spreadsheet aggregate functions  |
| ...           | ...        | ...       | ...                              |

### Sections Table
pk=section_id
fk=course_id
fk=professor_id

| section_id | course_id | professor_id | classroom | time        |
|------------|-----------|--------------|-----------|-------------|
| 1          | 101       | 1            | WWH 101   | 09:30-10:45 |
| 2          | 102       | 2            | 60FA 314  | 11:00-12:15 |
| 3          | 103       | 2            | WWH 201   | 02:00-03:15 |
| 4          | 103       | 3            | WWH 201   | 03:30-04:45 |
| ...        | ...       | ...          | ...       | ...         |


### Readings Table
pk=reading_id
fk=assignment_id

| reading_id | assignment_id | relevant_reading     |
|------------|---------------|----------------------|
| 1          | 1             | Deumlich Chapter 3   |
| 2          | 2             | Dümmlers Chapter 11  |
| 3          | 3             | Dümmlers Chapter 14  |
| 4          | 4             | Zehnder Page 87      |
| ...        | ...           | ...                  |

### Grades Table
pk=grade_id
fk= assignment_id
fk= student_id

| grade_id | student_id | assignment_id | grade |
|----------|------------|---------------|-------|
| 1        | 1          | 1             | 80    |
| 2        | 7          | 2             | 25    |
| 3        | 4          | 1             | 75    |
| 4        | 2          | 3             | 92    |
| 5        | 2          | 4             | 65    |
| ...      | ...        | ...           | ...   |

### Students Table
pk=student_id

| student_id | student_name |
|------------|--------------|
| 1          | Alice        |
| 2          | Bob          |
| 3          | Charlie      |
| 4          | Dylan        |
| 7          | Ethan        |
| ...        | ...          |


## Entity Relationship Diagram
![ER](/images/erd.drawio) 


## Changes made
To make the table compliant with 4NF, I split the table into several different tables such as courses, professors, students and assignments. Seperating these tables ensures that each table focuses on a single entity which in turn reduces redundancy and also improves data integrity. I also added a sections table to create clearer seperation between courses and their logistics which addresses the issue where different sections of the same course could meet at different times or with different professors. I also added surrogate keys like section_id and reading_id to make sure each record could be uniquely identified.
