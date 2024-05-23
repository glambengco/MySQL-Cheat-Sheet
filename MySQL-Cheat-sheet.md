# MySQL Cheat Sheet for Data Analysis

## Table of Contents

[**Retrieving Data**](#retrieving-data)
* [Selecting Data](#selecting-data) (`SELECT`, `FROM`)
* [Sorting and Filtering Data](#sorting-and-filtering-data) (`ORDER BY`, `WHERE`, `BETWEEN`, `LIKE`, `LIMIT`, `OFFSET`)
* [Aggregating Data](#aggregating-data) (`DISTINCT`, `COUNT`, `SUM`, `MIN`, `MAX`, `AVG`)
* [Aliasing](#aliasing) (`AS`)
* [Grouping Data](#grouping-data) (`GROUP BY`, `HAVING`)

[**Data Wrangling**](#data-wrangling)
* [String Functions](#string-functions)
* [Handling Null Values](#handling-null-values)
* [Date and Time Functions ](#date-and-time-functions)
* [Conditional Statements](#conditional-statements) (`CASE`)

[**Merging Data**](#merging-data) (`JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `CROSS JOIN`, `UNION`)

[**Subqueries and CTEs**](#subqueries-and-ctes)
* [Subqueries](#subqueries)
* [Common Table Expressions](#common-table-expressions-cte) (`WITH`)

[**Window Functions**](#window-functions) (`OVER`, `PARTITION BY`)
* [Ranking Data]() (`ROW_NUMBER`, `RANK`, `DENSE_RANK`)

[**Advanced Functions**](#advanced-functions)
* [Temporary Tables](#temporary-tables)
* [Stored Procedures](#stored-procedures)
* [Triggers](#triggers)

## Retrieving Data

### Selecting Data
| Command  | Description                              | Example Query           |
| -------- | ---------------------------------------- | ----------------------- |
| `SELECT` | Selects and display a value              | `SELECT 'random_string'`|
| `FROM`   | Used with `SELECT` to query from a table | `SELECT * FROM salary_table` - displays all columns |
|          |                                          | `SELECT first_name, last_name, age FROM salary_table` - displays name and age |

[back to table of contents](#table-of-contents)

### Sorting and Filtering Data
| Command    | Description                      | Example Query |
| ---------- | -------------------------------- | ------------- |
| `ORDER BY` | Sorts rows by a specified column. **NOTE:** Ascending is the default order, so no need to explicitly type `ASC` | `SELECT * FROM salary_table ORDER BY age` - displays data sorted by increasing age |
|            | Use `DESC` for descending order  | `SELECT * FROM salary_table ORDER BY salary DESC` - displays data sorted by decreasing salary |
| `WHERE`    | Filters data using a specific condition  | `SELECT first_name, last_name, job_position FROM salary_table WHERE age >= 50` - displays names and job positions where age is at least 50 |
| `BETWEEN`  | Determine whether data falls between a specified range, inclusive | `SELECT * FROM salary_table WHERE salary BETWEEN 30000 AND 50000` - Display rows where salary is between 30000 and 50000, inclusive |
| `LIKE`     | Displays approximate match       | `SELECT * FROM salary_table WHERE job_position LIKE 'Junior%'` - displays rows where job position starts with "Junior" followed by any number of characters |
|            |                                  | `SELECT * FROM salary_table WHERE name LIKE 'A__'` - displays rows where name starts with A and followed by exactly two characters |
| `LIMIT`    | Displays the top rows            | `SELECT * FROM salary_table LIMIT 10` - displays top 10 rows |
| `OFFSET`   | Offsets `LIMIT` by a specified number | `SELECT * FROM salary_table LIMIT 10 OFFSET 2` - displays top 10 rows starting from the 2nd row (displays 3rd to 12th row) |

[back to table of contents](#table-of-contents)

### Aggregating Data
| Command    | Description                                | Example Query           |
| ---------- | ------------------------------------------ | ----------------------- |
| `DISTINCT` | Displays unique values of a column         | `SELECT DISTINCT job_position FROM salary_table` - displays all unique values of job position |
| `COUNT`    | Counts the number of rows from selection   | `SELECT COUNT(*) FROM salary_table WHERE salary > 50000` - displays number of rows having salary values over 50000 |
|            | Use with `DISTINCT` to count unique values | `SELECT COUNT(DISTINCT job_position) FROM salary_table WHERE age >= 60` - displays number of unique job positions with at least one employee having age of at least 60 |
| `SUM`      | Displays the sum of values from selection  | `SELECT SUM(salary) FROM salary_table` |
|            |                                            | `SELECT SUM(salary) FROM salary_table WHERE age >= 60` - calculates total salary from all employees having age of at least 60 |
| `MIN`      | Displays minimum value from selection      | `SELECT MIN(salary) FROM salary_table` |
| `MAX`      | Displays maximum value from selection      | `SELECT MAX(salary) FROM salary_table WHERE gender = 'Female'` - displays maximum salary of all female employees |
| `AVG`      | Calculates average value from selection    | `SELECT AVG(salary) FROM salary_table` |
| `STD`         | Calculates the population standard deviation from selection | `SELECT STD(salary) FROM salary_table` |
| `STDDEV`      | Same as `STD` |
| `STDDEV_POP`  | Same AS `STD` |
| `STDDEV_SAMP` | Calculates the sample standard deviation from selection | `SELECT STDDEV_SAMP(salary) FROM salary_table` |
| `VARIANCE`    | Calculates the population variance from selection | `SELECT VARIANCE(salary) FROM salary_table` |
| `VAR_POP`     | Same as  `VARIANCE` |
| `VAR_SAMP`    | Calculates the sample variance from selection | `SELECT VAR_SAMP(salary) FROM salary_table` |

[back to table of contents](#table-of-contents)

### Aliasing
| Command | Description               | Example Query           |
| ------- | ------------------------- | ----------------------- |
| `AS`    | Renames a column or table | `SELECT DISTINCT department_name AS department_list FROM department_table` - displays unique values of departments into a column named department_list |

**NOTE:** Aliasing is useful in shortening column names for easier referencing and readability. Aliasing is also used in [subqueries](#subqueries).

[back to table of contents](#table-of-contents)

### Grouping Data
| Command    | Description               | Example Query           |
| ---------- | ------------------------- | ----------------------- |
| `GROUP BY` | Groups data by a specified column for using [aggregate functions](#aggregating-data) | `SELECT gender, COUNT(*) FROM salary_table GROUP BY gender` - displays values for gender column and number of employees for each gender value |
| `HAVING`   | Similar to `WHERE` but used when filtering grouped data | `SELECT job_position, AVG(salary) FROM salary_table GROUP BY job_position HAVING AVG(salary) > 50000` Displays each job position with respective average salary from job positions having average salary values over 50000 |
|            |                           | `SELECT job_position, AVG(salary) FROM salary_table GROUP BY job_position HAVING job_position LIKE 'Junior%'` - displays average salary for each junior position |

**NOTE:** `GROUP BY` is first executed before filtering with `HAVING`; therefore `HAVING` cannot be used to filter data before grouping. [Subqueries](#subqueries) should be used to filter data before grouping [(see example)](#using-a-subquery-to-filter-data-before-grouping).

[back to table of contents](#table-of-contents)


## Data Wrangling

### String Functions
| Command     | Description               | Example Query           | Return Value |
| ----------- | ------------------------- | ----------------------- | ------------ |
| `LENGTH`    | Displays the character length of the string | `SELECT LENGTH('Example text')` | `12` |
| `UPPER`     | Displays string in all upper case | `SELECT UPPER('Example text')` | `'EXAMPLE TEXT'` |
| `LOWER`     | Displays string in all lower case | `SELECT LOWER('Example text')` | `'example text'` |
| `TRIM`      | Removes whitespace from both sides | `SELECT TRIM('  Text  ')` | `'Text'` | 
| `LTRIM`     | Removes whitespace from the left side | `SELECT LTRIM('  Text  ')` | `'Text  '` |
| `RTRIM`     | Removes whitespace from the right side | `SELECT RTRIM('  Text  ')` | `'  Text'` |
| `LEFT`      | Displays the first n characters of the string | `SELECT LEFT('Example Text', 4)` | `'Exam'` |
| `RIGHT`     | Displays the last n characters of the string | `SELECT RIGHT('Example Text', 4)` | `'Text'` |
| `LOCATE`    | Displays the index of the first occurrence of the specified character | `SELECT LOCATE('a', 'Banana')` | `2` |
| `SUBSTRING` | Displays substring using a specified starting index and substring length | `SELECT SUBSTRING('1991-04-30', 6, 2)` | `'04'` |
| `REPLACE`   | Replaces one substring A from a string with another substring B | `SELECT REPLACE('Banana', 'na', 'l')` | `'Ball'` |
| `CONCAT`    | Concatenates strings in specified order | `SELECT CONCAT('first_name', '.', 'last_name', '@example.com')` | `'first_name.last_name@example.com'` |

[back to table of contents](#table-of-contents)

### Handling Null Values
| Command  | Description                              | Example Query           |
| -------- | ---------------------------------------- | ----------------------- |
| `ISNULL` | Checks if the value of a field is `NULL` | `SELECT * FROM salary_table WHERE ISNULL(department_id)` - displays rows with missing department values |

[back to table of contents](#table-of-contents)

### Date and Time Functions

#### Current Date and Time
| Command            | Description                                          | Example Query               |
| ------------------ | ---------------------------------------------------- | --------------------------- |
| `CURRENTDATE`      | Displays current date (YYYY-MM-DD)                   | `SELECT CURRENTDATE()`      |
| `CURRENTTIME`      | Displays current time (HH:MM:SS)                     | `SELECT CURRENTTIME()`      |
| `CURRENTTIMESTAMP` | Displays current date and time (YYYY-MM-DD HH:MM:SS) | `SELECT CURRENTTIMESTAMP()` |

#### Date
| Command      | Description | Example Query | Return Value |
| ------------ | ----------- | ------------- | ------------ |
| `DATE`       | Extracts date from a datetime string | `SELECT DATE('2024-12-25 12:45:00')` | `2024-12-25` |
|              | Extract date from a timestamp | `SELECT DATE(CURRENTTIMESTAMP())` | `2024-05-21` |
| `YEAR`       | Displays the year of a date | `SELECT YEAR(DATE('2024-12-25'))` | `2024` |
| `MONTH`      | Displays the month of from a date | `SELECT MONTH(DATE('2024-12-25'))` | `12` |
| `MONTHNAME`  | Displays the month name of a date | `SELECT MONTHNAME(DATE('2024-12-25'))` | `'December'` |
| `DAY`        | Displays the day of a  date | `SELECT DAY(DATE('2024-12-25'))` | `25` |
| `DAYNAME`    | Displays the day of week name of a date | `SELECT DAYNAME(DATE('2024-12-25'))` | `'Wednesday'` |
| `DAYOFWEEK`  | Displays the day of week number (1: Sunday, 7:Saturday) of a date | `SELECT DAYOFWEEK(DATE('2024-12-25'))` | `4` |
| `WEEKDAY`    | Displays the day of week number (0: Monday, 6: Sunday) of a week | `SELECT WEEKDAY(DATE('2024-12-25'))` | `2` |
| `DAYOFMONTH` | Displays the day of month of a date | `SELECT DAYOFMONTH(DATE('2024-12-25'))` | `25` |
| `DAYOFYEAR`  | Displays the day of year of a date | `SELECT DAYOFYEAR(DATE('2024-12-25'))` | `360` |
| `WEEK`       | Displays the week of year (starting first Monday of the year) of a date | `SELECT WEEK(DATE('2024-12-25'))` | `51` |
| `WEEKOFYEAR` | Displays the week of year (starting first day of the year) of a date | `SELECT WEEKOFYEAR(DATE('2024-12-25'))` | `52` |
| `YEARWEEK`   | Displays the `YEAR` + `WEEK` of a date | `SELECT YEARWEEK(DATE('2024-12-25'))` | `202451` |
| `QUARTER`    | Displays the quarter (Q1-Q4) of a date | `SELECT QUARTER(DATE('2024-12-25'))` | `4` |

#### Time
| Command  | Description                          | Example Query                        | Return Value |
| -------- | ------------------------------------ | ------------------------------------ | ------------ |
| `TIME`   | Extracts time from a datetime string | `SELECT TIME('2024-12-25 12:45:00')` | `12:45:00`   |
|          | Extract time from a timestamp        | `SELECT TIME(CURRENTTIMESTAMP())`    | `18:51:00`   |
| `HOUR`   | Extract hour (0-23) of a time        | `SELECT HOUR(TIME(12:45:00))`        | `12`         |
|`MINUTE`  | Extract minute from a time           | `SELECT MINUTE(TIME(12:45:00))`      | `45`         |
| `SECOND` | Extract seconds from a  time          | `SELECT SECOND(TIME(12:45:00))`      | `0`          |

#### Date and Time Formatting
##### Date formats
Example using the timestamp `2024-12-25 13:45:00`:
| Format | Description                                              | Return value  |
| ------ | -------------------------------------------------------- | ------------- |
| `%Y`   | Year (4 digit)                                           | `2024`        |
| `%y`   | Year (2 digit) | `24`                                    | `24`          |
| `%m`   | Month number, leading zero (01-12)                       | `12`          |
| `%c`   | Month number (1-12)                                      | `12`          |
| `%M`   | Month name                                               | `'December'`  |
| `%b`   | Month name, 3-letter)                                    | `'Dec'`       |  
| `%D`   | Day of month, ordinal (1st-31st)                         | `'25th'`      |
| `%d`   | Day of month, leading zero (01-31)                       | `25`          | 
| `%e`   | Day of month (1-31)                                      | `25`          |
| `%w`   | Day of week number (Sun=0, Sat=6)                        | `3`           |
| `%W`   | Day of week name                                         | `'Wednesday'` |
| `%a`   | Day of week, 3-letter                                    | `'Wed'`       |
| `%j`   | Day of year, leading zero (001-366)                      | `360`         |
| `%U`   | Week of year number, year starts on first Sunday (00-53) | `51`          |
| `%u`   | Week of year number, year starts on first Monday (00-53) | `52`          |
| `%V`   | Week of year number, year starts on first Sunday (01-53) | `51`          |
| `%v`   | Week of year number, year starts on first Monday (01-53) | `52`          |
| `%X`   | Year, used with %V                                       | `2024`        |
| `%x`   | Year, used with %v                                       | `2024`        |

##### Time formats
Example using the timestamp `2024-12-25 13:45:00`:
| Format     | Description                                | Return value   |
| ---------- | ------------------------------------------ | -------------- |
| `%T`       | hh:mm:ss (24h)                             | `'13:45:00'`   |
| `%r`       | hh:mm:ss AM/PM (12h)                       | `'1:45:00 PM'` |
| `%H`       | Hour 24h format, leading zero (00-23)      | `13`           |
| `%k`       | Hour 24h format (0-23)                     | `13`           |
| `%h`, `%I` | Hour 12h format, leading zero (01-12)      | `01`           |
| `%l`       | Hour 12h format (1-12)                     | `1`            |
| `%p`       | AM or PM                                   | `'PM'`         |
| `%i`       | Minutes, leading zero (00-59)              | `45`           |
| `%S`, `%s` | Seconds, leading zero (00-59)              | `00`           |
| `%f`       | Microseconds, leading zero (000000-999999) | `000000`       |

##### Format to string
Use the above strings to format date and time with the following commands:
| Command       | Description                          | Example Query                                   | Return Value          |
| ------------- | ------------------------------------ | ----------------------------------------------- | --------------------- |
| `DATE_FORMAT` | Format a date and returns as string  | `SELECT DATE_FORMAT('2023-12-23', '%M %d, %Y')` | `'December 23, 2023'` |
| `TIME_FORMAT` | Format a time and returns as string  | `SELECT TIME_FORMAT('12:45:00', '%I %i %p')`    | `'12:45 PM'`          |

[back to table of contents](#table-of-contents)

### Conditional Statements
`CASE` assigns value based on a set of at least 1 condition.
```SQL
SELECT first_name, last_name, salary,
CASE
    WHEN salary > 50000 THEN 'high'
    WHEN salary > 30000 THEN 'medium'
    ELSE 'low'
END
AS salary_bracket
FROM salary_table
```
**NOTE:** If none of the `WHEN` conditions are met and no `ELSE` is stated, the field will be assigned to `NULL`.

[back to table of contents](#table-of-contents)


## Merging Data
| Command      | Description                       | Example Query                                               |
| ------------ | --------------------------------- | ----------------------------------------------------------- |
| `JOIN`       | Performs an inner join            | `SELECT * FROM salary_table JOIN department_table ON salary_table.department_id = department_table.department_id` |
| `LEFT JOIN`  | Performs a left outer join        | `SELECT * FROM salary_table JOIN department_table ON salary_table.department_id = department_table.department_id` |
| `RIGHT JOIN` | Performs a right outer join       | `SELECT * FROM salary_table LEFT JOIN department_table ON salary_table.id = department_table.id` |
| `CROSS JOIN` | Performs a cross join             | `SELECT * FROM salary_table RIGHT JOIN department_table ON salary_table.department_id = department_table.department_id` |
| `UNION`      | Performs a union of rows          | `SELECT * FROM salary_table CROSS JOIN department_table`                    |

**NOTE:** MySQL does not have a built-in `FULL OUTER JOIN` command. To perform a full outer join, use the following query:
```SQL
SELECT * FROM salary_table LEFT JOIN department_table ON salary_table.id = department_table.id
UNION
SELECT * FROM salary_table RIGHT JOIN department_table ON salary_table.id = department_table.id
```

[back to table of contents](#table-of-contents)

## Subqueries and CTEs

### Subqueries

#### Using a subquery to filter data before grouping
Example query
```SQL
SELECT job_position, AVG(salary)
FROM
(SELECT *
  FROM salary_table
  WHERE salary >= 50000
  ) AS filtered_table
GROUP BY job_position
```

#### Using [CTE](#common-table-expressions-cte) for better readability:
Example query
```SQL
WITH CTE_salary AS (
  SELECT *
  FROM salary_table
  WHERE salary >= 50000
)

SELECT job_position, AVG(salary)
FROM CTE_salary
GROUP BY job_position
```

#### Using a subquery to filter table by an aggregate function
Example query
```SQL
SELECT *
FROM salary_table
WHERE salary >= (SELECT AVG(salary) FROM salary_table)
```

[back to table of contents](#table-of-contents)

### Common Table Expressions (CTE)

#### Multiple CTEs
Example query
```SQL
WITH CTE_1 AS {
  SELECT *
  FROM salary_table
  WHERE salary >= 50000
},
CTE_2AS (
  SELECT *
  FROM employee_table
  WHERE overtime_hours >= 5
)

SELECT * 
FROM CET_1
JOIN CTE_2
    ON CTE_1.id = CTE_2.id
```

#### CTE with column names
Example query
```SQL
WITH CTE_1 (job_position, average_salary, average_age AS (
  SELECT job_position, AVG(salary), AVG(age)
  FROM salary_table
  GROUP BY job_position
)

SELECT *
FROM CTE_1
```

[back to table of contents](#table-of-contents)


## Window Functions
| Command        | Description | Example Query |
| -------------- | ----------- | ------------- |
| `OVER`         | Can be used to display aggregate function to a new column | `SELECT *, AVG(salary) OVER() FROM salary_table` - displays average salary to a new column |
| `PARTITION BY` | Used in `OVER` to group data when creating a Window function | `SELECT *, AVG(salary) OVER(PARTITION BY department_id) FROM salary_table` - displays average salary of the respective department of each row |

[back to table of contents](#table-of-contents)

#### Rolling total using window functions.
```SQL
SELECT *, SUM(salary) OVER(ORDER BY id)
FROM salary_table
```

### Ranking Data
Use `OVER(ORDER BY column)` to specify which column to use for ranking.
| Command      | Description                       |
| ------------ | --------------------------------- |
| `ROW_NUMBER` | Rows with same value will be ranked by some pre-existing order (1, 2, 3, 4, 5, 6, 7) |
| `RANK`       | Rows with same value will get the same rank, next rank will be the next position (1, 2, 3, 3, 5, 6, 7) |
| `DENSE_RANK` | Rows with same value will get the same rank, next rank will be the next number (1, 2, 3, 3, 4, 5, 6) |

Example query to compare different ranking functions:
```SQL
SELECT salary, gender, 
       ROW_NUMBER(ORDER BY salary DESC) as `row_number`, 
       RANK(ORDER BY salary DESC) AS `rank`, 
       DENSE_RANK(ORDER BY salary DESC) AS `dense_rank`
FROM salary_table
ORDER BY salary DESC
LIMIT 7
```
Example output:
| salary | gender | row_number | rank | dense_rank |
| ------ | ------ | ---------- | ---- | ---------- |
| 95000  | Male   | 1 | 1 | 1 |
| 85000  | Male   | 2 | 2 | 2 |
| 70000  | Female | 3 | 3 | 3 |
| 70000  | Male   | 4 | 3 | 3 |
| 65000  | Female | 5 | 5 | 4 |
| 60000  | Female | 6 | 6 | 5 |
| 50000  | Male   | 7 | 7 | 6 |

[back to table of contents](#table-of-contents)


## Advanced Functions

### Temporary Tables
Example query
```SQL
CREATE TEMPORARY TABLE email_table
SELECT id, first_name, last_name,
CONCAT(LOWER(first_name), '_', LOWER(last_name), '@example.com') AS email
FROM employee_table
;

SELECT *
FROM email_table
```

[back to table of contents](#table-of-contents)

### Stored Procedures
Example query to create procedure
```SQL
USE employee_db;
DROP PROCEDURE IF EXISTS get_top_salaries;

DELIMITER $$
USE `employee_db`$$
CREATE PROCEDURE get_top_salaries (top_n_param INT)
BEGIN
	SELECT *, RANK() OVER(ORDER BY salary DESC)
    FROM salary_table
    ORDER BY salary DESC
    LIMIT top_n_param
    ;
END $$
DELIMITER ;
```

Example query to store procedure
```SQL
CALL get_top_salaries(10);
```

[back to table of contents](#table-of-contents)

### Triggers
Example query to create a trigger
```SQL
USE parks_and_r`employee_db`;
DROP TRIGGER IF EXISTS update_employee;

DELIMITER $$
USE `employee_db`` $$
CREATE TRIGGER update_employee
	AFTER INSERT ON salary_table
    FOR EACH ROW
BEGIN
	INSERT INTO email_table (id, first_name, last_name, email)
    VALUES (NEW.id, NEW.first_name, NEW.last_name, CONCAT(NEW.first_name, '_', NEW.last_name, '@example.com'));
END $$
DELIMITER ;
```

[back to table of contents](#table-of-contents)

### Events
Example query to create an event
```SQL
DELIMITER $$
CREATE EVENT update_days_since_onboard
ON SCHEDULE EVERY 1 DAY
DO
BEGIN
	UPDATE employee_db.employee_table
	SET `days_since_onboard` = DATEDIFF(CURRENT_DATE(), `onboard_date`)
	WHERE employee_id <> 0
    ;
END $$
DELIMITER ;
```
**NOTE:** A where clause is required when updating a column.


For additional information on event scheduler, click [here](https://dev.mysql.com/doc/refman/8.0/en/events-overview.html#:~:text=MySQL%20Events%20are%20tasks%20that,a%20specific%20date%20and%20time.).

[back to table of contents](#table-of-contents)
