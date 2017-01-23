# Querying Microsoft SQL Server 2012

## Transact-SQL (T-SQL)
  - T-SQL is the main language used to manage and manipulate data in Microsoft SQL Server. SQL Server also supports other languages, like C# and VB but T-SQL is usually the preferred language for data management and manipulation. 
    - T-SQL is a dialect of standard SQL. 
    - T-SQL is based on strong mathematical foundations. (Based on standard SQL, which in turn is based on the relational model, which is based on set theory and predicate logic.)
    - Follow the SQL standard:
      - <> over !=
      - CAST over CONVERT
      - ; at the end etc
  - A relation in the relational model refers to a relation as a table. The relational model is based on predicate logic and set logic.
  - **T-SQL vs SQL**
    - SQL is standard; T-SQL is the dialect of and extension to SQL that Microsoft implements in its RDBMS - SQL Server.
  - Write queries that interact with the table as a whole (not iteratively).
  - **T-SQL Deviations from SQL**
    - T-SQL is based more on multiset theory (bags or supersets) than on set theory. 
    - A table doesn't have to have a key.
    - Allows you to refer to ordinal positions of columns from the result in the ORDER BY clause i.e. ORDER BY 1; (1st column)
    - Allows defining result columns based on an expression without assigning a name to the target column. e.g. Select empid, firstname + ' ' + lastname FROM HR.EMployees. (We do not have to set the column name for firstname + lastname AS fullname, it just shows up empty for the column heading)
    - Allows a query to return multiple result columns with the same name. 
    - Uses three-valued predicate logic. True/False/Null. When a predicate comapres two values, if both are present, the result evaluates to either true or false, but if one is NULL, the result evalutes to a third logical value - Unknown.
    - NOTE: T-SQL provides tools/features to still do it the relational way. 

## Logical Query Processing
  - T-SQL is a declarative English-like language - You define what you want (the "what" part, the DB engine will figure out the "how" part) as opposed to how to achieve what you want. 
  - **Phases (In keyed-in order)**
    - SELECT
    - FROM
    - WHERE
    - GROUP BY
    - HAVING
    - ORDER BY
  - **Conceptual interpretation order is different for the above; it processes it like this:**
    - FROM
    - WHERE (Cannot refer to aliases here because SELECT is after this)
    - GROUP BY
    - HAVING (Evaluated per group, whereas WHERE is evaluated before rows are grouped; Therefore is evaluated per row.)
    - SELECT
    - ORDER BY
  - T-SQL eavluates all expressions that appear in the same logical query processing phase in an all-at-once manner. 
    - SQL server won't necessarily physically process all expressions at the same point in time, but it has to produce a result as if it did (conceptually). This is different to other programming languages where it's usually evaluated from left to right. But T-SQL is different.

