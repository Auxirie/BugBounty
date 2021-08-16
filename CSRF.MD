# CSRF

## Tips And Trick

   - Change the method POST => GET , GET => POST and vice cersa
     1. after change the method try to combine the techniques 
   - Change single char CIwNZNlR4XbisJF39I8yWnWX9wX4WFoz => CIwNZNlR4XbisJF39I8yWnWX9wX4**P**Foz
   - Send empty value of token `csrf=CIwNZNlR4XbisJF39I8yWnWX9wX4WFoz` => `csrf=`
   - Replace with same length `csrf=CIwNZNlR4XbisJF39I8yWnWX9wX4WFoz` => `csrf=MJUSZNlR4XbisJF39I8yWnWX9wX4WFoz
   - Clickjacking
   - Use another user valid token
   - Try to decrypt
   - Analyze Token
   - Check for validated header like referrer, Origin,user-agent
   - CSRF via CSS injection and CRLF Injection
   - Put and Delete method csrf
     1. “_method” parameter.
Some frameworks have enabled _method by default even it’s not necessary to execute the Request
(Simply change the PUT and DELETE to POST and pass them in _method
     2. framework laravel,Django,rr version
