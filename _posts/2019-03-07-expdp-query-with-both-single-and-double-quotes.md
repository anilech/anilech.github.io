---
layout: post
title: "expdp query with both single and double quotes"
date: 2019-03-07
---

![kdpv]({{ "/assets/linz_donau.jpg" | relative_url }})

To export/import subset of data data pump utilities use QUERY clause to define the subset. Recently i need to export partial data from a table with lower case column names and the filter has to be defined on a date column. After multiple attempts of trying to escape quotes with backslash character and receiving a lot of error messages like

```
ORA-39001: invalid argument value
ORA-39035: Data filter SUBQUERY has already been specified.

ORA-31655: no data or metadata objects selected for job

ORA-31693: Table data object XXX failed to load/unload and is being skipped due to error:
ORA-01858: a non-numeric character was found where a numeric was expected

LRM-00111: no closing quote for value
LRM-00113: error when processing file 'parfile.par'

UDE-00014: invalid value for parameter, 'query'.
```

i have found the solution - you need to double the single quotes:

```
QUERY='"lowercase_table":where "lowercase_date_column" <= DATE''2019-01-01'''
```

The whole QUERY clause is surrounded with single quotes, the table name and the column name is surrounded by double quotes, and the DATE literal is surrounded by double single quotes. This way it works correctly.

Hope it helps!
