Uusi viikko, uudet läksyt. Alkuperäiset tehtävänannot löytyvät <a href="https://terokarvinen.com/2023/eettinen-hakkerointi-2023/#h4-totally-legit-sertificate">täältä</a>.

## Lue/katso ja tiivistä

### OWASP 2021: OWASP Top 10:2021

#### <a href="https://owasp.org/Top10/A01_2021-Broken_Access_Control/">A01:2021 - Broken Access Control</a> (IDOR ja path traversal ovat osa tätä)

Access control (AC) enforces principle of least privilege or deny by default. If AC failt, it usually leads to unauthorized information disclosure, modification or destruction of all data/performing a business. Typical AC vulnerabilities:

- violation of the principle of least privilege/deny by default

- bypassing AC checks by modifying the URL, internal application state, HTML page, modifying API requests using an attack tool

- permitting viewing or editing someone else's account by providing its unique identifier

- accessing API with missing access controls for POST, PUT and DELETE

- elevation of privilege

- metadata manipulation
- CORS misconfiguration allows API access from unauthorized/untrusted origins

- force browsing to authenticated pages as an unauthenticated user/to privileged pages as a standard user

##### How to prevent

- deny by default (except for public resources)

- implement AC mechanisms once and re-use them through the application, incl. minimizing cross-origin resource sharing (CORS) usage

- model ACs should enforce record ownership

- unique application business limit requirements should be enforced by domain models

- disable web server directory listing and ensure file metadata and backup files are not within web r oots

- log AC failures, alert admins when appropriate

- rate limit API and controller access to minimize the harm from automated attack tooling

- stateful session identifiers should be invalidated on the server after logout

Developers and QA (quality assurance) staff should include functional AC unit and integration tests.

#### <a href="https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/">A10:2021 - Server-Side Request Forgery (SSRF)</a>

- **SSRF** flaws occur whenever a web application is fetching a remote resource without validating the user-supplied URL.

- it allows attacker to force the application to send a crafted request to an unexpected destination, even when protected by firewall/VPN/another type of network access control list (ACL)

##### How to prevent

- network layer:

  - segment remote resource access functionality in separate networks
  - "deny by default" firewall policies/network access control rules
 
- application layer:

  - sanitize and validate all client-supplied input data

  - enforce the URL schema, port, and destination with a positive allow list

  - do not send raw responses to clients

  - disable HTTP redirections

  - be aware of the URL consistency

- additional measures:

  - do not deploy other security relevant services on front system. Control local  traffic on these systems.
 
  - use network encryption (e.g. VPN) where needed (e.g. frontends with dedicated and manageable user groups)

### PortSwigger Academy:

#### <a href="https://portswigger.net/web-security/access-control">Access control vulnerabilities and privilege escalation</a> (IDOR on osa tätä)

#### <a href="https://portswigger.net/web-security/server-side-template-injection">Server-side template injection</a>

#### <a href="https://portswigger.net/web-security/ssrf">Server-side request forgery (SSRF)</a>

#### <a href="https://portswigger.net/web-security/cross-site-scripting">Cross-site scripting</a>

### <a href="https://terokarvinen.com/2023/webgoat-2023-4-ethical-web-hacking/">Karvinen 2020: Using New Webgoat 2023.4 to Try Web Hacking</a>

## a) Totally Legit Sertificate

*Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi. Laita ZAP proxyksi selaimeesi. Osoita, että hakupyynnöt ilmestyvät ZAP:n käyttöliittymään. (Ei toimi localhost:lla ilman Foxyproxya)*

## b) Kettumaista

*Asenna FoxyProxy Standard Firefox Addon, ja lisää ZAP proxy siihen.*

## PortSwigger Labs

*Ratkaise tehtävät. Selitä ratkaisusi: mitä palvelimella tapahtuu, mitä eri osat tekevät, miten hyökkäys löytyi, mistä vika johtuu. (Voi käyttää ZAPia, vaikka malliratkaisut käyttävät harjoitusten tekijän maksullista ohjelmaa)*

## c) <a href="https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references">Insecure Direct Object Reference (IDOR)</a>

## Path traversal

### d) <a href="https://portswigger.net/web-security/file-path-traversal/lab-simple">File path  traversal, simple case</a>

### e) <a href="https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass">File path traversal, traversal sequences blocked with absolute path bypass</a>

### f) <a href="https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively">File path traversal, traversal sequences stripped non-recursively</a>

## Server-Side Template Injection (SSTI)

### g) <a href="https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-with-information-disclosure-via-user-supplied-objects">Server-side template injection with information disclosure via user-supplied object</a>

## Server-Side Request Forgery (SSRF)

### h) <a href="https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost">Basic SSRF against local server</a>

## Cross Site Scripting (XSS)

### i) <a href="https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded">Reflected XSS into HTML context with nothing encoded</a>

### j) <a href="https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded">Strored XSS into HTML context with nothing encoded</a>

## k) Asenna Webgoat 2023.4

*Uusi versio, jossa on eri tehtäviä kuin vanhemmissa.*

## Ratkaise WebGoat 2023.4

### m) (A1) Broken Access Control (WebGoat 2023.4)
  
  - Hijack a session (1)

  - Insecure Direct Object References (4)

  - Missing Function Level Access Control (3)

  - Spoofing an Authentication Cookie (1)

### n) (A7) Identity & Auth Failure (WebGoat 2023.4)

  - Authentic Bypasses

  - Insecure Login (1)

### o) (A10) Server-Side Request Forgery (WebGoat 2023.4)


### Lähteet

https://owasp.org/Top10/A01_2021-Broken_Access_Control/">A01:2021 - Broken Access Control



  - Server-Side Request Forgery (2)

### p) Client side (WebGoat 2023.4)

### Vapaaehtoiset:

*Ratkaise lisää WegGoat- ja/tai PortSwigger Labs-tehtäviä*
