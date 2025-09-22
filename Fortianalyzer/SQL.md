==Structured Query Language==

==A dataset is an SQL select query==
![[Pasted image 20241017011614.png]]

In SQL there is two types of tables
User tables, which contain information that is in the database
System tables, which contain the database descriptio

SQL contains the next verbs for querying data:
- Select
- Insert
- Update
- Delete

But in fortianalyzer you use only select

### Select statement
FROM, which specifies the table.
• WHERE, which specifies the conditions. All rows that do not satisfy the condition are eliminated from the
output.
• GROUP BY, which collects data across multiple records and groups the results by one or more columns.
• ORDER BY, which orders the results by rows. If ORDER BY is not given, the rows are returned in
whatever order the system finds the fastest to produce. And finally,
• LIMIT, which limits the number of records returned based on a specified value. OFFSET is another clause
often used along with LIMIT, which offset the results by the number specified. For example, if you place a
limit of three records and an offset of one, the first record that would normally be returned is skipped and
instead the second, third, and fourth records (three in total) are returned.




### Where

Where excludes data that does not comply with the condition 
![[Pasted image 20241017013446.png]]

Applliable operands are AND, OR, NOT IN

==The filter is stored in $filter==


### Group by
Commonly used woth aggregate functions, without it its similar to distinct, which returns aggregated  unique rows

Group by is used to group results based on a set of values to return a single row, this since select returns a row for every record

For instance, group by dst_ip, will group results based in their dst ip

### Order by
![[Pasted image 20241017013952.png]]

Order by is used to order results based on alphabetical or numerical value
Can do from column, to column number  and asc or desc

### Limit and offset
![[Pasted image 20241017014109.png]]

Limit ; limits the number of rows returned, useful when making big queries that may impact cpu usage

Offset skips the given number of rows, so if it was to start at 1, it would start at n 

### Aggregate functions
SQL also has aggregate functions, which operate over the entire dataset rather than every single row like a normal function. (You cant use functions on where but you can use them on having)

##### NULLIF
Select NULLIF(exp1,exp2) - (same datatype values)

This function returns null if both arguments are equal, if not returns the first value

==Null is not zero==

##### COALESCE
Returns the first non null value from the given args, cant be strings

COALESCE (expression 1, expression 2, …)

##### More functions

![[Pasted image 20241017020539.png]]


### Operators

##### Arithmetic
![[Pasted image 20241017020714.png]]

##### Comparision
![[Pasted image 20241017020753.png]]


##### Logical operators
![[Pasted image 20241017020911.png]]
