# Account Takeover 

  - Login only with password
    1. Login as usual user
    2. Intercept the request and remove user parameter
    3. If you're lucky, the backend will take the password alone, check it against the database, and log you into the first matching account, which leads to account takeover
    4. or replace username with a wildcard or % to match a LIKE query
    5. Matching all string start at a (a% or a*) matching all string end at a (%a or \*a) Underscore (\_) represent 1 charachter  
  - 
