# internationalized Domain Name (IDN) Homograph Attack

## Introduction

The internationalized domain name (IDN) homograph attack is used to form domain names that visually resemble legitimate domain names, albeit, using a different set of characters. For example, the IDN "xn--akmai-yqa.com" which appears in unicode as "akámai.com" visually resembles the legitimate domain name "akamai.com". Attackers often apply IDN homograph attacks to form domain names that are used for malicious purposes

IDN | Unicode | Legitimate Match
--- | ------- | ----------------
xn--alixpress-d4a.com | aliéxpress.com | aliexpress.com
xn--go0gl-3we.fm | go0glе.fm | google.com
xn--mazon-wqa.com | ámazon.com | amazon.com

## Report Reference 
   - https://hackerone.com/reports/861940
