
# Basic SQL

## What is SQL?

SQL (Structured Query Language) is a programming language designed for managing data in a relational database. It's been around since the 1970s and is the most common method of accessing data in databases today. SQL has a variety of functions that allow its users to read, manipulate, and change data. Though SQL is commonly used by engineers in software development, it's also popular with data analysts for a few reasons:

- It's semantically easy to understand and learn.
- Because it can be used to access large amounts of data directly where it's stored, analysts don't have to copy data into other applications.
- Compared to spreadsheet tools, data analysis done in SQL is easy to audit and replicate. For analysts, this means no more looking for the cell with the typo in the formula.

SQL is great for performing the types of aggregations that you might normally do in an Excel pivot table—sums, counts, minimums and maximums, etc.—but over much larger datasets and on multiple tables at the same time.

## What's a database?

From Wikipedia: A database is an organized collection of data.

There are many ways to organize a database and many different types of databases designed for different purposes. Mode's structure is fairly simple:

If you've used Excel, you should already be familiar with tables—they're similar to spreadsheets. Tables have rows and columns just like Excel, but are a little more rigid. Database tables, for instance, are always organized by column, and each column must have a unique name. To get a sense of this organization, the image below shows a sample table containing data from the 2010 Academy Awards:

![Database Table](https://mode.com/resources/images/the-basics/sample-table.png)

Broadly, within databases, tables are organized in schemas. At Mode, we organize tables around the users who upload them, so each person has his or her own schema. Schemas are defined by usernames, so if your username is databass3000, all of the tables you upload will be stored under the databass3000 schema. For example, if databass3000 uploads a table on fish food sales called ***fish_food_sales***, that table would be referenced as ***databass3000.fish_food_sales***.

## About this dataset

To start out, I'll be working with real data from the U.S. Census. This dataset shows the number of completed housing units in major regions of the United States. The table I'll be working with has a column for each region. The values in each row represent the number of housing units completed in thousands during the corresponding month. The data was collected in August 2014 and can be accessed at the (U.S. Census website)[https://www.census.gov/econ/currentdata/].

## 1. SQL SELECT

**In this lesson I'll cover:**
- Basic syntax: **SELECT** and **FROM**
- What actually happens when you run a query
- Formatting convention
- Column names
- Practice problem

### Basic syntax: SELECT and FROM

There are two required ingredients in any SQL query: *SELECT* and *FROM* — and they have to be in that order. SELECT indicates which columns you'd like to view, and FROM identifies the table that they live in.

Let's start by looking at a couple columns from the *us_housing_unit* table:

```sql
SELECT year,
       month,
       west
  FROM tutorial.us_housing_units
```

So what's happening in the above query? In this case, the query is telling the database to return the *year*, *month*, and *west* columns from the table *tutorial.us_housing_units*. (Remember that when referencing tables, the table names have to be preceded by the name of user who uploaded it.) When you run this query, you'll get back a set of results that shows values in each of these columns.

![SQL SELECT!](https://mode.com/resources/images/the-basics/prelim-results.png)

Note that the three column names were separated by a comma in the query. Whenever you select multiple columns, they must be separated by commas, but you should **not** include a comma after the last column name.

If you want to select every column in a table, you can use * instead of the column names:

```sql
SELECT *
  FROM tutorial.us_housing_units
```
![SQL SELECT2!](https://mode.com/resources/images/the-basics/results.png)

### Formatting convention

You might have noticed that the **SELECT** and **FROM** commands are capitalized. This isn't actually necessary — SQL will understand these commands if you type them in lowercase. Capitalizing commands is simply a convention that makes queries easier to read. Similarly, SQL treats one space, multiple spaces, or a line break as being the same thing. For example, SQL treats this the same way it does the previous query:

```sql 
SELECT *        FROM tutorial.us_housing_units
```

It also treats this the same way:

```sql
SELECT *
  FROM tutorial.us_housing_units
```

### Column names

While we're on the topic of formatting, it's worth noting the format of column names. All of the columns in the ***tutorial.us_housing_units*** table are named in lower case, and use underscores instead of spaces. The table name itself also uses underscores instead of spaces. Most people avoid putting spaces in column names because it's annoying to deal with spaces in SQL—if you want to have spaces in column names, you need to always refer to those columns in double quotes.

If you'd like your results to look a bit more presentable, you can rename columns to include spaces. For example, if you want the ***west*** column to appear as ***West Region*** in the results, you would have to type:

```sql
SELECT west AS "West Region"
  FROM tutorial.us_housing_units
```

Without the double quotes, that query would read 'West' and 'Region' as separate objects and would return an error. Note that the results will only return capital letters if you put column names in double quotes. The following query, for example, will return results with lower-case column names.

```sql
SELECT west AS West_Region,
       south AS South_Region
  FROM tutorial.us_housing_units
```