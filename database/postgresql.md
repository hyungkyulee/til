# PostgreSQL general

- the world’s most advanced open source database
- fourth most popular database
- ACID compliant and highly extensible (object-oriented db)
- a wide variety of built-in PostgreSQL data types (JSON, XML, HSTORE (key-value), Geo-spatial (PostGIS), IPv6)
- flexible indexing, featuring composite indexes, GiST, SP- GiST, GIN
- full Text Search, online index reorganization
- accepts MongoDB queries to interface with Postgres data
- a contrib module interface
- extensive SQL support
- programming languages : PL/pgSQL, PL/SQL, Java, Python, Ruby, C/C+, PHP, Perl, Tcl, Scheme
- library interfaces : OCI, libpq, JDBC, ODBC, .NET, Perl, Python, Ruby, C/C+, PHP, Lisp, Scheme, and Qt

## installation
> ref: https://www.postgresqltutorial.com/install-postgresql-macos/

- download postgreSql for mac
  > https://www.enterprisedb.com/downloads/postgres-postgresql-downloads
  > ![image](https://user-images.githubusercontent.com/59367560/133525880-d2fe2c1e-3262-4fd5-a617-078c7924f4ec.png)
  > ![image](https://user-images.githubusercontent.com/59367560/133526054-caf2bb9e-ddae-4a49-9c04-a26b345dffd7.png)

- install and load the sample database
  > application: pgAdmin 4

## Command Line
### SELECT statement
- usage
```
SELECT <column> 
  FROM <table>
 WHERE <condition>
  LIKE <letters contains>
```
- SELECT
  - Oracle DBMS is allowing a fomula column name

- LIKE
  - % : any letters
  - some DMBS is not supporting LIKE 

- WHERE
  - BETWEEN : including start and end scope
  - () : good(recommended) for a complicated expression
  - 
