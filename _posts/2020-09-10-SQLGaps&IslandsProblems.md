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

Gaps and islands problems involve **missing values in a sequence**. Solving the **gaps** problem requires **finding the ranges of missing values**, whereas solving the **islands** problem involves **finding the ranges of existing values**.

The sequences of values in gaps and islands problems can be **numeric**, such as a sequence of order IDs, some of which were deleted. An example of the gaps problem in this case would be finding the ranges of deleted order IDs. An example of the islands problem would be finding the ranges of existing IDs.

The sequences involved can also be **temporal**, such as order dates, some of which are missing due to inactive periods (weekends, holidays). Finding periods of inactivity is an example of the gaps problem, and finding periods of activity is an example of the islands problem. Another example of a temporal sequence is a process that needs to report every fixed interval of time that it is online (for example, every 4 hours). Finding unavailability and availability periods is another example of gaps and islands problems.


# Sample Data & Desired Results

Here, we have two tables - Accounts & Logins. We want to identify the id and name of **active users who logged in to their accounts for 5 or more consecutive days**.

Accounts table:

| id | name     |
|:--:|:--------:|
| 1  | Winston  |
| 7  | Jonathan |

* id is the primary key for this table.
* This table contains the account id and the user name of each account.


Logins table:

| id | login_date |
|:--:|:----------:|
| 7  | 2020-05-30 |
| 1  | 2020-05-30 |
| 7  | 2020-05-31 |
| 7  | 2020-06-01 |
| 7  | 2020-06-02 |
| 7  | 2020-06-02 |
| 7  | 2020-06-03 |
| 1  | 2020-06-07 |
| 7  | 2020-06-10 |

* There is no primary key for this table, it may contain duplicates.
* This table contains the account id of the user who logged in and the login date. A user may log in multiple times in the day.

## Constructing the Query

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
            DATE_SUB(login_date, INTERVAL rank1 DAY) as sequence_grouping
FROM        c1
),

c3 AS 
(
SELECT      id, 
            MIN(login_date) AS start_date, 
            MAX(login_date) AS end_date, 
            sequence_grouping, 
            COUNT(DISTINCT login_date) as consecutive_days
FROM        c2
GROUP BY    id, sequence_grouping
HAVING      COUNT(DISTINCT login_date) >= 5
)

SELECT      DISTINCT c3.id, name
FROM        c3
INNER JOIN  Accounts as a
ON          a.id = c3.id
ORDER BY    c3.id

```

The first CTE (c1) will return a result table like this:

| id | login_date | rank1 |
|:--:|:----------:|:-----:|
| 1  | 2020-05-30 | 1     |
| 1  | 2020-06-07 | 2     |
| 7  | 2020-05-30 | 1     |
| 7  | 2020-05-31 | 2     |
| 7  | 2020-06-01 | 3     |
| 7  | 2020-06-02 | 4     |
| 7  | 2020-06-02 | 4     |
| 7  | 2020-06-03 | 5     |
| 7  | 2020-06-10 | 6     |

* Here, we use a DENSE_RANK() because the Logins table has duplicates. We can also use ROW_NUMBER() if there are no duplicates.

In the second CTE (c2), we use DATE_SUB() to subtract the rank1 number from login_date, which results in the following table.

| id | login_date | rank1 | sequence_grouping|
|:--:|:----------:|:-----:|:----------------:|
| 1  | 2020-05-30 | 1     | 2020-05-29       |
| 1  | 2020-06-07 | 2     | 2020-06-05       |
| 7  | 2020-05-30 | 1     | 2020-05-29       |
| 7  | 2020-05-31 | 2     | 2020-05-29       |
| 7  | 2020-06-01 | 3     | 2020-05-29       |
| 7  | 2020-06-02 | 4     | 2020-05-29       |
| 7  | 2020-06-02 | 4     | 2020-05-29       |
| 7  | 2020-06-03 | 5     | 2020-05-29       |
| 7  | 2020-06-10 | 6     | 2020-06-04       |

* The sequence_grouping will be the same for all records in the same "island".

In the third CTE (c3), we have the start_date (when the user begins to login consecutively), end_date, and consecutive_days (number of days user login consecutively). 

| id | start_date   | end_date     | sequence_grouping | consecutive_days |
|:--:|:------------:|:------------:|:-----------------:|:----------------:|
| 7  | 2020-05-30   | 2020-06-03   |   2020-05-29      | 5                |

Finally, by joining to the Accounts table, we have the final result table:

| id | name       |
|:--:|:----------:|
| 7  | Jonathan   |

Using this solution, We could identify active users who logged in to their accounts for n or more days by simply changing the HAVING clause in c3. 
