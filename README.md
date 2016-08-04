# bug\_notes
Notes on common bugs. 

# Cross Site Scripting (XSS)
### Location: 
Places where a site allows users to add content. For example, posting comments.
This is due to failing to sanitize user input.

### Example (www.hackspaining.com):
In a badly designed site, if someone tries to post the comment:

```
<script>malicious_code</script>
```

the site will run the malicious code instead of posting it as text. This allows
a malicious user to run arbitrary commands and potentially steal user creds
or site info. 

### Variants:
- Reflected XSS: Executed as soon as the injected code is processed. One-off.
- Stored XSS: Stored by the web application. Executed every time the malicious
data is loaded onto a page. This could happen, for example, if user comments
are stored for later viewing. 
- DOM-based XSS: When the DOM loaded by the page is dependent upon a given
field, such as `http://www.some.site/page.html?default=English.` In this case,
to exploit the vulnerability, one would set `default=<script>malicious_code</script>`

# CSRF

# Injection
