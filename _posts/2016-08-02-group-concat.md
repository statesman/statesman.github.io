---
layout: post
title:  "Using GROUP_CONCAT in SQL to see multiple field values"
date:   2016-08-02 21:12:00 -0500
categories: data analysis
tags: sql, mysql
byline:
    - name: "Christian McDonald"
      twitter: "crit"
---

I came across this SQL command that came in quite useful. It's not that advanced of a query and I probably should've learned it long ago, but it's been a game changer in a recent project.

Say you have a data set like this in `test.data`:

```
| User_ID | Name         |
|--------:|--------------|
|     001 | Bob S.       |
|     001 | Bobby Smith  |
|     001 | Robert Smith |
|     002 | Joe Adams    |
|     002 | J. Adams     |
```

You want to know how many records each person has, but their names are not consistent. You don't want to just get a count of the `User_ID`, because you won't know who they are, and you want to capture all the different ways the name is spelled, but you don't want a different record for each different name.

``` sql
SELECT
  User_ID,
    GROUP_CONCAT(Name) as Names,
    count(*) as Records
FROM test.data
GROUP BY User_ID
```

You get a result like this:

```
| User_ID | Names                           | Records |
|--------:|---------------------------------|---------|
|     001 | Bob S.,Bobby Smith,Robert Smith | 3       |
|     002 | Joe Adams,J. Adams              | 2       |
```

That was syntax from MySQL using MySQL Workbench. I've also used this with [Google BigQuery](https://cloud.google.com/bigquery/), but I could also set the separator in a nicer way:

`GROUP_CONCAT(unique(Name), '; ') as Names`

That gave me a nice, semi-colon separated list of `Names`.






