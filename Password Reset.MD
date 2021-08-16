# Password Reset Functionality

## Tips And Trick
   - Analyze the Token and the pattern if its possible to achive ATO
   - Check for token leak in header especially in referrer header
   - Use Forwarded Host header if it works use ngrok to capture the request to achive 1 click ATO
   - Use HTTP Parameter Pollution
   - Intercept Response To valid one
     1. notice that the response manipulation must same with valid response Dont just change the verb and status code
