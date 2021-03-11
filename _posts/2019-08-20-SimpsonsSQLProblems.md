---
layout:     post
title:      The Simpsons - Fun SQL Problems
subtitle:   Analyzing The Simpsons database by solving 2 fun problems using SQL
date:       2019-08-20
author:     Yvonne Liu
header-img: img/simpsons2.jpg
catalog: true
tags:
    - SQL
---

# Problem 1

## Problem Description

**In what season/episode does Homer utter his 100th, 200th, and 300th “d’oh”?**  

* Be sure to only include instances of “d’oh” linked to Homer’s character_id=2.  

* Be aware that a record in the “script_lines” table can contain multiple instances of “d’oh” and we want to count all of them.  For instance, if Homer says “d’oh, d’oh, d’oh” in one “script_lines” record then that counts as 3 “d’oh,” not 1.  

* Specifically, count all instances of the substring **‘annoyed grunt’** in the “raw_text” field **AND** all instances of the substring **‘doh’** in the “normalized_text” string (recall that ‘annoyed grunt’ in the “raw_text” field represents a ‘doh’).  

* Just to be crystal clear, if a record has 1 instance of ‘annoyed grunt’ in “raw_text” and 1 instance of ‘doh’ in “normalized_text” then that record counts as 2 total.  

* Output should include the **season**, **episode number within the season**, **episode title**, **location**, and **‘doh counter’** for each of the 100th, 200th, and 300th occurrences.

## Query

```
WITH tab1 AS
	(
	SELECT	id,
		episode_id,
		number, 
		character_id,
		location_id,
		ROUND((LENGTH(LOWER(raw_text)) - 
			LENGTH(REPLACE(LOWER(raw_text),'annoyed grunt',''))
			) 
			/ 
			LENGTH('annoyed grunt')        
			) AS count_ag,
		COALESCE(ROUND((LENGTH(LOWER(normalized_text)) - 
				LENGTH(REPLACE(LOWER(normalized_text),'doh',''))
				) 
				/ 
				LENGTH('doh')
			),0) AS count_doh
	FROM	script_lines
	WHERE	(raw_text LIKE '%annoyed grunt%' OR
		normalized_text LIKE 'doh %' OR
		normalized_text LIKE '% doh %' OR
		normalized_text LIKE '% doh') AND
		character_id = 2
	),
tab2 AS
	(		
	SELECT	*,
		count_ag + count_doh AS doh_total
	FROM	tab1
	),
tab3 AS
	(
	SELECT	*,
		SUM(doh_total) OVER(ORDER BY episode_id, number) AS doh_sum
	FROM	tab2
	)
SELECT		episodes.season AS Season,
		episodes.number_in_season AS Episode,
        	episodes.title AS Title,
        	locations.name AS Location,
        	doh_sum
FROM		tab3
INNER JOIN	episodes
ON		tab3.episode_id = episodes.id
INNER JOIN	locations
ON		tab3.location_id = locations.id
WHERE		doh_sum = 100 OR
		doh_sum = 200 OR
		doh_sum = 300;

```

## Output
```
Season      Episode     Title                               Location            doh_sum
3           24          Brother, Can You Spare Two Dimes?   Simpson Home        100
7           13          Two Bad Neighbors                   Simpson Home        200
14          17          Three Guys of the Condo             Apartment           300
```

***
# Problem 2

## Problem Description

**Barney Gumble (character_id=18)** is a funny character on the show who is likely Homer Simpson’s best friend.  Some have argued that Barney “always seems to be stuck in Homer’s shadow” and should be given more “airtime” independent of Homer.  We will investigate this claim.  **Specifically, we want to count the number of Barney’s “appearances” with and without Homer (character_id=2).**  

* A “Barney appearance” will not simply be defined as a record in “script_lines” with character_id=18 - if he has multiple lines in an episode that are relatively close to one another we want consider those lines as being part of the same “appearance.”  
* Specifically, order your script_lines data by “episode_id” and then by “number” and consider each episode separately in your calculations.  
* First, find all records credited to Barney (character_id=18), including speaking and non-speaking lines (in what is shown below I denote this with “barney_ind”).  
* Then, consider a “Barney appearance” to consist of the three records before the first instance of barney_ind=1, the three records after the last instance of barney_ind=1, and all records in-between as long as there is no gap of 6 consecutive records or more that are not credited to Barney.  
* So, let’s run through a couple of examples. 

![1_barney](https://user-images.githubusercontent.com/78829814/110739832-ca371200-81e6-11eb-9e82-18f612f1a8f6.jpg)


* The above yellow block of records would define one “Barney appearance.”  It consists of the three records before the first “barney_ind,” the three records after the last “barney_ind,” and there is no gap of six or more consecutive records with “barney_ind”=0 in between.  Also note that since there is at least one instance of character_id=2 (Homer) in this “Barney appearance,” this appearance would be counted as one “with Homer.” 


![2_barney](https://user-images.githubusercontent.com/78829814/110739620-63195d80-81e6-11eb-9ddf-28ce4de19bf1.jpg)

* Here, we have a gap of 6 records between two records with “barney_ind”=1.  So, the blue block of records would correspond with one “Barney appearance” and the yellow block of records would correspond with a second “Barney appearance.”  Again, also note that Homer appears in both of these “Barney appearances.”

## Query

```
WITH tab1 AS
(
SELECT		*,
		CASE
		WHEN	character_id = 18 THEN 1
		ELSE 	0
		END AS 	barney_ind
FROM		script_lines
),
tab2 AS
(
SELECT		*,
		SUM(barney_ind) OVER(ORDER BY episode_id, number) as b_ind_run_sum,
		CASE
		WHEN	(SUM(barney_ind) OVER(	PARTITION BY episode_id 
						ORDER BY episode_id,number 
						ROWS BETWEEN 3 PRECEDING AND 3 FOLLOWING) > 0) THEN 1
		ELSE	0
		END AS 	b_app_ind
FROM		tab1
),
tab3 AS
(
SELECT		*,
		CASE
		WHEN	(b_app_ind = 1 AND (LAG(b_app_ind) OVER(PARTITION BY episode_id 
								ORDER BY episode_id,number) = 0)) THEN 1
		WHEN	(b_app_ind = 1 AND (SUM(barney_ind) OVER(PARTITION BY episode_id 
								ORDER BY episode_id,number 
								ROWS BETWEEN 3 PRECEDING AND 2 FOLLOWING) = 0)) THEN 1
		ELSE 	0
		END AS 	start_b_app
FROM		tab2
),
tab4 AS
(
SELECT 		*,
		SUM(start_b_app) OVER(ORDER BY episode_id, number) AS barney_groups,
		CASE
		WHEN	character_id = 2 THEN 1
		ELSE 	0
        	END AS 	homer_ind
FROM		tab3
WHERE		b_app_ind = 1
),
tab5 AS
(
SELECT		barney_groups,
		MAX(homer_ind) AS homer_in_b_app
FROM		tab4
GROUP BY 	barney_groups
),
tab6 AS
(
SELECT		CASE
		WHEN 	homer_in_b_app = 1 THEN 'With Homer'
		WHEN 	homer_in_b_app = 0 THEN 'Without Homer'
		ELSE 	NULL
		END AS 	Barney_Appearance
FROM		tab5
)
SELECT		Barney_Appearance,
		COUNT(*) AS App_Count
FROM		tab6
GROUP BY 	Barney_Appearance;
```

## Output

```
Barney_Appearance	App_Count
With Homer		215
Without Homer		111

```

| Barney_Appearance | App_Count |	
|:----------------- |:---------:|	
| With Homer        |    215    |	
| Without Homer     |    111    |

