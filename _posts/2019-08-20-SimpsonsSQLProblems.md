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

# Problem 2

## Problem Description

**Barney Gumble (character_id=18)** is a funny character on the show who is likely Homer Simpson’s best friend.  Some have argued that Barney “always seems to be stuck in Homer’s shadow” and should be given more “airtime” independent of Homer.  We will investigate this claim.  **Specifically, we want to count the number of Barney’s “appearances” with and without Homer (character_id=2).**  

* A “Barney appearance” will not simply be defined as a record in “script_lines” with character_id=18 - if he has multiple lines in an episode that are relatively close to one another we want consider those lines as being part of the same “appearance.”  
* Specifically, order your script_lines data by “episode_id” and then by “number” and consider each episode separately in your calculations.  
* First, find all records credited to Barney (character_id=18), including speaking and non-speaking lines (in what is shown below I denote this with “barney_ind”).  
* Then, consider a “Barney appearance” to consist of the three records before the first instance of barney_ind=1, the three records after the last instance of barney_ind=1, and all records in-between as long as there is no gap of 6 consecutive records or more that are not credited to Barney.  
* So, let’s run through a couple of examples. 
