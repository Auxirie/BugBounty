# Java

## Exploit
   - most java web apps allow bypassing common LFI filtering rules by doing the following: http://domain.tld/page.jsp?include=..;/..;/sensitive.txt
   - If you find jsp page with no parameters. You can actually add path parameters using semicolon. Like this example.com/test.jsp;');alert(1)// & perform XSS. Apache tomcat support this
