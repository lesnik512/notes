## Security Headers

### Content-Security-Policy (CSP)
- makes you list all of the sources for content (scripts, images, frames, fonts, etc.) that your site will use that are outside of your domain, which would stop a vulnerable web application from calling out and running the secondary part of the attack

```
Content-Security-Policy: default-src 'self'; block-all-mixed-content;
# default

Content-Security-Policy: default-src 'self'; img-src
https://*.wehackpurple.com; media-src https://*.wehackpurple.com;
# allows the browser to load images, videos, and audio from *.wehackpurple.com.

Content-Security-Policy: default-src 'self'; style-src
https://*.jquery.com; script-src https://*.google.com;
# allows the browser to load styles from jquery.com and scripts from google.com.
```

### X-Frame-Options
- is deprecated and Content-Security-Policy (CSP) is used in its place for modern browsers
- is used for backward compatibility of older browsers

```
# To allow the site to use frames from within your own domain:
X-Frame-Options: SAMEORIGIN

# To allow no frames:
X-Frame-Options: DENY
```

### X-Content-Type-Options
- instructs a browser not to guess the content type of media

```
X-Content-Type-Options: nosniff
```

### Referrer-Policy
```
Referrer-Policy: origin  # send only protocol and domain
Referrer-Policy: strict-origin-when-cross-origin  # send only protocol and domain if leaving domain
Referrer-Policy: no-referrer  # do not send anything
```

### Strict-Transport-Security (HSTS)
- forces the connection to be HTTPS

### Set-Cookie
- is used to send a cookie from the server to the user agent, so the user agent can send it back to the server later
```
Ensures that your cookie will only be sent over encrypted (HTTPS) channels
  Set-Cookie: Secure; (plus all of your other settings)

Cannot be accessed via JavaScript; it can only be changed on the server side
  Set-Cookie: HttpOnly;

Expires: Jan 1, 2021
  Set-Cookie: Expires=Mon, 1st Jan 2021 00:00:00 GMT;
Max-age of 1 hour
  Set-Cookie: Max-Age=3600;

Set domains which can rsead
  Set-Cookie: Domain=app.NOTwehackpurple.com;

Set-Cookie: path=/YourApplicationsPath;
```

## Rules for Errors
The following is a list of error handling rules. Follow these rules to ensure your application never falls into an unknown state.
- Global exception handling mechanism
- Do not reveal to users stack traces, or other crash information
- Security-related errors (login fails, access control failures, server-side input validation failures) should issue a system alert
- Limit the maximum length of the information logged in order to prevent an overflow attack

## Glossary
1. Certificate Authority (often known as a CA): A trusted company or organization that verifies the identity of whoever is purchasing a certificate.
2. Electronic Frontier Foundation (EFF): An international non-profit organization that works to protect the privacy and other rights on the internet.
3. “Let’s Encrypt”: Run by the Electronic Frontier Foundation (EFF), this offers encryption certificates for free.
4. Wild Card Certificate: A “wild card” certificate covers all of your subdomains.

## Notes
1. "salt" is unique for user and stored with login
2. "pepper" is unique for app and stored in vault