
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
