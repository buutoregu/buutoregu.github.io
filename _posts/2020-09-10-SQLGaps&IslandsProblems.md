---
layout:     post
title:      SQL Gaps & Islands Problems
subtitle:   Identify continuous and missing values in a sequence
date:       2020-09-10
author:     Yvonne Liu
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - SQL
---

# Description of Gaps & Islands Problems

The "gaps and islands" problem is a scenario in which you need to identify groups of continuous data (“islands”) and groups where the data is missing (“gaps”) across a particular sequence.

# Case Study

Here, we have two tables:

Accounts: 

| Column Name | Type     | 
|:-----------:|:--------:|
| id          |   int    |  
| name        |  varchar |

* id is the primary key for this table.
* This table contains the account id and the user name of each account.

Logins:

| Column Name    | Type     | 
|:--------------:|:--------:|
| id             |   int    |  
| login_date     |  date    |

* There is no primary key for this table, it may contain duplicates.
* This table contains the account id of the user who logged in and the login date. A user may log in multiple times in the day.

## Problem

**Write an SQL query to find the id and name of active users.**

* Active users are those who logged in to their accounts for 5 or more consecutive days.
* Return the result table ordered by the id.

## Query

```
WITH c1 AS 
(
SELECT      id, 
            login_date, 
            DENSE_RANK() OVER(PARTITION BY id ORDER BY login_date) AS rank1
FROM        Logins
ORDER BY    id, login_date
),

c2 AS 
(
SELECT      id, 
            login_date, 
            rank1, 
            DATE_SUB(login_date, INTERVAL rank1 DAY) as rank2
FROM        c1
),

c3 AS 
(
SELECT      id, 
            MIN(login_date) AS start_date, 
            MAX(login_date) AS end_date, 
            rank2, 
            COUNT(DISTINCT login_date) as consecutive_days
FROM        c2
GROUP BY    id, rank2
HAVING      COUNT(DISTINCT login_date) >= 5
)

SELECT      DISTINCT c3.id, name
FROM        c3
INNER JOIN  Accounts as a
ON          a.id = c3.id
ORDER BY    c3.id

```
