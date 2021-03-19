---
layout:     post
title:      SQL Gaps & Islands Problems
subtitle:   Identify continuous and missing values in a sequence
date:       2020-09-10
author:     Yvonne Liu
header-img: img/simpsons2.jpg
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

* the id is the primary key for this table.
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


