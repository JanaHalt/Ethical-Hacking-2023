
Uusi viikko, uudet läksyt. Alkuperäiset tehtävänannot löytyvät <a href="https://terokarvinen.com/2023/eettinen-hakkerointi-2023/#h4-totally-legit-sertificate">täältä</a>. Suoritan tehtävän kannettavallani Acer Swift 3, Windows 11 Home, sekä VirtualBoxissa asennetulla virtuaalikoneella (Kali).

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

**Access control** is dependent on authentication and session management:

- *authentication*: confirms indentity

- *session management*: identifies which HTTP requests are made by  the same user

- *AC* determines whether the user is allowed to carry out the action that they are attempting to perform

**Vertical access controls** are mechanisms that restrict access to sensitive functionality to specific types of users.

**Horizontal access controls** are mechanisms that restric access to resources to specific users.

**Context-dependent access controls** restric access to functionality and resources based upon the state of the application or the user's interaction with it.

**Broken access** control vulnerabilities exit when a user can access resources or perfomr action that they are not supposed to be able to.

**Vertical privilege escalation** - if a user can gain access to functionality that  they are not permited to access (e.g. application doesn't enforce any protection for sensitive functionality).

**Horizontal privilege escalation** - if a user is able to gain access  to resources belonging to another user, instead of their own resources of that type (e.g. if an employee can access the records of other employees as well as their own).

Horizontal privilege escalation attack can be turned into a vertical privilege escalation by compromising a more privileged user.

**Insecure direct object references** (IDORs) are subcategory of access control vulnerabilities. -> if an application uses user-supplied input to access objects directly and an attacker can modify the input to obtain unauthorized access.

**AC vulnerabilities'** prevention:

- deny access by default (unless resource is intented to be publicly accessible)

- obfuscation alone is not sufficient for access control

- use a single mechanism for enforcing access controls throughout the application whenever possible

- audit and test access controls to ensure they work as designed

#### <a href="https://portswigger.net/web-security/server-side-template-injection">Server-side template injection</a>

- when attacker is able to use native template syntax to inject a malicious payload into a template, which is then executed server-side

- server-side template injection vulnerabilities can explose websites to a variety of attacks depending on the template engine in question and how exactly the application uses it ***!!!*** at worst - an attacker can achieve remote code execution

- server-side template injection vulnerabilities arise when user input is concatenated into templates rather than being passed in as data

Constructing a **server-side template injection** attack:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/07718efc-8ac5-4268-9e84-0e09c928d7cf)

**How to prevent?** Best way is to not allow any users to modify or submit new templates. Other ways, examples: use a "logic-less" template engine when possible; only execute users' code in a sandboxed environment; apply your own sandboxing by deploying your template environment in a locked-down Docker container

#### <a href="https://portswigger.net/web-security/ssrf">Server-side request forgery (SSRF)</a>

- a web security vulnerability that allows an attacker to cause the server-side application to make requests to an unintended location

- successful SSRF attack can result in unauthorized actions or access to data within organization

- in SSRF attack against the server, the attacker causes the application to make an HTTP request back to the server that is hosting the application, via its loopback network interface

- in some cases, the application server is able to interact with back-end systems that are not directly reachable by users

- internal back-end systems often contain sensitive functionality that can be accessed without authentication by anyone who is able to interact with the systems

- common **SSRF defenses** are **SSRF with blacklist-based input filters** and SSRD with whitelist-based input filter

- it is sometimes possible to bypass filter-based defenses by exploiting an open redirection vulnerability

- **Blind SSRF vulnerabilities** - if you can cause an application to issue a back-end HTTP reques to a supplied URL, but the response from the back-end request is not returned in  the application's front-end response

- finding hidden attack surface for SSRF vulnerabilities: partial URLs in requests, URLs within data formats; SSRF via the Referer header

#### <a href="https://portswigger.net/web-security/cross-site-scripting">Cross-site scripting (XSS)</a>

- allows an attacker to compromise the interactions that user have with a vulnerable application

- XSS works by manipulating a vulnerable web site so taht it returns malicious JavaScript to users -> when the malicious code executes inside a victim's browse, the attacker can fully compromise their interaction with the application

- ***three main types of XSS**
  
  - reflexted xss
 
  - stored xss (~persistent/second-order)
 
  - DOM-based xss:
 
- XSS use cases, attacker typically can:

  - impersonate or masquerade as the victim user
 
  - carry out any action that the user is able to perfrom
 
  - read any data that the user is able to access
 
  - capture the user's login credentials
 
  - perfrom virtual defacement of the web site
 
  - inject trojan functionality into the web side
 
- for finding and testing XSS vulnerabilities you can use e.g. Burp Suite's web vulnerability scanner, but performing manual testing is also possible

- prevention of XSS attacks

  - filter input on arrival
 
  - encode data on output
 
  - use appropriate response headers
 
  - content security policy

### <a href="https://terokarvinen.com/2023/webgoat-2023-4-ethical-web-hacking/">Karvinen 2020: Using New Webgoat 2023.4 to Try Web Hacking</a>

- article tested on Debian 12-Bookworm

- first install Java and a Firewall, then download latest WebGoat JAR

  - so that you can run Java archive files
 
  - using firewall might be a good idea, even when not installing highly vulnerable apps
  - most likely you can find latest WebGoat from Github

- run WebGoat, in an alternative port (means other than port 8080 :) )

## a) Totally Legit Sertificate

*Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi. Laita ZAP proxyksi selaimeesi. Osoita, että hakupyynnöt ilmestyvät ZAP:n käyttöliittymään. (Ei toimi localhost:lla ilman Foxyproxya)*

Aloitin päivittämällä virtuaalikonettani (Kali) ```sudo apt-get upgrade```. Sitten ```sudo apt install zaproxy```.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/d43c2d3a-8c3c-4e97-af53-38d382117826)

CA-sertifikaatin generointi:

- ZAP:issa -> Tools -> Options -> Network -> Server Certificates -> Generate

- tallensin kotikansioon:

  ![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/e6e2faf0-1c53-46fb-9aec-78ab2ddce0ef)

Sitten selaimessa:

- Settings -> Privacy & Security -> Certificates -> View Certificates

- Authorities -> Import

...ja ruksataan "Trust this CA to identify websites/email users"

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/d8c9a877-7f59-4101-9c0b-f8b04fd0cf62)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/d0313c90-1b26-42c7-9812-1b93502250e0)

- sertifikaatti nyt näkyy selaimeen asennettujen sertifikaattien listalla:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/89522e2a-e138-483e-9bfe-3133e105293a)

Sitten vielä asetetaan zap proxyksi selaimeen:

Eli *settings* -> network settings (connection settings) -> *manual proxy configuration*

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/0f611fdc-c47e-4265-80bb-cb4a06317fee)

Yllä oleviin asetuksiin laitettavat arvot voi tarkistaa ZAPista, Tools -> Options -> Network -> Local Servers/Proxies: 

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/4b65943e-c02b-4e3f-9e93-ec8368046abf)

Toimivuutta testaan seuraavan osatehtävän kohdalla (b, Kettumaista; kts alla)

## b) Kettumaista

*Asenna FoxyProxy Standard Firefox Addon, ja lisää ZAP proxy siihen.*

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/3aaf47ea-983e-4d66-b0bd-0018e0cf6e2f)

Klikkasin ***Add to Firefox*** ja pop-up ikkunassa, joka sitten pomppasi ruudulle ***Add***.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/87a3efaf-4591-4d80-8d26-14c1b329cdc3)

Lisättiin vielä proxyt niin HTTP:lle kuin HTTPS:lle - kummallekin samoilla tiedoilla kuin kuvassa alla:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/ba9eadfe-02e1-4710-8eb6-8a2bbc20d17a)

Hakupyynnöt nyt ilmestyvät ZAP:n käyttöliittymään:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/68cec97e-63cf-44fe-a71f-c7d511f509ea)


## PortSwigger Labs

*Ratkaise tehtävät. Selitä ratkaisusi: mitä palvelimella tapahtuu, mitä eri osat tekevät, miten hyökkäys löytyi, mistä vika johtuu. (Voi käyttää ZAPia, vaikka malliratkaisut käyttävät harjoitusten tekijän maksullista ohjelmaa)*

### c) <a href="https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references">Insecure Direct Object Reference (IDOR)</a>

## Path traversal

### d) <a href="https://portswigger.net/web-security/file-path-traversal/lab-simple">File path  traversal, simple case</a>

Path traversal hyökkäyksessä yritetään päästä käsiksi kansioihin/tiedostoihin, jotka sijaitsevat juurikansion ulkopuolella. Sellaisiin resursseihin lienee mahdollista päästä get pyynnön (hae -> hakupyyntö) polkua muokkaamalla. 

Avasin ZAPin kirjoittamalla komentorivillä ```zaproxy``` -> File -> New session. PortSwiggerin sivuilla olin jo äsken klikannut *Access the lab* painiketta, joten minulla oli jo "verkkokaupan" sivu auki. Jotta saisin hakupyyntöjä ilmestymään ZAPin liittymään latasin kaupan sivun uudelleen. Kuten alhaalla olevassa kuvassa näkyy, ZAPiin tuli hakupyyntöjä näkyviin. Suodatin tulokset sanalla "image", jotta sain vain kuviin liittyvät pyynnöt. Kuten myös kuvassa näkyvä GET pyyntö liittyy kuvan (*/image?filename=47.jpg*) hakemiseen palvelimelta.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/455693c9-e5d7-4372-beef-ac19f5ae2b0a)

En oikein ollut varma, miten pääsisin muokkaamaan hakupyynnön polkua, joten katsoin <a href="https://www.youtube.com/watch?v=XhieEh9BlGc">File Path Traversal, simple case</a> videosta vinkkiä. Vähän meinasi hämätä, kun siinä käytetään BurpSuitea, mutta hetken ihmeteltyäni löysin oikean kohdan myös ZAPissa :) Joten klikkasin sitä pyyntöä hiiren oikealla -> *Open in Requester Tab*. Ekana kokeilin muuttaa ***filename*** parametria: 47.jpg -> /etc/passwd. Sitten kokeilin mennä yhtä kansiotasoa ylemmäs: /etc/passwd -> ../etc/passwd. Tulos oli sama. Jatkoin sammalla tavalla, kunnes löytyi oikea ratkaisu (toinen kuva alempana). Muokkaamalla filename-parametria tiedostonnimestä kansiopoluksi kerrottiin selaimelle mistä ME halutaan, että se hakee vastausta get hakupyyntöön - eikä sieltä mistä "verkkokauppasivun" tekijä suunnitteli.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/da844739-18ad-4c6f-aff7-93fadde24a82)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/c499bb5b-e0b6-43b5-9f51-47bfd053a6f4)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/d29cf3f9-73f7-4d16-bacf-9cb1f92f53ce)

### e) <a href="https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass">File path traversal, traversal sequences blocked with absolute path bypass</a>

Kuten PortSwiggerin sivulla lukee, niin monessa sovelluksessa, joka vie käyttäjän syötteen osaksi tiedostopolkua, on käytössä suoja path traversal tyyppisiä hyökkäyksiä vastaan. Joskus sitä on kuitenkin mahdollista ohittaa, esimerkiksi korvaamalla tiedoston nimeä absoluuttisella polulla juurikansiosta lähtien. 

Tässäkin harjoituksessa etenin samalla tavalla kuin edellisessä harjoituksessa. Klikkasin *Access the lab* painiketta ja ZAPissa suodatin sinne tulleet pyynnöt sanalla "image". Klikkasin hiiren oikealla hakupyynnön URLia ja painoin *Open in requester tab*. Kokeilin muokata get hakupyynnön URLia *filename=image8.jpg* sellaiseksi, että vaihdoin "image8.jpg"n polun tilalle ```/etc/passwd```. Ja se olikin heti oikea:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/6bdc1e10-a985-48de-a09d-16b6cb41a996)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/e3b12f9e-941e-4898-8971-abec349a13ca)

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

  - Server-Side Request Forgery (2)

### p) Client side (WebGoat 2023.4)

### Vapaaehtoiset:

*Ratkaise lisää WegGoat- ja/tai PortSwigger Labs-tehtäviä*



### Lähteet

https://owasp.org/Top10/A01_2021-Broken_Access_Control/"

https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/

https://portswigger.net/web-security/access-control

https://portswigger.net/web-security/ssrf

https://portswigger.net/web-security/cross-site-scripting

https://terokarvinen.com/2023/webgoat-2023-4-ethical-web-hacking/

https://www.zaproxy.org/docs/desktop/start/proxies/

https://owasp.org/www-community/attacks/Path_Traversal

https://www.youtube.com/watch?v=XhieEh9BlGc
