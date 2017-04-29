# sql

*************************************************************************************************************

*************************************************************************************************************
what is DEFAULT feature in SQL?
Ans: default is used with insert or update statements . 
	Ex: update emp set comm = default where comm is null
*************************************************************************************************************
How to update two columns?
Ans: Using subqueries we can do this.
	Ex:update emp10 set sal = (select sal from emp where job = 'PRESIDENT'), 
						comm = (select max(comm) from emp);
*************************************************************************************************************
How to copy rows from another table?
Ans: insert into emp10 select * from emp where deptno = 10; 
		Note:
		To run the above command emp10 table should exist so execute the below command first.
		
		create table emp10 as select * from emp where 1=2; -- creates table without copying data by using the false condition.
*************************************************************************************************************
what are conversion functions ?
Ans: There are two types of convertions .
	i)Implicit conversion : varchar2 to Number or Date.
	ii)Explicit conversion : varchar2 to Number or Date and vice versa.
	Explicit conversion happens through to_number,to_char and to_date functions.
	
	Elements of the Date Format Model :
	YYYY	Full year in numbers
	YEAR	Year spelled out
	MM		Two-digit value for month
	MONTH	Full name of the month
	MON		Three-letter abbreviation of the month
	DY		Three-letter abbreviation of the day of the week
	DAY		Full name of the day of the week
	DD 		Numeric day of the month
	Example:
	Time elements format the time portion of the date.
	• Add character strings by enclosing them in double quotation marks.
	• Number suffixes spell out numbers.
		HH24:MI:SS AM --->15:45:32 PM
		DD "of" MONTH --->12 of OCTOBER
		ddspth --->fourteenth
	
	a) TO_CHAR function with Date.
	Syntax: TO_CHAR(date, 'format__model')
	The format model:
	• Must be enclosed in single quotation marks and is case sensitive
	• Can include any valid date format element
	• Has an fm element to remove padded blanks or suppress leading zeros
	Ex: SELECT ename,hiredate, TO_CHAR(hiredate, 'fmDD Month YYYY') hiredate2 FROM emp;
	b)TO_CHAR with numbers
	Syntax: TO_CHAR(number, 'format__model')
	format elements:
	----------------
	9	Represents a number
	0	Forces a zero to be displayed
	$	Places a floating dollar sign
	L	Uses the floating local currency symbol
	.	Prints a decimal point
	,	Prints a thousand indicator
	
	Ex: SELECT TO_CHAR(sal, '$99,999.00') SALARY FROM emp;
	
	c)TO_NUMBER
		syntax: TO_NUMBER(char[, 'format_model']
		Ex: select to_number('12345.678' , '99999.999') from dual; --
	d)TO_DATE 
	Year gas to formats, YY and RR.
	Note:
	If the year is in the range of 50-99, RR selects previous century. 
	syntax: TO_DATE(char[, 'format_model']
		Ex: i)select to_date('01-jan-87' , 'DD-MM-YY') from dual; --
		o/p : 1/1/2087. 
		ii) select to_date('01-jan-87' , 'DD-MM-RR') from dual;
		o/p : 1/1/1987.
		iii)select to_date('01-jan-45' , 'DD-MM-YY') from dual;
		o/p : 1/1/2045.
		iv) select to_date('01-jan-45' , 'DD-MM-RR') from dual;
		o/p : 1/1/2045.
***************************************************************************************************
what are General functions in oracle?
Ans: There are four general functions.These functions work with any data type and pertain to using null value.
		• NVL (expr1, expr2)
		• NVL2 (expr1, expr2, expr3)
		• NULLIF (expr1, expr2)
		• COALESCE (expr1, expr2, ..., exprn)
		
		i)NVL:
		• Converts a null to an actual value
		• Data types that can be used are date, character,and number.
		• Data types must match:	
		Ex:
		– NVL(commission_pct,0)
		– NVL(hire_date,'01-JAN-97')
		– NVL(job_id,'No Job Yet')
		-->SELECT ename, sal, (sal*12) + (sal*12*NVL(comm, 0)) AN_SAL FROM emp;
		
		ii)NVL2:
		Ex: SELECT ename, sal, comm,NVL2(comm,'SAL+COMM', 'SAL') income FROM emp;
		O/p prints 'SAL+COMM' if commission is not null, otherwise 'SAL'. 
		
		iii)NULLIF(expr1,expr2) : prints null if expr1 and expr2 are equal. i.e. expr1 and expr2 should result in same data type.
		
		iv)COALESCE : The advantage of the COALESCE function over the NVL function is that the COALESCE function can take multiple alternate values.
		COALESCE is a variety of the CASE expression. For example,
		COALESCE (expr1, expr2)is equivalent to:
			CASE WHEN expr1 IS NOT NULL THEN 
					expr1 
				 ELSE expr2 
				 END
	    Similarly,
			COALESCE (expr1, expr2, ..., exprn), for n>=3
		is equivalent to:
		CASE WHEN expr1 IS NOT NULL THEN 
				expr1 
			 ELSE 
			 COALESCE (expr2, ..., exprn) 
			 END
***************************************************************************************************
-- what are the conditional expressions ?
Ans: There are two condtional expressions
	 i) CASE 
	 ii)DECODE 
	Bothe does the work of an IF-THEN-ELSE statement. The difference  is 'DECODE' is oracle specific,but 'CASE' is ANSI specific.
	
		Syntax: case <col_name>
				when <value1> then
				............................
				when <value2>  then
				...............................
				else 
				....................
				end <alias_name>
		
		Ex:1) select empno,ename,deptno,
				case deptno
				when 10 then 'Accounting'
				when 20 then 'Research'
				when 30  then 'Sales'
				else 'Others'
				end dname
				from emp;
			
			Note: For 'CASE' expression column is not necessary.
			2)select empno,ename,deptno,
				case 
				when sal<nvl(comm,0) then 'sal<comm'
				when sal=nvl(comm,0) then 'sal=comm'
				else 'sal>comm'
				end sal_comm
				from emp;
			Syntax: DECODE(col|expression, search1, result1
					[, search2, result2,...,]
					[, default])
			Ex: select empno,ename,deptno decode(deptno, 10,'Accounting', 20, 'Research', 30, 'Sales' , 'Others') dname from emp;		
		
****************************************************************************************************************
--What are group functions?
Ans:Group functions operate on sets of rows to give one result per group.
	Types of group functions are
	• AVG
	• COUNT
	• MAX
	• MIN
	• STDDEV
	• SUM
	• VARIANCE
	
	Note: AVG and SUM are for numeric data. MIN and MAX for all data types.
	All the group functions exclude null values.
	
****************************************************************************************************************
--What is Group By?
Ans: Group By is used to split(or group) records into multiple groups based on one or more colummn based values.
	Syntax:SELECT DISTINCT <col1>,<col2>,...
		FROM <table1>,<table2>,........
		[WHERE <condition>]
		GROUP BY <col1>,<col2>,...
		[HAVING <condition>]
		[ORDER BY <column> asc/desc,..]

	Order of execution: FROM--->WHERE---->GROUP BY----->HAVING---->DISTINCT--->ORDER BY---->SELECT

	Rules:
	*a)The columns those are specified in GROUP BY list clause only allowed in select list.
	*b)Any column with any group function can be used with SELECT though it is not specified in GROUP BY list.

	Ex:
	Question: 
	a)Display highest sal from each dept.
	Ans:SELECT deptno,max(sal) from emp GROUP BY deptno;--this query will return the selected columns but one row per deptno. so it will group all the rows belongs to same dept and gives summarized info. 

	b)Display the no of employees belongs to each job group.
	Ans:SELECT job,count(job) FROM emp GROUP BY job;
	
	c)Display the departments where atleast 5 emps are working.
	Ans:SELECT deptno,count(*) FROM emp GROUP BY deptno HAVING count(*)>=5;

	*d)Display the details of manager for whom atleast 5 employees are working?
	Ans:SELECT * FROM emp where empno in (SELECT mgr from emp GROUP BY mgr having count(*)>=5);

	The following is an illegal query using GROUP BY.
	-------------------------------------------------
	i)SELECT job,count(job) FROM emp; 
	**Any column or expression in the SELECT list when used with group function if it is not present in the group by clause will throw an exception saying not a single group function .
	
	ii)SELECT deptno, AVG(sal)FROM employees WHERE AVG(sal) > 2000 GROUP BY deptno;
	• You cannot use the WHERE clause to restrict groups.
	• You must use HAVING clause to restrict groups. 
	• You cannot use group functions in the WHERE clause.
********************************************************************************************************************
--what is a sub query or nested query?
Ans:A simple select query cannot solve the following problem. 
	which employees have salaries greater than 'SMITH' salary?
	
	A sub query always .
	The result of the sub query is used by the main query. Here is the solution for the above problem using sub query.
	
	select * from emp where sal > 
					(select sal from emp where ename = 'SMITH')
	
	Guidelines for using subqueries:
	--------------------------------
	• The ORDER BY clause in the subquery is not needed unless you are performing top-n analysis.
	• Use single-row operators with single-row subqueries and use multiple-row operators with multiple-row subqueries.	
	
	Single row subqueries return single row. single row comparision operators are =, >,>=,<,<=,<> .
	The above example is a single row subquey.
	Multiple row subqueries return mutliple rows. Multiple row comparision operators are IN, ANY and ALL.
	Ex:SELECT * FROM emp WHERE sal < any  (SELECT sal FROM emp where deptno = 30);
	
	Illegal use of subqueries
	-------------------------
	SELECT ename FROM employees WHERE sal = 
		(SELECT MIN(sal) FROM emp GROUP BY deptno);
	This query fails because subquery returns multiple rows. i.e. you cannot use single row operators with multiple row subqueries.	
	
	Display the highest sal from each dept.
	Ans:SELECT * FROM emp x where sal=(SELECT max(sal) FROM emp y where (y.deptno=x.deptno)); //See above how to the same thing with GROUP BY clause. 
	Note: The above query is called as correlated sub query because sub query executes for every record of main query.
********************************************************************************************************************
--display duplicate records based on a column?
Ans: 
	Solution1:
	select * from emp x where rowid <> (select max(rowid) from emp y where y.ename=x.ename);//CORRELATED NESTED QUERY
**************************************************************************************************************************
--delete duplicate records based on a column?
Ans: delete from emp x where rowid <> (select max(rowid) from emp y where y.ename=x.ename);
**************************************************************************************************************************
how to find the nth highest salary?
Ans: 
	Solution1:
	select * from emp e1 where &n=(select count(distinct e2.sal) from emp e2 where 	e2.sal>=e1.sal);//This is a corelated nested query. 
	Solution2:
	Select * FROM (SELECT emp.* ,RANK() OVER (ORDER BY sal DESC) no FROM emp) WHERE no=&n;//using RANK function


**************************************************************************************************************************
how to find the top n records?
Ans: same as above with one change.
select * from emp e1 where &n>=(select count(e2.sal) from emp e2 where 	e2.sal>=e1.sal);//remove the distinct column from count and  change &n= to &n>=.
		//If there are two persons with same salary, if you avoid them using Distinct clause then they will eliminated from inner query,but you would see them because of the &n>=. so it may display the duplicate records which were removed from inner query.
**************************************************************************************************************************
What is RANK function?
Ans: A RANK function is which gives numbers in sequence starting from 1 to n (where n is the no of rows returned), used to rank the 	rows returned from a select query.
	Syntax:SELECT <col1>,<col2>.. ,RANK() OVER (ORDER BY sal desc) <alias_name> FROM emp;

	Ex:SELECT emp.* ,RANK() OVER (ORDER BY sal desc) myrank FROM emp;//displays records in desc order of salary along with rank .

**************************************************************************************************************************
What are Exceptions  in pl/sql?
Ans: There are two types of exceptions in oracle.
	1)system defined. Ex: no_data_found,too_many_rows, value_error etc.
		Example:
		Declare 
			Name emp.ename%type;
			no   emp.empno%type;
			salary emp.sal%type;

		Begin 
			no:=&empno;	
			select sal into salary from emp where empno=no;
			DBMS_OUTPUT.put_line('the salary for empno '||no||' is '||salary);

			Exception 
			when no_data_found then
			DBMS_OUTPUT.put_line('this'|| no||'does not exist');
		End;

	2)user defined:  user can raise any exception in two ways .
	a)raise : If user is intereseted to process the error , use this method.
		Example:
			Declare 
				Name emp.ename%type;
				no   emp.empno%type;
				salary emp.sal%type;
				x Exception;
			
			Begin 
				no:=&empno;	
				select sal into salary from emp where empno=no;
				if salary<1000 then
				raise x;
				end if;
			
				Exception 
				when x then
				DBMS_OUTPUT.put_line('this employees salary is lessthan thousand');
			End;


	b)raise_application_error(err#,errmsg): This helps to stop the program. 
		
		Example:  this will raise an error if salary is greater than 4000.
			      Declare
				Name emp.ename%type;
				no   emp.empno%type;
				salary emp.sal%type;
				
			
			Begin 
				no:=&empno;	
				select sal into salary from emp where empno=no;
				if salary>1000 then
				raise_application_error(-20000, 'employee with empno '||no||' is greater than 4000'); 
				end if;
			
				Exception 
				when no_data_found then
				DBMS_OUTPUT.put_line('this employees salary is lessthan thousand');
				when others then
				DBMS_OUTPUT.put_line('error no '||sqlcode);
				DBMS_OUTPUT.put_line('error message '||sqlerrm);
			End;

			Note: SQLCODE,SQLERRM(lower case also valid) are prdefined functions and specific to pl/sql. SQLCODE gives the recent error code, SQLERRM gives the message include with that. Oracle has given space for user defined error codes from -20999 to -20000.
			OTHERS is used to handle all the unhandled exceptions.
**************************************************************************************************************************
--what is multi table insert?
Ans: 'INSERT ALL' -- it is an ETL(Extraction,Transformation,Loading) command.  This command access data from other table or tables and load into specified list of tables
	conditionally or unconditionally.
	Ex:
	1)insert all 
		into emp10
		into emp20
		into emp30 select * from emp; //no conditions.  you can specify required column names  also.

	2)insert all 
		when deptno=10 then into emp10
		when deptno=20 then into emp20
		when deptno=30 then into emp30
		when deptno=40 then into emp40 select * from emp; //conditional
		Note: You must have emp10,emp20,emp30,emp40 to be created prior to this execution.
**************************************************************************************************************************
--what is UPSERT(update+insert)?
Ans: UPSERT is also an ETL command to load the data from source table and update the data in the destination table based on join condition defined on common column.
		Syntax:	merge into <dest_table>
					using <source_table> <alias_name>
					on (<join condition>)
					when matched then
					update set <col1>=<value>
					when not matched then
					insert values(val1,val2,......);

		Example:merge into emp10
					using emp e on 
					(e.empno=emp10.empno)
					when matched then
					update set sal=sal+e.sal*10/100
					when not matched then
					insert values(e.empno,e.ename,e.job,e.mgr,e.hiredate,e.sal,e.comm,e.deptno);
*******************************************************************************************************************
--what is JOIN?
Ans:  A JOIN is used to combine records from two or more tables. A JOIN is always defined between two tables because JOIN  is binary operator, but for multiple tables 
		you need to define more than one JOIN. To defube a join  on n tables, we require n-1 joins. There are two standards one by oracle and another by ANSI for JOINs. 
		Types of joins by oracle
		
		• Equijoin
		• Nonequijoin
		• Outer join
		• Self join

		Types of Joins by ANSI
		• Cross joins
		• Natural joins
		• Using clause
		• Full or two sided outer joins
		• Arbitrary join conditions for outer joins

		Syntax: 
		------	
		SELECT table1.column, table2.column
			FROM table1
			[CROSS JOIN table2] |
			[NATURAL JOIN table2] |
			[JOIN table2 USING (column_name)] |
			[JOIN table2 ON(table1.column_name = table2.column_name)] |
			[LEFT|RIGHT|FULL OUTER JOIN table2 ON (table1.column_name = table2.column_name)];



		1)INNER JOIN ----> Retrieves only matching records. Types of  INNER JOIN are a)EQUI-JOIN b)NATURAL JOIN(ANSI) c)CROSS JOIN(ANSI) 
		2)OUTER JOIN ----->Retrives matched records and non matched records, also called FORCED JOIN. Types are a) LEFT OUTER JOIN b) RIGHT OUTER JOIN 
						     c)FULL OUTER JOIN
		3)SELF JOIN.(Oracle)-------> A join on the same table.
		
		INNERJOIN
		-----------------
			a)EQUI-JOIN:If the relation between the columns of diff tables is established by using exact match, then it is  EQUI-JOIN.
			------------------
				Example: 
				1)select  loc from emp e,dept d where e.ename='SMITH' and d.deptno=e.deptno; //oracle standard	
				Note: if e.ename ='SMITH' is not used then dbms searches for the name in dept table also. process time is wasted.
				 
				2)select e.empno,e.ename,e.sal,d.dname,d.loc from emp e,dept d where d.deptno=e.deptno; //for simple join on a column , you need to mention the condition in where clause 
				ANSI standard
				---------------------
				2)select e.empno,e.ename,e.sal,d.dname,d.loc from emp e INNER JOIN dept d on e.deptno=d.deptno;//for simple join on a column, you dont require a where cluase.

			b)NATURAL JOIN:
			The NATURAL JOIN clause is based on all columns in the two tables that have the same name.It selects rows from the two tables that have equal
			values in all matched columns. Natural Join is also a kind of equijoin.

			Note: If the columns having the same names have different data types, then an error is returned.

				Example: 
				1)select * from emp Natural join dept;//This will print records from emp and dept based on deptno, because that is the common column. This is ANSI standard.  no oracle standard available here. to acheive this you need to write left and right outer joins and unite them using UNION clause.

			If several columns have the same names but the data types do not match, the NATURAL JOIN clause can be modified with the USING clause to
			specify the columns that should be used for an equijoin. 
			
			i)USING
			--------
			Note: Use the USING clause to match only one column when more than one column matches.
			
			• Do not use a table name or alias in the referenced columns.
			• The NATURAL JOIN and USING clauses are mutually exclusive.
			Ex: select e.ename,e.empno,deptno,d.loc from emp e join dept d using (deptno); 

			ii)ON
			-----
			The join condition for the natural join is basically an equijoin of all columns with the same name.
			• To specify arbitrary conditions or specify columns to join, the ON clause is used.
			• Separates the join condition from other search conditions.
			• The ON clause makes code easy to understand.

			c)CROSS JOIN :CROSS JOIN  gives the cartesian product from both the tables.

				Example:
				1)select * from emp,dept; --oracle standard
				2)select * from emp CROSS JOIN dept; --ANSI   


		2)OUTER JOIN: 
		-------------
			a)LEFT OUTER JOIN: It displays the records from the left table irrespective of the match in the right table.If a match is found then displays the columns from both, otherwise leave the  non matching fields blank.

			Example:
			Query: Display the details of the dept table irrespective of the matched records from emp table, if matched display empno and ename also.
			1)select d.*,e.empno,e.ename  from dept d,emp e where 
			d.deptno=e.deptno(+); --oracle standard
			2)select d.*,e.empno,e.ename from (dept d LEFT OUTER JOIN emp e on (d.deptno=e.deptno));--oracle standard

			b)RIGHT OUTER JOIN: It is similar to LEFT OUTER JOIN , but displays the records from the right table irrespective of the match in the right table.
			Example:The same query above using RIGHT OUTER JOIN.
			1)select d.*,e.empno,e.ename  from dept d,emp e where e.deptno=d.deptno(+);--oracle standard
			2)select d.*,e.empno,e.ename from (emp e RIGHT OUTER JOIN dept d on (e.deptno=d.deptno));--ANSI standard

		3)SELF JOIN:It is a join on the same table.
		------------
			Example:
			Query: Display the List of all the employees belongs to the same dept as that of 'SMITH'?
			Ans:It is not possible to retreive records based on the same column comparision.
			1)Select B.* from emp A,emp B where A.ename='SMITH' and B.deptno=A.deptno;
***********************************************************************************************
Difference between Inner join and outer join ?
Ans:The join of two tables returning only matched rows is an inner join.
	• A join between two tables that returns the results of the inner join as well as unmatched rows left (or right) tables is a left (or right) outer join.
	• A join between two tables that returns the results of an inner join as well as the results of a left and right join is a full outer join.
********************************************************************************************
--Difference between Select and Join?
Ans: Select is used to retrieve the data from one table,but join is used to retreive 			data from multiple tables.
********************************************************************************************
--what is UNION?
Ans: UNION is a set operator.It combines the o/p of two different queries.so, it 			simply add the rows from two queries.It won't allow 				duplicates.UNION ALL gives the duplicate rows also.
		Example:Display the details of all the employees whose sal is same as that of either SCOTT or ADAMS?
		Ans: select * from emp where sal in (select sal from emp where ename='SCOTT' UNION select sal from emp where ename='ADAMS');
**************************************************************************************************************************
--Difference between UNION and JOIN?
UNION : The union operator combines the results of two or more queries into a single 			result set. But no.of columns must match in both/all the 			queries (and also the order) which are used for union. 
			You cannot use the union operator within a create view 
			statement. 
			You cannot use the union operator on text and image columns.

JOIN: JOIN is used to extract information from more than one table based on the 			related column/coloums. any no. of rows can be retrived based on matching colums. 
**************************************************************************************************************************
--what is the difference between INNER JOIN and INTERSECT?//Nomura Holdings
Ans:Both will return matched records but there are certain differences between them.

		------->INNER JOIN displays columns from two or more tables based on a 	condition on common column(s) where as 
		------->INTERSECT is used to retrieve the common records from both the 	left and the right query of the Intersect Operator. 
		------->INTERSECT operator returns almost same results as INNER JOIN 	clause many times.
		------->The order of the columns and no.of columns from both the 	select queries (with INTERSECT) must match.
********************************************************************************************************************
what are Database transactions?
Ans:A database transaction consists of one of the following:
	• DML statements which constitute one consistent change to the data followed by One DDL statement or DCL statement.
	 A DB transaction Begin when the first DML SQL statement is executed and End with one of the following events:
	– A COMMIT or ROLLBACK statement is issued
	– A DDL or DCL statement executes (automatic commit)
	– The system crashes
	With COMMIT and ROLLBACK statements, we can:
	• Ensure data consistency
	• Preview data changes before making changes permanent
	• Group logically related operations
	Ex:
	COMMIT---> Transaction starts---> DELETE --->SAVEPOINT A --->
	INSERT --->UPDATE --->SAVEPOINT B --> INSERT

	At this stage we can do the following .
	ROLLBACK to B
	ROLLBACK to A
	ROLLBACK . This will take us to transaction starting point.
	
	Note: If a single DML statement fails during execution,only that statement is rolled back.The user should terminate transactions explicitly by executing a COMMIT or ROLLBACK statement.
	
*************************************************************************************************************

What is index?
Ans:Index is used to improve the performance of a select query.
	Ex:select * from emp where sal<2000;// To run this query, first dbms should sort on sal in ascending order and  gives the matching records. Most of the time is wasted in sorting only. so this is the reason for using indexes.
	
	Note:
	a)Every index occupies memory. They are stored seperately from actual data. 
	b)Having too many indexes make the process slow.
	c)Indexes donot have to be activated or deactivated. 
	d)Indexes are mostly useful on larger tables and on columns that are likely to appear in  WHERE cluase.Tables having less data (say less than 100 rows) indexing is not necessary.
	e)If data is static and heavily queried then more indexes will help. If data is dynamic and lots of updates on indexed columns will slowdown the process.
	f)It will be better if data is loaded first and index is created.
	f)we can create index on multiple columns. oracle 9i supports a max of 16 columns and 255 characters of column space.

	There are two types of indexes.(oracle claims)
	
	1)unique index -- sorts only distinct data. Unique index cannot be created on primary key or unique key columns, because they are already indexed. 
	
	Syntax: create [unique] index <index_name> on <table_name>(<col1>,<col2>..);
	
	Ex:create unique index empidx on emp(empno);
	
	Note:This option  will be failed, if the indexed column already contains duplicate data.
	
	2)non unique index -- sorts with duplicate data. Syntax is same as above just remove the unique keyword.

	Ex:
	a)create index sal_idx on emp(sal);
	b)create index ename_index on emp(upper(ename));
	Note:This is called function based index. oracle supports it from oracle 8i.
	c)create index ename_job on emp(ename,job);

	There are many types of indexes like cluster index, bitmap index etc..
	But i categorize them into unique and non-unique, because i found no reason in stating those many types. Many authors claim many ways like Implicit indexes, Explicit Index or cluster index , non cluster index etc. Nobody has given standard types, probably i need to check with ANSI standards. 

	Alter index <index_name> rebuild;//to rebuild the index.
	drop index <index_name>;//to drop the index.
	select index_name from user_indexes where table_name='emp';//to retrieve the 						indexes defined on a table.
**************************************************************************************************************************
--what is bitmap index?
Ans:A bitmap index is a special kind of index that stores the bulk of its data as bit 	arrays (bitmaps) and answers most queries by performing bitwise logical 	operations on these bitmaps. the bitmap index is designed for cases where the 	values of a variable repeat very frequently. For example, the gender field in 	a customer database usually contains two distinct values: male or female. For 	such variables, the bitmap index can have a significant performance advantage 	over the commonly used trees.

	Syntax:create bitmap index <index_name> on <table_name>(column_name);
**************************************************************************************************************************
--what is cluster index?
Ans:A cluster is a db object to improve the performance.It is generally required for 	tables those are frequently joined. 
	A cluster indicates a common column or the column on which the tables are joined. 
	The definition of the cluster maintains the specification of the column.  Every cluster needs a cluster index, with out index tables cluster table wont accept data.
	The name of the cluster column and the column used in table need not be same but data type should match.

	Creating cluster:
	syntax:
	create cluster <cluster_name> (<col_name> datatype<width>);
	
	Ex:create cluster cluster_dept (deptno number(2));


	As stated above every cluster needs a cluster index.

	Creating cluster index:
	syntax:
	create index <index_name> on cluster<cluster_name>;

	Ex:create index cluster_index on cluster_dept;

	Creating cluster table;

	create table emp
	(
	empno number(4) primary key,
	---------
	----------
	----------
	deptno number(2) references dept(deptno) on delete cascade)
	}
	cluster cluster_dept;
**************************************************************************************************************************
--what is a sequence?
Ans: A sequence is a db object used to generate sequence of integers. It is used in 	multi user environment where in different applications access a serial number.
	
	Syntax:
	create sequence <sequence_name>
	start with <int> //default 1
	increment by <int> //default 1
	minvalue/nominvalue //default 1
	maxvalue/nomaxvalue
	cycle/nocycle //default nocycle
	cache/nocache <int> //default 25
	[order]

	Note:Cyclic sequence will make to restart the numbers from min values.

	Ex:create sequence seq_10000 
	    start with 1000
	    increment by 100
	    maxvalue 10000
	    cache 50;
**************************************************************************************************************************
--how to use sequence?
Ans: to use the sequence for generating the values oracle has given two psuedo 		columns. 1)CURRVAL 2)NEXTVAL
	Ex:
	insert into emp(empno,ename,sal) values(seq_10000.nextval,'bravo',5000);
	
	Note: Alter sequence seq_10000 maxvalue 20000;// to alter a sequence
	      Drop sequence seq_1000;//to drop a sequence.
	      select sequence_name from user_sequences;//list all sequence names.
**************************************************************************************************************************	      
--what is VIEW?
Ans:A VIEW is a db object, which is used to register a select statement permanently in 	the database.
	Advanatages:
	1)we can register the complex queries to disc.
	2)view compiles the query before registering,so they are ready to execute.

	Note:views won't store the data, but occupies some memory to store the definition.

	Ex:select * from tab;//tab is a predefined view from oracle.

	Syntax: create view <view_name>(<column1>,<column2>,....)
		as
		select statements;

	Ex: 
	1)CREATE VIEW emp_job(job)
		as
	SELECT * FROM emp WHERE job='CLERK';//you can leave the job column.

	2)CREATE VIEW emp_sal(ma_sal,mi_sal,avg_sal)
		as
	SELECT max(sal),min(sal),avg(sal) FROM emp;
	
	Note:alias names must be given when using arithmetic expressions,functions,pseudo columns etc.you can write in the view definition (i.e. as defined above) or you can write directly in a select query. 	
	      
**************************************************************************************************************************
--can we work with INSERT,UPDATE,DELETE commands on view?
Ans:It is possible to insert,update and delete using views.But there are some constraints.They are
	Rules:
	1)It is not possibel to delete a row using view, if the view is derived from more than one table.
	2)For updation,the rule 1 + the view should not use Arthmetic expressions, Functions and pseudo columns.
	3)For Insertion rule 1+rule 2+ the view should contain all the mandatory fields of base table.

	Note:All the above rules can be violated from oracle 8i. Oracle 8i introduced a concept called 'VIEW TRIGGER'. A New Trigger called 'INSTEAD OFF' is introduced , which allow you to violate the rules stated above.

**************************************************************************************************************************
--what is WITH CHECK OPTION in VIEW?
Ans:	create VIEW empdept 
	as
	select empno,ename,deptno from emp where deptno=30;
	
	Using This above view a user can insert the records using the follwoing command.
	insert into empdept(empno,ename,deptno) values(1234,'raj',20);
	
	The whole thing is wrong because the view is written to handle only deptno 30, but user can enter any deptno.

	So to avoid this we can use WITH CHECK OPTION like this.
	
	create VIEW empdept 
	as
	select empno,ename,deptno from emp where deptno=30 WITH CHECK OPTION;//The same query written above to insert a row, when we try to run now we get exception.

	Note: WITH CHECK OPTION will fail if WHERE clause is not there in the statement.
**************************************************************************************
--What is forced view?
Ans:A forced view can be used to create a view even if the base table does not exist.
	But we cannot access anything from this view until we create a table.
	Syntax: CREATE [FORCE] VIEW <view_name> 
		as 
		select statement.........;
	Note:Even if the base table dropped eventhough view table exists. We need to re-create the table, to access the vie0/8w.
************************************************************************
--what is key preserved table?
Ans:A table is key preserved table if every key of the table can also be a key of a result of JOIN.It is Generally the table which 	contains the primary key.
	Ex:we have a join based on deptno, between emp and dept. Then emp is key preserved table and dept is non-key preserved table, because empno is primary key.ofcourse, deptno is also a primary key in dept table, but in the resulting JOIN , deptno may be repeated but empno is unique.

	create view emp_dept 
	as 
	select e.empno,e.ename,e.sal,e.deptno,d.dname,d.loc from emp e,dept d where e.deptno=d.deptno and d.loc in ('DALLAS','BOSTON');

	Note:You cannot update or delete with a VIEW that contains JOIN.

**************************************************************************************************************************
--Does oracle supports lock?
Ans:yes.Oracle supports both row level lock and table level lock.
	
	Ex:1)select * from emp where ename='SMITH' FOR UPDATE OF SAL;
	   2)LOCK TABLE emp IN SHARE MODE	
	Note: 'FOR UPDATE OF' is used to obtain row level lock.
	        LOCK TABLE command is used to obtain table lock.

**************************************************************************************
--What are TCL commands?
Ans:Oracle has 3 TCL(transaction control language) commands.
	1)COMMIT:Used to save the transactions to the disc. 
	Syntax: COMMIT;
	2)ROLLBACK:Used to undo the transaction.
	Syntax:ROLLBACK; or ROLLBACK to S
	AVEPOINT<savepoint_name>
	3)SAVEPOINT:Used to identify a position of the transaction, for ROLLBACK purpose.
	Syntax:SAVEPOINT <savepoint_name>

	Note: SAVEPOINT is always used in conjunction with ROLLBACK.

	Ex:insert into emp ...............;
	SAVEPOINT A
	insert into emp ...............;
	SAVEPOINT B
	insert into emp ...............;
	SAVEPOINT c 
	
	Note: In the following situations the transactions are automatically committed.
	1)when a DDL command is issued
	2)when any DCL command is issued
	3)when the environment setting AUTOCOMMIT is on.


************************************************************************

what are DCL commands?
Ans:GRANT and REVOKE are the DCL(data control language) commands.
	GRANT
	-----
	Syntax:Grant <privilege> priv1,priv2 [on <object_name>] to <user1>,<user2>...,<role1>,<role2>.....
	
	Types of privileges:
	1)User level privilege: A permisson(s)(or role) which is granted by a Database Administrator(DBA) to other valid users of a DB.Every User will have a role  or permission(s) associated.we can say DBA itself is a Role(or permission(s)). 

	Ex: create table,create any table,create view, create any view etc..
	Note:create table is to create a table in one's own schema, where as 
	     create any table allows the user to create a table anybody's schema.
	Examples:
	1)GRANT ALL on emp to test;--This will give all types of permissions to 'test' user.

	2)Object level privilege:Granting permission by one user of a DB to another user of DB on a specific object.
	
	Ex:SELECT,UPDATE,DELETE,INSERT,ALTER,INDEX,REFERENCE,EXECUTE.
	Note:EXECUTE permission is on PL/SQL programs like procedures, functions etc.

	Example:
	1)GRANT select on emp to demo;--This wll give select permission on emp table 				to user demo.
	2)GRANT UPDATE,DELETE on emp to po9;--(personal oracle 9)
	
	REVOKE:It is used to cancel the permission.
	-------
	Syntax:REVOKE <priv1><priv2> [ON <object>] from <user1>,<user2>
	Ex:REVOKE SELECT ON emp FROM test

************************************************************************
--What is a Role?
Ans: A Role is a collection of permissions. we can say a Role is a type of permissions 	on different objects defined all together as a single unit.permission is 	generally on a single object, where as a Role can be defined on one or more 	objects.
	Syntax:create Role <role_name> [identified by <password>]
	
	Ex: 1)create role Read_data;--	Now we need to define some object level 				permissions to the above defined Role.
		GRANT SELECT ON emp TO read_data;
		GRANT SELECT ON dept TO read_data;
		GRANT SELECT ON salgrade TO read_data;
		GRANT SELECT ON bonus TO read_data;
	Now it is easy to give read_data to any user, instead of writing these 4 statements every time.
	Ex:GRANT Read_data to test;--which will give all the above 4 permissions to test.	
	    2)create Role change_data;
		GRANT INSERT,UPDATE ON emp TO read_data;
		GRANT INSERT,UPDATE ON dept TO read_data;
		GRANT INSERT,UPDATE ON salgrade TO read_data;
		GRANT INSERT,UPDATE ON bonus TO read_data;
	
	Note:A Role cannot be cyclic. for example, you can say 
	"Grant change_data to read_data".--it is possible to do that, but you cannot assign "Grant read_data to change_data" immediately. you can revoke and try the second statement,then it will work.

	
***************************************************************************
--What is the Difference between SQL and PL/SQL.
Ans:1)SQL is a programming language but non-procedural language.
    --PL/SQL is also a programming language but procedural language.	
    2)Network traffic is high, because for every instruction network is used two times. one for seding the data and one for receiving the result.
    --with PL/SQL network traffic is low,because all instructins are bunched to form a block. That is why PL/SQL is also called block language.
    3)SQL is a data modifying language
    --PL/SQL is a data processing language.
    4)SQL display the data given by server.
    --PL/SQL can modify the data given by server
    5)SQL does not support any user defined types.
    --PL/SQL supports user defined types like PL/SQL tables,PL/SQL records,ref cursor etc..
    6)SQL displays results on console.
    --PL/SQL does not have any console to display by default. you need to use methods from dbms_output package to display the o/p.
***************************************************************************
--what are different types of loops exist in PL/SQL?
Ans:Loop is an execution a sequence of statements repeatedly.
	Types of loops:
	1)simple loop(infinite loop)
	Syntax:loop
		----;
		----;
		end loop;
	2)for loop
	Syntax: for <variable_name> in <int1>..<int2>
		loop
		----;
		----;
		end loop;
	3)reverse for loop
	Syntax: for <variable_name> in reverse <int1>..<int2>
		loop
		----;
		----;
		end loop;
	
	4)while loop
	Syntax:while <condition>
		-----;
		-----;
		end loop;
		Note: condition can be based on number,date or char datatype.

***************************************************************************
--Write a program to accept a string and print it in upper case without using upper() and initcap() functions?
Solutions:
		Declare
			str varchar2(10):='&str';
			rstr varchar2(10);
			c char;
		begin
			for i in 1..length(str)
			loop
				c:=substr(str,i,1);
				if(ascii(c) between 97 and 122) then
					c:=chr(ascii(c)-32);
				end if;
				rstr:=rstr||c;
			end loop;
			dbms_output.put_line('uppercase of given string '||rstr);
		end;
		Note:rstr:=c||rstr. this statement above will print reverse of the given string.

**************************************************************************
--what is embedded SQL?
Ans:Embedded SQL is used to store the results of SQL statements into PL/SQL variables.
	Syntax:SELECT <col1>,<col2>.....
		INTO <var1>,<var2>......
		FROM <Tab1>,<Tab2>.....
		WHERE <condition>

***************************************************************************
--What is Cursor?
Ans:Cursor is a db object, which comprises a control structure for the successful 	traversal of records in a result set.
	writing cursor is a 4 step process. 
	1)Allocating memory & store SQL statements in context area.--This is called declaration of Cursor.
	2)Executing SQL statements in Executor and send to DB.--Opening cursor.
	3)Reading Data from Active set and load the values into PL/SQL variable --This is called fetching
	4)Release the memory from context area. --closing the cursor.
	
	Note:Using simple SQL we cannot store the result into memory and travers through it. SQL can just display the data.
	

***************************************************************************
--Types of Cursors?
Ans:There are two types of cursors.
	1)Implicit -- default one. The name given to this cursor is 'SQL'.
	2)Explicit

***************************************************************************
--what are cursor attributes?
Ans:There are 4 attributes defined for cursors in oracle 9i.
	They are <cursorname>%
	1)ISOPEN --> to cross verify whether statement is executed or not. returns 		boolean value true if opened.
	2)FOUND -->If fetch is pointing from active set, returns true.
	3)NOTFOUND-->If fetch is not pointing from active set, returns true. (like EOF 		of java)  
	4)ROWCOUNT-->It is a changing value in each fetch.

	The exceptions associate with Cursors are
	1)Cursor already opened
	2)Invalid cursor --occurs when try to fetch from a closed cursor.

***************************************************************************
Example for Implicit cursor:
----------------------------
	Declare 

	name emp.ename%type;
	no   emp.empno%type;
	salary emp.sal%type;

	Begin 
		no:=&empno;
		select ename, sal into name, salary from emp where empno=no;
		DBMS_OUTPUT.put_line('the salary for  '||no||' is '||salary);
	End;

Example for Explicit cursor:
----------------------------
1)using simple loop

--write a program to print the details of all employees from emp table, with their experience.


	Declare
		cursor c1 is select * from emp;
		var1 emp%rowtype;
		exp number(5,3);
	begin
		open c1;
		dbms_output.put_line('--------------------------------------------');
		loop
		fetch c1 into var1;
		exit when c1%notfound;
		exp:=months_between(sysdate,var1.hiredate)/12;
		dbms_output.put('empno '||var1.empno||' ');
		dbms_output.put('ename '||var1.ename||' ');
		dbms_output.put('sal '||var1.sal||' ');
		dbms_output.put_line('experience '||exp);
		dbms_output.put_line('---------------------------------------------');
		end loop;
	end;

*********************************************************************************
--What is cursor for loop?
Ans: If the cursor is operated using for loop then it is called as cursor for loop.
	In this 4 steps are reduced to 2.
	Just declaring the cursor is enough. All 3 steps opening,fetching and closing will be performed by loop itself.
	Ex:
	Declare
		cursor c1 is select * from emp;
		exp number(5,3);
	begin
		dbms_output.put_line('--------------------------------------------');
		for i in c1
		loop
		exp:=months_between(sysdate,i.hiredate)/12;
		dbms_output.put('empno '||i.empno||' ');
		dbms_output.put('ename '||i.ename||' ');
		dbms_output.put('sal '||i.sal||' ');
		dbms_output.put_line('experience '||exp);
		dbms_output.put_line('---------------------------------------------');
		end loop;
	end;


*********************************************************************************
