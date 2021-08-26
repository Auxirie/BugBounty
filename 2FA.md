# 2FA Bypass

  - Try to other api request or action of functionality while in 2FA page or when you not enter the 2FA code yet
  - Same as above, but try to deacivate the 2FA method or you can use CSRF attack to deactivate Or Clickjacking
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
  - Lack of rate limit re-sending the code via SMS
    1. You want be able to bypass the 2FA but you will be able to waste money of the company
  - Using the same session start the flow using your account and the victims account. When reaching the 2FA point with both account, complete the 2FA with your account but do not access the next part. Instead of that, try to access to the next step with the victims account floe. If the back-end only set a boolean inside your sessions saying that you have successfully passed the 2FA you will be able to bypass the 2FA of the victim.
    1. Analyze the cookie
  - Find other vulnerability to pull response code
