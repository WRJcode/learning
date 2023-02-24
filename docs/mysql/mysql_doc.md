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

|Type	|Storage (Bytes)	|Minimum Value Signed	|Minimum Value Unsigned	|Maximum Value Signed	|Maximum Value Unsigned|
|--------|--------|--------|--------|--------|--------|
|TINYINT	|1	|-128	|0	|127	|255|
|SMALLINT	|2	|-32768	|0	|32767	|65535|
|MEDIUMINT	|3	|-8388608	|0	|8388607	|16777215|
|INT	|4	|-2147483648	|0	|2147483647	|4294967295|
|BIGINT	|8	|-263	|0	|263-1	|264-1


### Fixed-Point Types (Exact Value) - DECIMAL, NUMERIC

```mysql
salary DECIMAL(5,2)
//-999.99 to 999.99.
```

### Floating-Point Types (Approximate Value)

1. MySQL uses four bytes for single-precision values and eight bytes for double-precision values.**(MySQL对单精度值使用四个字节，对双精度值使用八个字节。)**
2. A precision from 0 to 23 results in a 4-byte single-precision FLOAT column. A precision from 24 to 53 results in an 8-byte double-precision DOUBLE column.

### Bit-Value Type - BIT

1. A type of BIT(M) enables storage of M-bit values. M can range from 1 to 64.


### Numeric Type Attributes

1. MySQL supports an extension for optionally specifying the display width of integer data types in parentheses following the base keyword for the type.For example, a column specified as SMALLINT(3) has the usual SMALLINT range of -32768 to 32767, and values outside the range permitted by three digits are displayed in full using more than three digits.
2. When used in conjunction with the optional (nonstandard) ZEROFILL attribute, the default padding of spaces is replaced with zeros. For example, for a column declared as INT(4) ZEROFILL, a value of 5 is retrieved as 0005.
3. For example, applications can use the LPAD() function to zero-pad numbers up to the desired width, or they can store the formatted numbers in CHAR columns.
4. Integer or floating-point data types can have the AUTO_INCREMENT attribute. When you insert a value of NULL into an indexed AUTO_INCREMENT column, the column is set to the next sequence value. Typically this is value+1, where value is the largest value for the column currently in the table. (AUTO_INCREMENT sequences begin with 1.)
Storing 0 into an AUTO_INCREMENT column has the same effect as storing NULL, unless the NO_AUTO_VALUE_ON_ZERO SQL mode is enabled.
5. Inserting NULL to generate AUTO_INCREMENT values requires that the column be declared NOT NULL.
6. Negative values for AUTO_INCREMENT columns are not supported.
7. CHECK constraints cannot refer to columns that have the AUTO_INCREMENT attribute, nor can the AUTO_INCREMENT attribute be added to existing columns that are used in CHECK constraints.

### Out-of-Range and Overflow Handling


