# Google Dork

## Api
   - site:target.tld inurl:api
   - site:target.tld intitle:"index of" "api.yaml"
   - site:target.tld inurl:/application.wadl
   - site:target.tld ext:wsdl inurl:/%24metadata
   - site:target.tld ext:wadl
   - site:target.tld ext:wsdl
   - user filetype:wadl
   - user filetype:wsdl

## Information Disclosure 
   - site:http://target.com intitle:index.of
   - 
## Directory Listing
   - site:target intitle:index.of
## Configuration Files Exposed
   - site:target ext:xml | ext:conf | ext:cnf | ext:reg | ext:inf | ext:rdp | ext:cfg | ext:txt | ext:ora | ext:ini | ext:env
## Database files exposed
   - site:target ext:sql | ext:dbf | ext:mdb
## Log files exposed
   - site:target ext:log
## Backup and old files
   - site:target ext:bkf | ext:bkp | ext:bak | ext:old | ext:backup
## SQL errors
   - site:target intext:"sql syntax near" | intext:"syntax error has occurred" | intext:"incorrect syntax near" | intext:"unexpected end of SQL command" | intext:"Warning: mysql_connect()" | intext:"Warning: mysql_query()" | intext:"Warning: pg_connect()"
## PHP errors / warnings
   - site:target "PHP Parse error" | "PHP Warning" | "PHP Error"
## phpinfo()
   - site:target ext:php intitle:phpinfo "published by the PHP Group"
## Signup
   - site:target inurl:signup | inurl:register | intitle:Signup
## Signin
   - site:target inurl:login | inurl:signin | intitle:Login | intitle:"sign in" | inurl:auth
