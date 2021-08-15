# API Pentest 

## Tips And Trick
   - Look for older version `api/v3/something` Look for `api/v2/something` might more vulnerable and lack security implement
   - Look other way to authenticate an API,modern app may have many api endpoint for 1 functionality `/api/mobile/login` | `/api/v3/login` | `/api/magic_link`
   - If you are testing a JSON endpoint, always try to change one letter in the parameter names to make them invalid. I had quite a few cases where the server thrown back an error with all of the accepted parameters.
   - 
