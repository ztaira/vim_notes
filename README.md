# bug\_notes
Notes on common bugs. 

# Cross Site Scripting (XSS)
### Explanation 
When designing a site, one must be careful to sanitize user input in places where
a site allows users to add content. If the user input is not sanitized, it could
allow users to execute arbitrary code for malicious purposes.

### Example
In a badly designed site, if someone tries to post the comment:

```
<script>malicious_code</script>
```

the site will run the malicious code instead of posting it as text. This allows
a malicious user to run arbitrary commands and potentially steal user creds
or site info. 

### Variants
- Reflected XSS: Executed as soon as the injected code is processed. One-off.
- Stored XSS: Stored by the web application. Executed every time the malicious
data is loaded onto a page. This could happen, for example, if user comments
are stored for later viewing. 
- DOM-based XSS: When the DOM loaded by the page is dependent upon a given
field, such as `http://www.some.site/page.html?default=English.` In this case,
to exploit the vulnerability, one would set `default=<script>malicious_code</script>`
This would allow said user to inject their code through the DOM.

### Prevention
- Sanitize your input! 

# Cross-Site Request Forgery (CSRF)
### Explanation
Cross-site request forgery happens when a third party tricks a user to execute
unwanted actions on a web application where they're currently authenticated.
This allows malicious users to control the action of legitimate users' accounts.

### Example
Let's say a site allows users to post things via URL. Therefore, visiting
`www.example.com/post?message=newpost` will cause a user to post the message,
'newpost'. 

In this scenario, if a malicous user creates the url
`www.example.com/post?message=badpost` and gets an innocent user to visit
said url, the innocent user will post 'badpost' from THEIR account, not the 
malicious user's. By changing the contents of the URL, the malicious user
can do many unwanted things with the innocent user's account.

### Variants

### Prevention
- Ensure GET requests don't have any side-effects
- Use anti-forgery tokens to make sure that the POST requests are actually
from the intended user
- Include additional authentication for sensitive action

# SQL Injection
### Explanation
This bug occurs when a site fails to sanitize user input, allowing a malicious
user to run arbitrary commands against a site's database. This can lead to
things like modifying/dumping a database, logging in as another user, or
injecting malicious code.

### Example
A user is at an authentication page. In a badly designed site, trying to login
with the password `password' or 1=1--` will allow a malicious user to login
as anybody else without having to provide a legitimate password.

This is because, when the site creates the SQL query, it now looks like:
`SELECT * FROM users WHERE email = 'user@email.com' AND pass = 'o' or 1=1`.

This query returns the user with email = user@email.com and pass = 'o', or
email = user@email.com and 1=1. Since 1=1 is always true, the site will behave
as if the malicous user inputted user@email.com's legitimate password.

### Variants
Injection has too many variants to list here.

### Prevention
- Parametrize your inputs so that the database looks for the password
`password' or 1=1--` as a single string, rather than constructing a potentially
harmful SQL statement
- Sanitize your inputs
- Use object-relational mapping tools (ORM)
