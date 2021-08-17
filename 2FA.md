# 2FA Bypass

  - Try to other api request or action of functionality while in 2FA page or when you not enter the 2FA code yet
  - Same as above, but try to deacivate the 2FA method or you can use CSRF attack to deactivate
  - Delete parameter
  - Response Manipulation
  - Try both step 3 and 4
  - Reuse the token
  - Use token from another user
  - Check for token expiring lets say 1 day or 2 day its enough to crack the token
  - Send double token check if first and second token is valid
  - Use HTTP parameter pullution to get 1 token in fouble authenticator
  - Try Rate limiting and Brute force
  - Direct request to the page which comes after 2FA dont forget to change to referrer header to 2FA page
  - Test in other environtment to like android or IOS if have
