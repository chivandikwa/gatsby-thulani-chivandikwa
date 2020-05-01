---
layout: post
title: SQL Transactions
image: img/animal1.jpg
author: Thulani S. Chivandikwa
date: 2020-04-04T00:00:00.001Z
tags: [SQL, Transaction]
draft: false
---

Each time I want to write a SQL query that modifies the database I find myself having to look up how to correctly make use of transactions. What better way for me to look this up next time than from my own blog.

A transaction is a unit of work for SQL queries, this allows for ACID (Atomi, Consistent, Isolated, Durable) operations that either fully commit or get rolled back to mantain data integrity. Imagine a couple of statements modifying data in multiple tables that is related. If one of those statements fails you will want to roll back everything and likewise if all succeed you would want to commit.

A simple pattern for this unit of work is as follows

```sql
BEGIN TRANSACTION [transaction_1]

  BEGIN TRY

      -- SQL statements

      COMMIT TRANSACTION [transaction_1]

  END TRY

  BEGIN CATCH

      ROLLBACK TRANSACTION [transaction_1]

  END CATCH
```
