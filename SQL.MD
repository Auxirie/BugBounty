# SQL Injection

## All Kind of SQL Injection 
   - **Boolean-Based Blind**
   
we can manipulate the behavior of the sort depending on internal data. For example: if the admin username starts with an ‘a’ we sort the IDs in the query ASC (ascending) if not we sort them DESC (descending). This allows one to enumerate every single character of the username by just looking at the sort direction of the output.

   - **Stacked Queries**

we might add our own queries to the query. This one is tricky and quite dangerous. We are allowed to extend the query with new queries. This might allow one to [delete database](https://stackoverflow.com/questions/12403662/how-to-remove-all-mysql-tables-from-the-command-line-without-drop-database-permi) content, start [reverse shells](https://security.stackexchange.com/questions/6919/leveraging-a-shell-from-sql-injection) or easily create time based SQL attacks (more about that below) in order to retrieve data.

   - **Time-Based Blind**

most databases support the SLEEP() function. A function that pauses the execution of the query for a certain amount of seconds. We can use this to ex filtrate data by measuring the time it takes for the server to send a reply back. For example if the current database username of the server starts with a X we trigger a sleep for 2 seconds, if it starts with a Z a sleep for 4 seconds, etc. Repeat this for every position in the string and you can easily reconstruct the full username by looking at the response times of the request.

## Reference
   - https://medium.com/@jonathanbouman/blind-sql-injection-at-fasteditor-hema-com-6ac140c0d1a3
