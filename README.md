# MySQL Cheat Sheet for Data Analysis
 This project compiles a list of common MySQL commands for data analysis with brief descriptions and example syntax.

## Table of Contents

[**Basic Commands**](#basic-commands)
* [Retrieving Data](#retrieving-data)
* [Sorting and Filtering Data](#sorting-and-filtering-data)
* [Aggregating Data](#aggregating-data)
* [Aliasing](#aliasing)
* [Grouping Data](#grouping-data)

[**Data Wrangling**](#data-wrangling)
* [String Functions](#string-functions)
* [Handling Null Values](#handling-null-values)
* [Date and Time Functions ](#date-and-time-functions)
  * [Current Date and Time](#current-date-and-time)
  * [Date](#date)
  * [Time](#time)
* [Conditional Statements](#conditional-statements) (`CASE`)

[**Advanced Commands**](#advanced-commands)
* [Subqueries](#subqueries)
* [Window Functions](#window-functions) (`OVER`, `PARTITION BY`)
* [Ranking Data]() (`ROWNUMBER`, `RANK`, `DENSE_RANK`)
* [Merging Data]() (`JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `CROSS JOIN`, `UNION`)



## Basic Commands

### Retrieving Data
| Command  | Description                              | Example Query           |
| -------- | ---------------------------------------- | ----------------------- |
| `SELECT` | Selects and display a value              | `SELECT 'random_string'`|
| `FROM`   | Used with `SELECT` to query from a table | `SELECT * FROM table1` - displays all columns |
|          |                                          | `SELECT name, age FROM table1` - displays name and age |

[back to table of contents](#table-of-contents)

### Sorting and Filtering Data
| Command    | Description                      | Example Query |
| ---------- | -------------------------------- | ------------- |
| `ORDER BY` | Sorts rows by a specified column. **NOTE:** Ascenbding is the default order, so no need to explicitly type `ASC` | `SELECT * FROM table1 ORDER BY age` - displays data sorted by increasing age |
|            | Use `DESC` for descending order  | `SELECT * FROM table1 ORDER BY salary DESC` - displays data sorted by decreasing salary |
| `WHERE`    | Filters data using a specific condition  | `SELECT name, job_position FROM table1 WHERE age >= 50` - displays names and job posiitons where age is at least 50 |
| `BETWEEN`  | Determine whether data falls between a specified range, inclusive | `SELECT * FROM table1 WHERE salary BETWEEN 30000 AND 50000` - Display rows where salary is between 30000 and 50000, inclusive |
| `LIKE`     | Displays approximate match       | `SELECT * FROM table1 WHERE job_position LIKE 'Junior%'` - displays rows where job position starts with "Junior" followed by any number of characters |
|            |                                  | `SELECT * FRTOM table1 WHERE name LIKE 'A__'` - displays rows where name starts with A andf followed by exactly two characters |
| `LIMIT`    | Displays the top rows            | `SELECT * FROM table1 LIMIT 10` - displays top 10 rows |
| `OFFSET`   | Offsets `LIMIT` by a specified number | `SELECT * FROM table1 LIMIT 10 OFFSET 2` - displays top 10 rows starting from the 2nd row (displays 3rd to 12th row) |

[back to table of contents](#table-of-contents)

### Aggregating Data
| Command    | Description                                | Example Query           |
| ---------- | ------------------------------------------ | ----------------------- |
| `DISTINCT` | Displays unique values of a column         | `SELECT DISTINCT job_position FROM table1` - displays all unique values of job posiiton |
| `COUNT`    | Counts the number of rows from selection   | `SELECT COUNT(*) FROM table1 WHERE salary > 50000` - displays number of rows having salary values over 50000 |
|            | Use with `DISTINCT` to count unique values | `SELECT COUNT(DISTINCT job_position) FROM table1 WHERE age >= 60` - displays number of unique departments with at least one employee having age of at least 60 |
| `SUM`      | Displays the sum of values from selection  | `SELECT SUM(salary) FROM table1` - calculates total salary from all rows |
|            |                                            | `SELECT SUM(salary) FROM table1 WHERE age >= 60` - calculates total salary from all employees having age of at least 60 |
| `MIN`      | Displays minimum value from selection      | `SELECT MIN(salary) FROM table1` - displays minimum salary of all employees |
| `MAX`      | Displays maximum value from selection      | `SELECT MAX(salary) FROM table1 WHERE gender = 'Female'` - displays maximum salary of all female employees |
| `AVG`      | Calculates average value from selection    | `SELECT AVG(salary) FROM table1` - calcuates the average salary of all employees |

[back to table of contents](#table-of-contents)

### Aliasing
| Command | Description               | Example Query           |
| ------- | ------------------------- | ----------------------- |
| `AS`    | Renames a column or table | `SELECT DISTINCT department AS department_list FROM table1` - displays unique values of departments into a column named department_list |

**NOTE:** Aliasing is useful in shortening column names for easier referencing and readability. Aliasing is also required in [subqueries](#subqueries).

[back to table of contents](#table-of-contents)

### Grouping Data
| Command    | Description               | Example Query           |
| ---------- | ------------------------- | ----------------------- |
| `GROUP BY` | Groups data by a specified column for using [aggregate functions](#aggregating-data) | `SELECT gender, COUNT(*) FROM table1 GROUP BY gender` - displays values for gender column and number of employees for each gender value |
| `HAVING`   | Similar to `WHERE` but used when filtering grouped data | `SELECT job_position, AVG(salary) FROM table1 GROUP BY job_position HAVING AVG(salary) > 50000` Displays each job position with respective average salary from job positions having average salary values over 50000 |
|            |                           | `SELECT job_position, AVG(salary) FROM table1 GROUP BY job_position HAVING job_position LIKE 'Junior%'` - displays average salary for each junior position |

**NOTE:** `GROUP BY` is first executed before filtering with `HAVING`; therefore `HAVING` cannot be used to filter data before grouping. [Subqueries](#subqueries) should be used to filter data before grouping [(see example)](#using-a-subquery-to-filter-data-before-grouping).

[back to table of contents](#table-of-contents)


## Data Wrangling

### String Functions
| Command     | Description               | Example Query           | Return Value |
| ----------- | ------------------------- | ----------------------- | ------------ |
| `LENGTH`    | Dsiplays the character length of the string | `SELECT LENGTH('Example text')` | `12` |
| `UPPER`     | Displays string in all upper case | `SELECT UPPER('Example text')` | `'EXAMPLE TEXT'` |
| `LOWER`     | Displays string in all lower case | `SELECT LOWER('Example text')` | `'example text'` |
| `TRIM`      | Removes whitespaces from both sides | `SELECT TRIM('  Text  ')` | `'Text'` | 
| `LTRIM`     | Removes whitespaces from the left side | `SELECT LTRIM('  Text  ')` | `'Text  '` |
| `RTRIM`     | emoves whitespaces from the right side | `SELECT RTRIM('  Text  ')` | `'  Text'` |
| `LEFT`      | Displays the first n characters of the string | `SELECT LEFT('Example Text', 4)` | `'Exam'` |
| `RIGHT`     | Displays the last n characters of the string | `SELECT RIGHT('Example Text', 4)` | `'Text'` |
| `LOCATE`    | Displays the index of the first occurence of the specified character | `SELECT LOCATE('a', 'Banana')` | `2` |
| `SUBSTRING` | Displays substring using a specified starting index and substring length | `SELECT SUBSTRING('1991-04-30', 6, 2)` | `'04'` |
| `REPLACE`   | Replaces one substring A from a string with another substring B | `SELECT REPLACE('Banana', 'na', 'l')` | `'Ball'` |
| `CONCAT`    | Concatenates strings in specified order | `SELECT CONCAT('first_name', '.', 'last_name', '@example.com')` | `'first_name.last_name@example.com'` |

[back to table of contents](#table-of-contents)

### Handling Null Values
| Command  | Description                              | Example Query           |
| -------- | ---------------------------------------- | ----------------------- |
| `ISNULL` | Checks if the value of a field is `NULL` | `SELECT * FROM table1 WHERE ISNULL(department)` - displays rows with missing department values |

[back to table of contents](#table-of-contents)

### Date and Time Functions

#### Current Date and Time
| Command            | Description                                          | Example Query               |
| ------------------ | ---------------------------------------------------- | --------------------------- |
| `CURRENTDATE`      | Displays current date (YYYY-MM-DD)                   | `SELECT CURRENTDATE()`      |
| `CURRENTTIME`      | Displays current time (HH:MM:SS)                     | `SELECT CURRENTTIME()`      |
| `CURRENTTIMESTAMP` | Displays current date and time (YYYY-MM-DD HH:MM:SS) | `SELECT CURRENTTIMESTAMP()` |

[back to table of contents](#table-of-contents)

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

[back to table of contents](#table-of-contents)

#### Time
| Command  | Description                          | Example Query                        | Return Value |
| -------- | ------------------------------------ | ------------------------------------ | ------------ |
| `TIME`   | Extracts time from a datetime string | `SELECT TIME('2024-12-25 12:45:00')` | `12:45:00`   |
|          | Extract time from a timestamp        | `SELECT TIME(CURRENTTIMESTAMP())`    | `18:51:00`   |
| `HOUR`   | Extract hour (0-23) of a time        | `SELECT HOUR(TIME(12:45:00))`        | `12`         |
|`MINUTE`  | Extract minute from a time           | `SELECT MINUTE(TIME(12:45:00))`      | `45`         |
| `SECOND` | Extract seconds froma  time          | `SELECT SECOND(TIME(12:45:00))`      | `0`          |

[back to table of contents](#table-of-contents)

#### Date and Time Formatting
##### Date formats
Example uses the timestamp `2024-12-25 13:45:00`:
| Format string    | Description | Example output |
| ---------------- | ----------- | ------- |
| `%Y`             | Year (4 digit)
| `%y`             | Year (2 digit)
| `%m` | Month number (01-12)
| `%c` | Month number (1-12)
| `%M` | Month name
| `%b` | Month name (3-letter)
%D	Day of the month as a numeric value, followed by suffix (1st, 2nd, 3rd, ...)
%d	Day of the month as a numeric value (01 to 31)
%e	Day of the month as a numeric value (0 to 31)
%a	Abbreviated weekday name (Sun to Sat)
%j	Day of the year (001 to 366)
%W	Weekday name in full (Sunday to Saturday)
%w	Day of the week where Sunday=0 and Saturday=6
%U	Week where Sunday is the first day of the week (00 to 53)
%u	Week where Monday is the first day of the week (00 to 53)
%V	Week where Sunday is the first day of the week (01 to 53). Used with %X
%v	Week where Monday is the first day of the week (01 to 53). Used with %x
%X	Year for the week where Sunday is the first day of the week. Used with %V
%x	Year for the week where Monday is the first day of the week. Used with %v

##### Time formats
Example uses the timestamp `2024-12-25 13:45:00`:
| Format           | Description                     | Example output |
| ---------------- | ------------------------------- | -------------- |
| `%T`             | hh:mm:ss (24-hour)              | `'13:45:00'`
| `%r`             | hh:mm:ss AM/PM (12 hour)        | `'1:45:00 PM'`
| `%H`, `%k`       | Hour (00-23)                    | `13`
| `%h`, `%I`, `%l` | Hour (01-12)                    | `1`
| `%p`             | AM or PM                        | `
| `%i`             | Minutes (00 to 59)              |
| `%S`, `%s`       | Seconds (00 to 59)              |
| `%f`             | Microseconds (000000 to 999999) |

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
SELECT name, salary,
CASE
    WHEN salary > 50000 THEN 'high'
    WHEN salary > 30000 THEN 'medium'
    ELSE 'low'
END
AS salary_bracket
FROM table1
```
**NOTE:** If none of the `WHEN` conditions are met and no `ELSE` is stated, the field will be assigned to `NULL`.

[back to table of contents](#table-of-contents)


## Advanced Commands

### Subqueries

#### Using a subquery to filter data before grouping

[back to table of contents](#table-of-contents)

## Window Functions

[back to table of contents](#table-of-contents)

## Ranking Data
Ranking data uses the `OVER` command as a [window function](#window-functions).

[back to table of contents](#table-of-contents)

## Merging Data

[back to table of contents](#table-of-contents)


