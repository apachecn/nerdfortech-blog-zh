# SQL 102:连接和连接表

> 原文：<https://medium.com/nerd-for-tech/sql-102-joins-and-join-tables-d71d0dfc2f4?source=collection_archive---------11----------------------->

![](img/528c526fb72a7dd6f31744c40171d241.png)

在我上一篇[博客](https://kylefarmer85.medium.com/sql-101-create-a-table-insert-data-and-perform-queries-83368766b9fd)中，我们学习了如何创建一个表，插入数据，以及使用 SQL 查询表中的数据。现在，让我们看看如何使用 SQL 进行更复杂的查询，通过连接从两个表中获取数据，以及如何创建单独的连接表！

在上一篇博客中，我们使用了一个`students`表作为例子。让我们来看看如何使用 SQL 再次创建该表:

```
CREATE TABLE students(
 id INTEGER PRIMARY KEY,
 name TEXT,
 grade INTEGER,
 gpa FLOAT,
 tardies INTEGER);
```

现在让我们为老师做一张新桌子。

```
CREATE TABLE teachers(
 id INTEGER PRIMARY KEY,
 name TEXT,
 subject TEXT,
 grade INTEGER);
```

假设每个学生都有一个老师。我们怎样才能把学生和他们的老师联系起来？很简单，我们只需要在包含他们老师的`id`的`students`表中添加一列！每个教师的`id`将与`teachers`表上教师的主键相同。`students`表上的`teacher_id`称为**外键**。让我们看看如何用`ALTER TABLE`语句添加它:

```
ALTER TABLE Orders
  ADD FOREIGN KEY (teacher_id) REFERENCES teachers(id)
```

现在让我们将一些数据插入到表格中:

```
INSERT INTO students (name, grade, gpa, tardies, teacher_id)
  VALUES ("Jake", 12, 3.5, 4, 2);
INSERT INTO students (name, grade, gpa, tardies)
  VALUES ("Emily", 10, 3.7, 2, 4);
INSERT INTO students (name, grade, gpa, tardies)
  VALUES ("Sam", 11, 3.5, 3, 1);
INSERT INTO students (name, grade, gpa, tardies)
  VALUES ("Jordan", 10, 3.6, 2, null);
INSERT INTO students (name, grade, gpa, tardies)
  VALUES ("Victoria", 9, 3.2, 3, 3);INSERT INTO teachers (name, grade, subject)
  VALUES ("Mr. Smith", 12, "Math");
INSERT INTO students (name, grade, gpa, tardies)
  VALUES ("Mrs. Brown", 10, "Science");
INSERT INTO students (name, grade, gpa, tardies)
  VALUES ("Mrs. Reed", 11, "English");
INSERT INTO students (name, grade, gpa, tardies)
  VALUES ("Mr. Jones", 10, "Music");
```

## 使用内部联接

我们如何能看到所有学生的名字以及每个学生的老师的名字？我们可以使用`INNER JOIN`将`teacher_id`上的桌子连接起来。

```
SELECT student.name, teacher.name
FROM students
INNER JOIN teachers
ON students.teacher_id = teacher.id
```

这将返回包含学生姓名和每个学生的老师姓名的列的数据行。您可能还注意到，我们现在在写列名时使用了点符号，当 SQL 语句中引用了多个表时，这是必要的。当我们使用一个`INNER JOIN`时，只有在我们连接的列上*没有*空数据的行才会被返回。如果我们想要包含没有被分配教师的学生的记录，我们可以使用`LEFT JOIN`。

这种`students`和`teachers`的关系被称为“具有多个/属于”关系。每个学生“属于”一个老师，每个老师“有许多”学生。当您有这种关系时，外键将足以连接表。

## 错认假频伪信号

如果我们选择了很多列，我们的代码会有点冗长。让我们看看如何使用`AS`来别名。首先，让我们看一个选择几个列的例子，然后看看我们如何通过别名表名来清理它:

```
SELECT student.name, student.grade, student.gpa, teacher.name, teacher.subject
FROM students
INNER JOIN teachers
ON students.teacher_id = teacher.id
```

现在让别名`students`和`teachers`:

```
SELECT s.name, s.grade, s.gpa, t.name, t.subject
FROM students AS s
INNER JOIN teachers AS t
ON s.teacher_id = t.id
```

啊，好多了！我们还可以给列起别名:

```
SELECT s.name AS student_name, t.name AS teacher_name
FROM students AS s
INNER JOIN teachers AS t
ON s.teacher_id = t.id
```

继续，我们已经看到了如何用外键引用另一个表，但是如果每个学生都有多个老师，我们该怎么办呢？每行只能有一个`teacher_id`，所以如果每个学生有多个`teacher_id`就不行了。我们需要创建一个连接表！

## 创建连接表

由于现在每个学生有很多老师，每个老师又有很多学生，我们称之为多对多关系。每当我们有多对多的关系时，就需要一个连接表。可以查询每个学生有哪些老师，每个老师有哪些学生。所以表中的每一行都包含一个`teacher_id`和一个`student_id`。让我们称之为`class_assignments`，现在创建表格:

```
CREATE TABLE class_assignments(
 student_id INTEGER,
 teacher_id INTEGER);
```

现在让我们插入一些数据:

```
INSERT INTO class_assignments (student_id, teacher_id) VALUES (3, 2)
INSERT INTO class_assignments (student_id, teacher_id) VALUES (1, 2)
INSERT INTO class_assignments (student_id, teacher_id) VALUES (3, 4)
INSERT INTO class_assignments (student_id, teacher_id) VALUES (1, 3)
```

我们如何创建一个查询来获取学生的教师姓名？让我们以我们的学生“Sam”为例，他的`id`为 3:

```
SELECT teacher.name 
FROM teacher
INNER JOIN class_assignments 
ON teacher.id **=** class_assignments.teacher_id WHERE student.id **=** 3;
```

通过将`teachers`表与我们的`class_assignments`连接表连接起来，我们得到了 Sam 所有老师的名字。很整洁，对吧！？