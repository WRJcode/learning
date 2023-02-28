# 从Mysql官方文档找到的蛛丝马迹!

## Numeric Data Type

### Numeric Data Type Syntax

1. BIT[(M)]
2. TINYINT[(M)] [UNSIGNED] [ZEROFILL]
3. BOOL, BOOLEAN
4. SMALLINT[(M)] [UNSIGNED] [ZEROFILL]
5. MEDIUMINT[(M)] [UNSIGNED] [ZEROFILL]
6. INT[(M)] [UNSIGNED] [ZEROFILL]
7. INTEGER[(M)] [UNSIGNED] [ZEROFILL]
8. BIGINT[(M)] [UNSIGNED] [ZEROFILL]
9. DECIMAL[(M[,D])] [UNSIGNED] [ZEROFILL]
10. DEC[(M[,D])] [UNSIGNED] [ZEROFILL], NUMERIC[(M[,D])] [UNSIGNED] [ZEROFILL], FIXED[(M[,D])] [UNSIGNED] [ZEROFILL]
11. FLOAT[(M,D)] [UNSIGNED] [ZEROFILL]
12. FLOAT(p) [UNSIGNED] [ZEROFILL]
13. DOUBLE[(M,D)] [UNSIGNED] [ZEROFILL]
14. DOUBLE PRECISION[(M,D)] [UNSIGNED] [ZEROFILL], REAL[(M,D)] [UNSIGNED] [ZEROFILL]


### Integer Types (Exact Value)

| Type      | Storage (Bytes) | Minimum Value Signed | Minimum Value Unsigned | Maximum Value Signed | Maximum Value Unsigned |
| --------- | --------------- | -------------------- | ---------------------- | -------------------- | ---------------------- |
| TINYINT   | 1               | -128                 | 0                      | 127                  | 255                    |
| SMALLINT  | 2               | -32768               | 0                      | 32767                | 65535                  |
| MEDIUMINT | 3               | -8388608             | 0                      | 8388607              | 16777215               |
| INT       | 4               | -2147483648          | 0                      | 2147483647           | 4294967295             |
| BIGINT    | 8               | -263                 | 0                      | 263-1                | 264-1                  |


### Fixed-Point Types (Exact Value) - DECIMAL, NUMERIC

```mysql
salary DECIMAL(5,2)
//-999.99 to 999.99.
```

### Floating-Point Types (Approximate Value)

1. MySQL uses four bytes for single-precision values and eight bytes for double-precision values.**(MySQL对单精度值使用四个字节，对双精度值使用八个字节。)**
2. A precision from 0 to 23 results in a 4-byte single-precision FLOAT column. A precision from 24 to 53 results in an 8-byte double-precision DOUBLE column.**(从0到23的精度导致4字节单精度FLOAT列。从24到53的精度导致8字节双精度double列。)**

### Bit-Value Type - BIT

1. A type of BIT(M) enables storage of M-bit values. M can range from 1 to 64.


### Numeric Type Attributes

1. MySQL supports an extension for optionally specifying the display width of integer data types in parentheses following the base keyword for the type.For example, a column specified as SMALLINT(3) has the usual SMALLINT range of -32768 to 32767, and values outside the range permitted by three digits are displayed in full using more than three digits.**(MySQL支持一个扩展，可以选择在类型的基关键字后面的括号中指定整数数据类型的显示宽度。例如，指定为SMALLINT（3）的列的SMALLINT范围通常为-32768到32767，超出三位数允许范围的值将使用三位数以上的数字完整显示。)**
2. When used in conjunction with the optional (nonstandard) ZEROFILL attribute, the default padding of spaces is replaced with zeros. For example, for a column declared as INT(4) ZEROFILL, a value of 5 is retrieved as 0005.**(与可选（非标准）ZEROFILL属性一起使用时，空格的默认填充将替换为零。例如，对于声明为INT（4）ZEROFILL的列，值5将检索为0005。)**
3. For example, applications can use the LPAD() function to zero-pad numbers up to the desired width, or they can store the formatted numbers in CHAR columns.**(例如，应用程序可以使用LPAD（）函数将数字调零到所需的宽度，也可以将格式化的数字存储在CHAR列中。)**
4. Integer or floating-point data types can have the AUTO_INCREMENT attribute. When you insert a value of NULL into an indexed AUTO_INCREMENT column, the column is set to the next sequence value. Typically this is value+1, where value is the largest value for the column currently in the table. (AUTO_INCREMENT sequences begin with 1.)
Storing 0 into an AUTO_INCREMENT column has the same effect as storing NULL, unless the NO_AUTO_VALUE_ON_ZERO SQL mode is enabled.
5. Inserting NULL to generate AUTO_INCREMENT values requires that the column be declared NOT NULL.
6. Negative values for AUTO_INCREMENT columns are not supported.
7. CHECK constraints cannot refer to columns that have the AUTO_INCREMENT attribute, nor can the AUTO_INCREMENT attribute be added to existing columns that are used in CHECK constraints.

### Out-of-Range and Overflow Handling

1. If strict SQL mode is enabled, MySQL rejects the out-of-range value with an error, and the insert fails, in accordance with the SQL standa
2. If no restrictive modes are enabled, MySQL clips the value to the appropriate endpoint of the column data type range and stores the resulting value instead.

When an out-of-range value is assigned to an integer column, MySQL stores the value representing the corresponding endpoint of the column data type range.

When a floating-point or fixed-point column is assigned a value that exceeds the range implied by the specified (or default) precision and scale, MySQL stores the value representing the corresponding endpoint of that range.

3. Overflow during numeric expression evaluation results in an error.
4. Whether overflow occurs depends on the range of the operands, so another way to handle the preceding expression is to use exact-value arithmetic because DECIMAL values have a larger range than integers
5. Subtraction between integer values, where one is of type UNSIGNED, produces an unsigned result by default. If the result would otherwise have been negative, an error results.If the NO_UNSIGNED_SUBTRACTION SQL mode is enabled, the result is negative:
   
 ```sql
 
 mysql> SET sql_mode = '';
 Query OK, 0 rows affected (0.00 sec)
 mysql> SELECT CAST(0 AS UNSIGNED) - 1;
 ERROR 1690 (22003): BIGINT UNSIGNED value is out of range in '(cast(0 as unsigned) - 1)'

 ```


 ```sql

 mysql> SET sql_mode = 'NO_UNSIGNED_SUBTRACTION';
 mysql> SELECT CAST(0 AS UNSIGNED) - 1;
 +-------------------------+
 | CAST(0 AS UNSIGNED) - 1 |
 +-------------------------+
 |                      -1 |
 +-------------------------+

 ```

 6. If the result of such an operation is used to update an UNSIGNED integer column, the result is clipped to the maximum value for the column type, or clipped to 0 if NO_UNSIGNED_SUBTRACTION is enabled. If strict SQL mode is enabled, an error occurs and the column remains unchanged.

## Date and Time Data Types

1. “Zero” date or time values used through Connector/ODBC are converted automatically to NULL because ODBC cannot handle such values.
2. 
### Date and Time Data Type Syntax
1. The date and time data types for representing temporal values are **DATE, TIME, DATETIME, TIMESTAMP, and YEAR.**
2. MySQL permits fractional seconds for TIME, DATETIME, and TIMESTAMP values, with up to microseconds (6 digits) precision. To define a column that includes a fractional seconds part, use the syntax type_name(fsp), where type_name is **TIME, DATETIME, or TIMESTAMP**, and ***fsp*** is the fractional seconds precision. For example:

```sql

 CREATE TABLE t1 (t TIME(3), dt DATETIME(6), ts TIMESTAMP(0));

```
3. A date. The supported range is '1000-01-01' to '9999-12-31'. MySQL displays DATE values in 'YYYY-MM-DD' format, but permits assignment of values to DATE columns using **either strings or numbers.**
4. A date and time combination. The supported range is '1000-01-01 00:00:00.000000' to '9999-12-31 23:59:59.999999'. MySQL displays DATETIME values in 'YYYY-MM-DD hh:mm:ss[.fraction]' format, but permits assignment of values to DATETIME columns using **either strings or numbers.**
5. A timestamp. The range is '1970-01-01 00:00:01.000000' UTC to '2038-01-19 03:14:07.999999' UTC. 
6. If explicit_defaults_for_timestamp is enabled, there is no automatic assignment of the DEFAULT CURRENT_TIMESTAMP or ON UPDATE CURRENT_TIMESTAMP attributes to any TIMESTAMP column. They must be included explicitly in the column definition. Also, any TIMESTAMP not explicitly declared as NOT NULL permits NULL values.
7. A time. The range is '-838:59:59.000000' to '838:59:59.000000'. MySQL displays TIME values in 'hh:mm:ss[.fraction]' format, but permits assignment of values to TIME columns using either strings or numbers.
8. A year in 4-digit format. MySQL displays YEAR values in YYYY format, but permits assignment of values to YEAR columns using either strings or numbers. Values display as 1901 to 2155, or 0000.
9. The SUM() and AVG() aggregate functions do not work with temporal values. (They convert the values to numbers, losing everything after the first nonnumeric character.) To work around this problem, convert to numeric units, perform the aggregate operation, and convert back to a temporal value. Examples:

```sql
 
  SELECT SEC_TO_TIME(SUM(TIME_TO_SEC(time_col))) FROM tbl_name;
  SELECT FROM_DAYS(SUM(TO_DAYS(date_col))) FROM tbl_name;

```

### The DATE, DATETIME, and TIMESTAMP Types

1. MySQL converts TIMESTAMP values from the current time zone to UTC for storage, and back from UTC to the current time zone for retrieva.**(MySQL将TIMESTAMP值从当前时区转换为UTC进行存储，并将其从UTC转换为当前时区进行检索)**
2.  By default, the current time zone for each connection is the server's time.
3.  

### The TIME Type
1. TIME values may range from '-838:59:59' to '838:59:59'
2. The hours part may be so large because the TIME type can be used not only to represent a time of day (which must be less than 24 hours), but also elapsed time or a time interval between two events (which may be much greater than 24 hours, or even negative).
3. MySQL recognizes TIME values in several formats, some of which can include a trailing fractional seconds part in up to microseconds (6 digits) precision.
4. '11:12' means '11:12:00', not '00:11:12'.For example, you might think of '1112' and 1112 as meaning '11:12:00' (12 minutes after 11 o'clock), but MySQL interprets them as '00:11:12' (11 minutes, 12 seconds). Similarly, '12' and 12 are interpreted as '00:00:12'.
5.  For example, '-850:00:00' and '850:00:00' are converted to '-838:59:59' and '838:59:59'. Invalid TIME values are converted to '00:00:00'.

### The YEAR Type
1. The YEAR type is a 1-byte type used to represent year values. It can be declared as YEAR with an implicit display width of 4 characters, or equivalently as YEAR(4) with an explicit display width.

### Automatic Initialization and Updating for TIMESTAMP and DATETIME

 ```sql

 CREATE TABLE t1 (
  ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  dt DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
 );

 CREATE TABLE t1 (
  ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  dt DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE t1 (
  ts TIMESTAMP DEFAULT 0,
  dt DATETIME DEFAULT 0
);

CREATE TABLE t1 (
  ts TIMESTAMP DEFAULT 0 ON UPDATE CURRENT_TIMESTAMP,
  dt DATETIME DEFAULT 0 ON UPDATE CURRENT_TIMESTAMP
);

CREATE TABLE t1 (
  ts1 TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,     -- default 0
  ts2 TIMESTAMP NULL ON UPDATE CURRENT_TIMESTAMP -- default NULL
);

CREATE TABLE t1 (
  dt1 DATETIME ON UPDATE CURRENT_TIMESTAMP,         -- default NULL
  dt2 DATETIME NOT NULL ON UPDATE CURRENT_TIMESTAMP -- default 0
);

CREATE TABLE t1 (
  ts1 TIMESTAMP DEFAULT 0,
  ts2 TIMESTAMP DEFAULT CURRENT_TIMESTAMP
                ON UPDATE CURRENT_TIMESTAMP);
CREATE TABLE t2 (
  ts1 TIMESTAMP NULL,
  ts2 TIMESTAMP DEFAULT CURRENT_TIMESTAMP
                ON UPDATE CURRENT_TIMESTAMP);
CREATE TABLE t3 (
  ts1 TIMESTAMP NULL DEFAULT 0,
  ts2 TIMESTAMP DEFAULT CURRENT_TIMESTAMP
                ON UPDATE CURRENT_TIMESTAMP);

CREATE TABLE t
(
  ts1 TIMESTAMP NULL DEFAULT NULL,
  ts2 TIMESTAMP NULL DEFAULT 0,
  ts3 TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP
);

 ```

### Fractional Seconds in Time Values

**MySQL has fractional seconds support for TIME, DATETIME, and TIMESTAMP values, with up to microseconds (6 digits) precision:** ***use the syntax type_name(fsp)***

```sql

 CREATE TABLE t1 (t TIME(3), dt DATETIME(6));

```

1. Inserting a TIME, DATE, or TIMESTAMP value with a fractional seconds part into a column of the same type but having fewer fractional digits results in rounding. Consider a table created and populated as follows:

  ```sql

   CREATE TABLE fractest( c1 TIME(2), c2 DATETIME(2), c3 TIMESTAMP(2) );
   INSERT INTO fractest VALUES
   ('17:51:04.777', '2018-09-08 17:51:04.777', '2018-09-08 17:51:04.777');


    mysql> SELECT * FROM fractest;
    +-------------+------------------------+------------------------+
    | c1          | c2                     | c3                     |
    +-------------+------------------------+------------------------+
    | 17:51:04.78 | 2018-09-08 17:51:04.78 | 2018-09-08 17:51:04.78 |
    +-------------+------------------------+------------------------+

  ```

2. To insert the values with truncation instead, enable the TIME_TRUNCATE_FRACTIONAL SQL mode:

  ```sql
   
   SET @@sql_mode = sys.list_add(@@sql_mode, 'TIME_TRUNCATE_FRACTIONAL');

   mysql> SELECT * FROM fractest;
    +-------------+------------------------+------------------------+
    | c1          | c2                     | c3                     |
    +-------------+------------------------+------------------------+
    | 17:51:04.77 | 2018-09-08 17:51:04.77 | 2018-09-08 17:51:04.77 |
    +-------------+------------------------+------------------------+

  ```

3. Functions that take temporal arguments accept values with fractional seconds. Return values from temporal functions include fractional seconds as appropriate. For example, NOW() with no argument returns the current date and time with no fractional part, but takes an optional argument from 0 to 6 to specify that the return value includes a fractional seconds part of that many digits.



### Conversion Between Date and Time Types

1. Explicit conversion can be used to override implicit conversion. For example, in comparison of DATE and DATETIME values, the DATE value is coerced to the DATETIME type by adding a time part of '00:00:00'. To perform the comparison by ignoring the time part of the DATETIME value instead, use the CAST() function in the following way:

  ```sql
   date_col = CAST(datetime_col AS DATE)
  ```

2. Conversion of TIME and DATETIME values to numeric form (for example, by adding +0) depends on whether the value contains a fractional seconds part. TIME(N) or DATETIME(N) is converted to integer when N is 0 (or omitted) and to a DECIMAL value with N decimal digits when N is greater than 0:
   
   ```sql
    mysql> SELECT CURTIME(), CURTIME()+0, CURTIME(3)+0;
      +-----------+-------------+--------------+
      | CURTIME() | CURTIME()+0 | CURTIME(3)+0 |
      +-----------+-------------+--------------+
      | 09:28:00  |       92800 |    92800.887 |
      +-----------+-------------+--------------+
      mysql> SELECT NOW(), NOW()+0, NOW(3)+0;
      +---------------------+----------------+--------------------+
      | NOW()               | NOW()+0        | NOW(3)+0           |
      +---------------------+----------------+--------------------+
      | 2012-08-15 09:28:00 | 20120815092800 | 20120815092800.889 |
      +---------------------+----------------+--------------------+
  ```

### 2-Digit Years in Dates

 Date values with 2-digit years are ambiguous because the century is unknown. Such values must be interpreted into 4-digit form because MySQL stores years internally using 4 digits.

 For DATETIME, DATE, and TIMESTAMP types, MySQL interprets dates specified with ambiguous year values using these rules:

  * Year values in the range 00-69 become 2000-2069.

  * Year values in the range 70-99 become 1970-1999.

## String Data Types
 
  
  * String Data Type Syntax
  * CHAR and VARCHAR Types
  * The BINARY and VARBINARY Types
  * The BLOB and TEXT Types
  * The ENUM Type
  * The ENUM Type
  * The SET Type

### String Data Type Syntax

 1. For definitions of character string columns (CHAR, VARCHAR, and the TEXT types), MySQL interprets length specifications in ***character units***. For definitions of binary string columns (BINARY, VARBINARY, and the BLOB types), MySQL interprets length specifications in ***byte units***.

 2. Column definitions for character string data types CHAR, VARCHAR, the TEXT types, ENUM, SET, and any synonyms) can specify the column character set and collation:
  * CHARACTER SET specifies the character set. If desired, a collation for the character set can be specified with the COLLATE attribute, along with any other attributes. For example:
   ```sql 
    CREATE TABLE t
      (
          c1 VARCHAR(20) CHARACTER SET utf8mb4,
          c2 TEXT CHARACTER SET latin1 COLLATE latin1_general_cs
      );
   ```
  
  * Specifying the CHARACTER SET binary attribute for a character string data type causes the column to be created as the corresponding binary string data type: CHAR becomes BINARY, VARCHAR becomes VARBINARY, and TEXT becomes BLOB. For the ENUM and SET data types, this does not occur; they are created as declared. Suppose that you specify a table using this definition:

   ```sql
    CREATE TABLE t
    (
      c1 VARCHAR(10) CHARACTER SET binary,
      c2 TEXT CHARACTER SET binary,
      c3 ENUM('a','b','c') CHARACTER SET binary
    );

    The resulting table has this definition:

    CREATE TABLE t
    (
      c1 VARBINARY(10),
      c2 BLOB,
      c3 ENUM('a','b','c') CHARACTER SET binary
    );

   ```
  * The BINARY attribute is a nonstandard MySQL extension that is shorthand for specifying the binary (_bin) collation of the column character set (or of the table default character set if no column character set is specified). In this case, comparison and sorting are based on numeric character code values. Suppose that you specify a table using this definition:
  
   ```sql
    CREATE TABLE t
    (
      c1 VARCHAR(10) CHARACTER SET latin1 BINARY,
      c2 TEXT BINARY
    ) CHARACTER SET utf8mb4;

    The resulting table has this definition:

    CREATE TABLE t (
      c1 VARCHAR(10) CHARACTER SET latin1 COLLATE latin1_bin,
      c2 TEXT CHARACTER SET utf8mb4 COLLATE utf8mb4_bin
    ) CHARACTER SET utf8mb4;
   ```

### The CHAR and VARCHAR Types

  
|Value	|CHAR(4)	|**Storage Required**	|VARCHAR(4)	|**Storage Required**|
| --------- | --------------- | -------------------- | ---------------------- | -------------------- |
|''	|'&emsp;&emsp;&emsp;&emsp;'	|4 bytes	|''	|1 byte|
|'ab'	|'ab&emsp;&emsp;'	|4 bytes	|'ab'	|3 bytes|
|'abcd'	|'abcd'	|4 bytes	|'abcd'	|5 bytes|
|'abcdefgh'	|'abcd'	|4 bytes	|'abcd'	|5 bytes|

  ```sql

  mysql> CREATE TABLE vc (v VARCHAR(4), c CHAR(4));
  Query OK, 0 rows affected (0.01 sec)

  mysql> INSERT INTO vc VALUES ('ab  ', 'ab  ');
  Query OK, 1 row affected (0.00 sec)

  mysql> SELECT CONCAT('(', v, ')'), CONCAT('(', c, ')') FROM vc;
  +---------------------+---------------------+
  | CONCAT('(', v, ')') | CONCAT('(', c, ')') |
  +---------------------+---------------------+
  | (ab  )              | (ab)                |
  +---------------------+---------------------+
  1 row in set (0.06 sec)

```

### The BINARY and VARBINARY Types

 ```sql
  mysql> CREATE TABLE t (c BINARY(3));
  Query OK, 0 rows affected (0.01 sec)

  mysql> INSERT INTO t SET c = 'a';
  Query OK, 1 row affected (0.01 sec)

  mysql> SELECT HEX(c), c = 'a', c = 'a\0\0' from t;
  +--------+---------+-------------+
  | HEX(c) | c = 'a' | c = 'a\0\0' |
  +--------+---------+-------------+
  | 610000 |       0 |           1 |
  +--------+---------+-------------+
  1 row in set (0.09 sec)
 ```

### The JSON Data Type


## Functions and Operators

### Built-In Function and Operator Reference

 https://dev.mysql.com/doc/refman/8.0/en/built-in-function-reference.html

 
### Loadable Function Reference
 
 https://dev.mysql.com/doc/refman/8.0/en/loadable-function-reference.html

### Operator Precedence

  https://dev.mysql.com/doc/refman/8.0/en/non-typed-operators.html

 #### Operator Precedence

 ```sql
  INTERVAL
  BINARY, COLLATE
  !
  - (unary minus), ~ (unary bit inversion)
  ^
  *, /, DIV, %, MOD
  -, +
  <<, >>
  &
  |
  = (comparison), <=>, >=, >, <=, <, <>, !=, IS, LIKE, REGEXP, IN, MEMBER OF
  BETWEEN, CASE, WHEN, THEN, ELSE
  NOT
  AND, &&
  XOR
  OR, ||
  = (assignment), :=
 ```

 #### Comparison Functions and Operators

  https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html

#### Logical Operators

 https://dev.mysql.com/doc/refman/8.0/en/logical-operators.html

#### Assignment Operators

 https://dev.mysql.com/doc/refman/8.0/en/assignment-operators.html

### Flow Control Functions

 
|Name	|Description|
|-----|-----------|
|CASE	|Case operator|
|IF()	|If/else construct|
|IFNULL()	|Null if/else construct|
|NULLIF()	|Return NULL if expr1 = expr2|
