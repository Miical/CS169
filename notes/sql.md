## 创建表并插入数据

**创建表**

```sql
CREATE TABLE customers (id INTEGER PRIMARY KEY, name TEXT, age INTEGER, weight REAL);
```

**插入数据**

```sql
INSERT INTO customers VALUES (73, "Brian", 33);

INSERT INTO customers (name, age) VALUES ("Brian", 33);
```

## 查询数据

```sql
SELECT * FROM customers;

SELECT * FROM customers WHERE age > 21;

SELECT * FROM customers WHERE age < 21 AND state = "NY";

SELECT * FROM customers WHERE plan IN ("free", "basic");

SELECT name, age FROM customers;

SELECT * FROM customers WHERE age > 21 ORDER BY age DESC;

SELECT name, CASE WHEN age > 18 THEN "adult" ELSE "minor" END "type" FROM customers;
```

NOTE:

**嵌套查询**

```sql
SELECT title FROM songs WHERE artist IN (SELECT name FROM artists WHERE genre == 'Pop');
```

**使用CASE计算结果**

```sql
SELECT type, heart_rate,
    CASE 
        WHEN heart_rate > 220-30 THEN "above max"
        WHEN heart_rate > ROUND(0.90 * (220-30)) THEN "above target"
        WHEN heart_rate > ROUND(0.50 * (220-30)) THEN "within target"
        ELSE "below target"
    END as "hr_zone"
FROM exercise_logs;
```



## 聚合数据

```sql
SELECT MAX(age) FROM customers;

SELECT gender, COUNT(*) FROM students GROUP BY gender;
```

**使用HAVING限制分组后数据的查询结果**

```sql
SELECT type, SUM(calories) AS total_calories FROM exercise_logs
    GROUP BY type
    HAVING total_calories > 150
    ;

/* COUNT函数的使用 */
SELECT type FROM exercise_logs GROUP BY type HAVING COUNT(*) >= 2;
```



## 联合相关表

```sql
/* cross join */
SELECT * FROM student_grades, students;

/* implicit inner join */
SELECT * FROM student_grades, students
    WHERE student_grades.student_id = students.id;
    
/* explicit inner join - JOIN */
SELECT students.first_name, students.last_name, students.email, student_grades.test, student_grades.grade FROM students
    JOIN student_grades
    ON students.id = student_grades.student_id
    WHERE grade > 90;
```

**LEFT OUTER JOIN**

```sql
/* outer join */ 
SELECT students.first_name, students.last_name, student_projects.title
    FROM students
    LEFT OUTER JOIN student_projects
    ON students.id = student_projects.student_id;
```

若student_projects里没有某个学生、这个学生也会显示

**self-joins**

```sql
/* self join */
SELECT students.first_name, students.last_name, buddies.email as buddy_email
    FROM students
    JOIN students buddies
    ON students.buddy_id = buddies.id;
```

**multiple joins**

```sql
SELECT a.title, b.title FROM project_pairs
    JOIN student_projects a
    ON project_pairs.project1_id = a.id
    JOIN student_projects b
    ON project_pairs.project2_id = b.id;
```

