
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

- three main types of XSS
  
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

## Insecure Direct Object Reference (IDOR)

### c) <a href="https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references">Insecure Direct Object Reference (IDOR)</a>

Tässä tehtävässä yritetään päästä käsiksi toisen käyttäjän chat-logiin, jotta löytäisimme hänen salasanansa. Tehtävän "verkkokauppa" säilyttää chat logeja suoraan palvelimen tiedostorakenteessa ja hakee ne käyttämällä staattisia URLeja. 

Saadakseni tiedon, mikä on tiedostopolku/URL, mistä chat log löytyy, piti jutella sivuston kanssa sieltä löytyvän live chat-ominaisuuden kautta ja ladata transskripti. Sitten etsin ZAPista löytää POST pyyntö, joka viittasi siihen, että halusin ladata chatin transskriptin ja sen jälkeen tullut GET pyyntö, jonka vastauksena oli ko. transkripti/tekstitiedosto.

Seuraavaksi klikkasin hiiren oikealla tuota GET pyyntöää (kuvan alalaidassa GET pyyntö, jonka ID on 11) ja valitsin *Open/Resend with Request Editor*. Toisessa kuvassa nähdään, että sen GET pyynnön vastauksena oli chatin transkripti. Niinpä niin, siinä lukee *filename="15.txt"*, kesti hetken, että tajusin homman jujun :D - mutta sentään huomasin, että sivusto tallentaa ne chat-transkriptiot nimenä nouseva numerointi.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/21b2be02-effe-47ef-8ddf-3acd424a13ab)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/303c7c61-46bd-4559-baeb-36fa30978a0a)

Seuraavaksi sitten manual requestin editorissa menin request-välilehdelle ja muokkasin GET pyynnön URLin *15.txt*sta *0.txt*ksi. Vastauksena oli **No transcript**.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/522caa23-fa6e-4be2-9d7e-1dcee76deccc)

Seuraavaksi muutin *0.txt*sta *1.txt*ksi. Vastauksena Carlosin (käyttäjä, jonka salasanaa etsimme) chatin transkripti.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/89e35c1d-1e13-46f7-97b1-bb6e71d8aff9)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/f639e355-6cf3-4c0a-ad6e-7e92d77cbf2d)

Kopioin sieltä löydetyn salasanan **s1j84j6ipa7pyrcb58nh** ja menin kokeilemaan, onnistuuko kirjautuminen. Ja sehän onnistuikin :)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/a20d136e-89a0-4513-93f6-28306c4ead72)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/d9c880e5-7f35-4c94-af51-97e2664d48da)


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

Sovellus saattaa poistaa tai estää hakemistojen traversal sequences (läpikulkusekvenssit?) käyttäjän syöttämästä tiedostonimestä. Sitä saattaa olla mahdollista ohittaa käyttämällä "nested traversal sequences" (sisäkkäsiä läpikulkusekvenssejä), kuten ```....//``` tai ```....\/```, katso tarkemmin esim. <a href="https://portswigger.net/web-security/file-path-traversal">täältä</a>.

Tässäkin tehtävässä yritetään päästä käsiksi ```/etc/passwd```n sisältöön. Joten kuten aikaisemmassa tehtävässä, avasin labran ja katsoin ZAPiin tulleita GET pyyntöjä. Valitsin sellaisen, joka liittyi kuvan lataamiseen palvelimelta:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/1aebacc8-9cd2-4956-a6e4-81e3be827e8a)

Ensin yritin muokata *30.jpg*sta */etc/passwd*ksi. Tuloksena "No such file":

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/b218d42d-d079-440b-913c-39da7661793e)

Saman tuloksen sain kokeilemalla tiedoston nimen kohdalla ```....//etc/passwd```. Joten lisäsin "yhden tason" lisää -> ```....//....//etc/passwd```, tuloksena edelleen "No such file". Lisäsin vielä yhden tason -> ```....//....//....//etc/passwd```. Vastauksena vihdoinkin 

```HTTP/1.1 200 OK```

```Content-Type: image/jpeg```

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/a26ba6d1-c371-48a1-8b6b-f7d3d2f2bd8f)

## Server-Side Template Injection (SSTI)

Server-side template injection tarkoittaa, että hyökkääjä pystyy käyttämään alkuperäistä mallisyntaxia (template syntax) haitallisen hyötykuorman syöttämiseen mallisyntaxiin ja sitä sitten suoritetaan palvelimen puolella (server-side).

### g) <a href="https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-with-information-disclosure-via-user-supplied-objects">Server-side template injection with information disclosure via user-supplied object</a>

Kirjauduin "kauppaan" tehtävänannossa annetuilla tunnuksilla. 

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/d6885a53-855d-4d0b-83d0-a9a8951e61e5)

Ja koska SSTI:ssä hyökkääjä syöttää haitallista hyötykuormaansa jonkinlaisen user input kohdan kautta, sellaista sitten aloin etsimään. Toki kun kirjauduin, niin ekana siinä on kohta, jossa voi vaihtaa salasanan. Nyt kuitenkin etsittiin kohtaa, jonka kautta voisimme muokata malleja (templates). Menin katsomaan tuotesivuja ja sieltä löytyi lupaava kohta:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/c64318dd-5028-4df2-9513-ef1d1e0fe217)

Aloin muokkaamaan sitä ja kuten tehtävänannon ratkaisuohjeessa ehdotettiin, lisäsin editointi-ikkunaan ```${{<%[%'"}}%\```, eli jotain mitä siinä ei kuuluisi olla. Tallensin ja odotetusti error-viestissä oli viittaus siihen, että käytössä on django framework on käytössä. 

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/453ec508-e567-4f42-84e7-007138f58bd8)

Syötin tuon virheilmoituksen hakuun ja ystävämme bing ai-chatti ehdotti seuraavan:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/ffdeb790-f8c8-4cc3-8233-7b75f969cb04)

Tuon inspiroimana aloin tutkimaan djangon dokumentaatiota, hakusana ```template tag```:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/d286cd64-f7d0-42d1-8d64-f491e68d5a7a)

Hakutuloksista huomasin, että djangossa on template tagien kaava tällainen ```{% tag %}```. Koska olin parhaillaan tutkimassa aiemmin tullutta error-viestiä hain dokumentaatiosta sanaa ```debug```:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/56ec0e5d-b679-419e-ad51-7c8ec1cae138)

Laitoin templateen virheellisen ```${{<%[%'"}}%\```n sijaan ```{% debug %}```. Kuten tehtävän ratkaisuohjeessa sanottiin, tuloksena oli lista objekteja ja ominaisuuksia. Settings-sanaa sieltä löytyi myös, joten seuraavaksi tutkin sitä djangon dokumentaatiosta.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/6b7dabc8-ae3b-4d56-a954-0f46a087fbed)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/3342db5a-1c0a-416e-ac7f-058e303f4317)

Tuloksista klikkasin ***Django settings*** ja sitä selaillessa tulin kohtaan ***Available settings***. Joten rupesin tutkimaan, mitä siinä olisi tarjolla. Löysin kuin löysinkin ratkaisuohjeessa mainitun ```SECRET_KEY``` ominaisuuden. Jos hyökkääjä on tästä ominaisuudesta tietoinen, sitä voi hyödyntää siten, että syötetään sivulle (templaten kautta) haitallista koodia, jota sitten suoritetaan palvelimen puolella.

Jatkoin ratkaisuohjeen mukaan ja poistin ```{% debug %}``` templatesta ja laitoin tilalle ```{{settings.SECRET_KEY}}```. Tuloksena se mitä etsittiin, ***secret key***!!

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/353fd8d7-8c5d-46fc-81a8-ea43a4c7b1bb)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/15e2de1a-cc15-4fac-adbc-a39632daf3ab)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/22b66164-6ff3-4d5b-8297-63c2c99593e9)

YES!!! Ratkaistu :D

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/70121e5a-82af-472f-bb31-fc4d868908da)


Siitä huolimatta, että hyödynsin tehtävän ratkaisuohjetta, niin koen, että opin todella paljon.

## Server-Side Request Forgery (SSRF)

***SSRF*** on haavoittuvuus, jonka ansiosta hyökkääjä pystyy käskemään palvelimen puolella olevaa ohjelmaa lähettämään pyyntöjä muulle kuin tarkoituksenmukaiselle kohteelle. Järjestelmät eivät yleensä salli pääsyä admin sivulle ilman autentikointia. Hätätapauksia varten voi olla mahdollisuus päästä admin sivulle ilman tunnuksia, mikäli sinne mennään paikallisesti (localhost).

### h) <a href="https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost">Basic SSRF against local server</a>

Tavoitteena on päästä admin sivulle ja poistaa käyttäjä Carlos.

Kuten odotettu, ei päästä admin sivulle suoraan lisäämällä ```/admin``` URLin perään:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/4ce5b237-a6d3-45e2-acd7-7dd9a83930eb)

Adminilla on luultavasti pääsy katsomaan verkkokaupan saldoja yms. Joten menin yhden tuotteen sivulle ja painoin **check stock**. Sitten siirryin ZAPin puolelle tutkimaan siihen liittyvää POST pyyntöä. Ehkä en vain huomannut edellisissä harjoituksissa, mutta tällä kertaa ZAPissa oli myös APIiin liittyvä kohta. En ole mikään koodari, mutta eiköhän API on se välikappale käyttäjän (selaimen) ja itse ohjelman/tässä kaupan verkkosivun ja sen tietokannan, välissä. Joten arvo **stockApi** parametrissa viittanee siihen kohtaan tietokantaa, jossa sijaitsee tieto tarkasteltavan tuotteen varastosaldosta.

Mikäli sivustossa on SSRF haavoittuvuus (ja tässä tapauksessa toki on), tämä voisi olla väylämme päästä admin sivulle ja sitä kautta poistaa käyttäjä Carlos. Joten avasin kyseisen POST pyynnön **manual request editorissa**. Koska **localhost**ista voisi onnistua pääsy admin sivulle ilman tunnuksia, kokeilin sitä.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/e05c3e0b-3e99-48fc-9cd9-7e9a1ece2f13)

Onnistuin ja sieltähän löytyi myös keino poistaa käyttäjä Carlos.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/77c13718-92ca-4b57-82db-fb7a047afac8)

Joten kopioin tuon polun ```/admin/delete?username=carlos``` ja muutin (manual request editorissa) stockApi parametrin arvon tällaiseksi ```http://localhost/admin/delete?username=carlos``` ja painoin "Send".

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/b7e32373-ad8e-42d0-9573-9454a5d4a030)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/decc037d-40cf-4744-84c1-cd869c35dbc3)


## Cross Site Scripting (XSS)

<a href="https://portswigger.net/web-security/cross-site-scripting">Cross-site scripting (XSS)</a> on haavoittuvuus, jota hyödyntämällä hyökkääjä voi vaarantaa käyttäjän ja sovelluksen välistä kommunikaatiota. XSS haavoittuvuus antaa hyökkääjälle mahdollisuuden naamioitua uhriksi, suorittaa toisen käyttäjän komentoja/toimintoja sekä käyttämään kyseisen käyttäjän tietoja.

XSS:ssä manipuloidaan haavoittuvaa verkkosivua siten, että se palauttaa käyttäjälle haitallista (malicious) JavaScriptiä. Kun haitallinen koodi suoritetaan käyttäjän selaimessa, hyökkääjä voi rikkoa/murtautua/vahingoittaa käyttäjän ja selaimen/sovelluksen välisen kommunikaation täysin.

### i) <a href="https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded">Reflected XSS into HTML context with nothing encoded</a>

Tässä tehtävässä on yksinkertainen XSS haavoittuvuus hakutoiminnossa. Tehtävä tulisi suorittaa tekemällä XSS hyökkäys, joka kutsuu ```alert```funktiota. 

Koska tehtävänannossa sanottiin, että haavoittuvuus on hakutoiminnossa ja hyökkäys tehdään kutsumalla alert-funktiota, niin ajattelin yksinkertaisesti vain kirjoittaa hakukenttään ```alert()```. Tuolla tavallahan merkataan funktioita. Toteutin sen ***Manual Request Editorissa***, ZAP paussilla. Yritys epäonnistui -> ***lab not solved***. Seuraavaksi ajattelin laittaa ko. funktion syötteeksi jotain arvoa, kokeilin ```alert(2)``` (numeron valitsin ihan sattumalta). Sama tulos kuin edellä.

Mietin asiaa enemmän ja ajattelin, että hetkinen... haavoittuvuuden nimi on ***cross-site SCRIPTING***. Joten siinähän on oltava skripti. Googlasin ***script and search bar vulnerability***. Heti eka hakutulos oli <a href="https://security.stackexchange.com/questions/119989/typical-search-box-xss-attack">Typical search box xss attack</a>. Kurkkasin mitä sieltä löytyy ja päätin kokeilla sieltä löydettyä ```<script>alert(1)</script>``` verkkokaupan hakukentässä. Tässä tulos:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/672b0cd1-2c90-49b9-89ce-140f6e2bfe84)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/df888a1a-0af3-41dc-8f29-02f851d05319)

Eli verkkokauppa ei käsitellyt syötettäni pelkkänä tekstinä, vaan ```<script>``` tagin ansiosta skriptinä ja suoritti sen.

### j) <a href="https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded">Stored XSS into HTML context with nothing encoded</a>

***Stored XSS*** haavoittuvuus syntyy, kun selain vastaanottaa dataa epäluotettavalta lähteeltä ja sisällyttää sitä myöhemmissä HTTP vastauksissa (responses) vaarallisella tavalla. Stored XSS tunnetaan myös nimellä second-order XSS / persistent XSS.

Avasin yhden blogipostauksen ja kokeiluna jätin sinne yhden "tavallisen" kommentin. Vastauksena sain "Thank you..." ja kommenttini näkyi blogipostauksen alla:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/437b8fa5-5d5f-4075-b9de-8be1189933a9)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/30803df6-ac57-479c-94c3-100454ac89ab)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/502e6ad7-27e8-4649-964a-fe439ef45c8c)

Vaikuttaisi siltä, että sovellus/sivu ei käsittele dataa millään muulla tavalla kuin, että tulostaa sen ruudulle - eli tallentaa sen (sinne mihin ikinä tallentaakin käyttäjien kommentit ja näyttää sitten ruudulla). Joten ajattelin kokeilla kirjoittaa kommenttiin ```<script>```in, kuten kerrotaan myös <a href="https://portswigger.net/web-security/cross-site-scripting/stored">täällä</a>. Seuraavaksi siis kirjoitin tällaisen kommentin:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/0c6c915c-0cf5-4dcc-bb5d-2792eb1a7fa9)

***Lab solved***:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/c19d93e3-86b0-4685-8435-ca462061bdbf)

...josta todisteena, että kun menin takaisin blogiin, niin ruudulle tuli heti tämä:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/b7ed7ae1-f9c0-4f6f-b00e-3283527569b6)

:)

## k) Asenna Webgoat 2023.4

*Uusi versio, jossa on eri tehtäviä kuin vanhemmissa.*

WebGoatin asennuksessa seurasin <a href="https://terokarvinen.com/2023/webgoat-2023-4-ethical-web-hacking/?fromSearch=webgoat#profit">artikkelia</a>. Eli ensin päivitin virtuaalikoneeni ```sudo apt-get update```. Minun ei tarvinnut asentaa ```openjdk-17-jre```, sillä uusin versio oli jo valmiiksi asennettuna. Asensin palomuurin ```sudo apt-get install ufw``` ja enabloin sen ```sudo ufw enable```.

Latasin uusimman WebGoatin version ```wget https://github.com/WebGoat/WebGoat/releases/download/v2023.4/webgoat-2023.4.jar``` ja käynnistin sen ```java -Dfile.encoding=UTF-8 -Dwebgoat.port=8888 -Dwebwolf.port=9090 -jar webgoat-2023.4.jar```. Tuossa viimeisimmässä komennossa myös säädetään, että WebGoat tulee pyörimään portissa 8888, sillä portissa 8080 jo valmiiksi pyörii OWASP ZAP. Lopussa komentoriville tuli ohje mennä osoitteeseen ```http://127.0.0.1:8888/WebGoat```. Menin sinne suoraan ZAPista löytyvän firefoxin-selainpainikkeen kautta. 

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/e54814d5-8847-4f46-ab3d-557ff31a0876)

## Ratkaise WebGoat 2023.4

### m) (A1) Broken Access Control (WebGoat 2023.4)
  
  - Hijack a session (1)

Navigoin oikean tehtävän kohdalle:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/33366f61-11da-4e21-af56-f56696d19368)

Tehtävänannossa kerrotaan, että tavoitteena on "arvata" hijack_cookien arvo. Sen avulla erotellaan autentikoidut (sisäänkirjautuneet) ja anonyymit WebGoatin käyttäjät. 

Cookiet ovat istuntokohtaisia. Joten jokaisella cookiella lienee jokin istunto ID. Tarkistetaan se webdeveloper-toolista. Selaimessa painetaan ```F12``` ja siellä välilehdelle "Storage" -> "Cookies":

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/8b770878-eca5-4890-8a24-c09c4f023fe1)

Eli meidän kohdalla ```JSESSIONID:"zoDte3napacztrA-LEFgwqyI8b98dm6nX5tNYe8-"```. Painoin tehtäväsivulla ***Access*** nappia ja ZAPiin ilmestyi POST pyyntö, jonka URLissa luki mm. "HijackSession", joten arvelin että jotain tällaista pitäisi nyt etsiä. Avasin tuon POSTin *Manual Request Editor*issa, painoin *Send* ja Response-välilehdelle saatiin näkyviin hijack_cookien arvo:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/1fe919a3-738f-4c00-8a5e-ff5d7d671a63)

Painoin *Access* muutamaan kertaan ja tallensin ne arvot tekstieditoriin:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/40e01a38-502b-4f28-aa89-83de1fa709fc)

Tehtävävinkkien mukaan ensimmäinen osa on "sequential number" ja toinen "unix epoch time". Ne cookies, jotka tulee näkyviin, ovat "minun" sessioita. Niiden tulisi tässä tehtävässä mennä käsittääkseni järjestyksessä, eli sequential number (SN) pitäisi kasvaa aina 1:lla. Silloin kun yksi numero jää välistä, eli SN kasvaa kahdella, se tarkoittaa, että joku muu käyttäjä on kirjautunut siinä välissä.

En jotenkin meinannut päästä eteenpäin, joten etsin youtubesta jotain vinkkejä. Törmäsin yhteen <a href="https://www.youtube.com/watch?v=_MdvIWvh7rM">ratkaisuvideoon</a>. Tai no löytyi aika monta, mutta tuo yksi vaikutti fiksulta. Kaikissa niissä oli vain yksi ongelma. Niissä käytettiin burpsuitea. Joten en kovin hyvin osannut soveltaa, kun burpsuiten ominaisuudet ja valikot eivät ole välttämättä samannimisiä kuin ZAPissa. Löytyi <a href="https://www.zaproxy.org/docs/burp-to-zap-feature-map/">Burp to ZAP Feature Map</a>. Tuolla listalla oli myös yksi työkalu, jota käytettiin löytämässäni videossa, <a href="https://www.zaproxy.org/addons/">Token Generation and Analysis</a> - burpsuitessa se on *sequencer*.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/c6acaf78-73f6-456c-8788-c988a54810a8)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/3f7307ae-a94d-45f5-b831-f19fa7c75f28)

Latasin sen ja sitten ZAPissa File -> Load Add on.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/2ec4f49c-711d-4d9b-9e0d-04049f84b3a2)

Sitten vain klikkasin hiiren oikealla -> Generate Tokens. Haluttu määrä tokeneja generoitiin ja tallensin ne ne kansioon ```~/.ZAP/sessions``` nimellä ```gen_tokens1```. Jotain tein ilmeisesti väärin, koska se tallensi vain analyysin, eikä tokeneja:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/941a6dc9-1211-4861-8307-d231d2ef7c26)


  - Insecure Direct Object References (4)

Tämän tyyppiset hyökkäykset tapahtuvat tehtäväannon mukaan usein sellaisten käyttäjien toimesta, jotka ovat autentikoituja, mutta heillä ei kuitenkaan ole oikeutta suorittaa sitä mitä he yrittävät. 

Nyt aloitetaan. Kirjaudutaan sisään -> tulos: **You are now logged in as tom. Please proceed.**

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/184906af-7811-4594-81da-e246556473a8)

Selaimessa näkyy profiilissa *name*, *color*, *size*. ZAPissa (response) näkyy myös *role* ja *userId*.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/a4a3548f-c892-4ce7-b153-7cac01eafc23)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/590cd5e5-1207-414e-8e05-2e73710d9560)

Samasta paikasta löytyi vastaus seuraavaankin vaiheeseen:

```WebGoat/IDOR/profile/23423844```

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/21c99bf8-b1d5-437a-8023-c22958ee9462)

Seuraavan vaiheen kanssa tuli ongelmia. Tiesin, että pitää löytää toisen käyttäjän ID ja toisessa vaiheessa muokata tämän käyttäjän profiilia. Mietin tätä melko pitkään ja etsin mahdollisia ratkaisuja netistä. Katsoin ratkaisuvideoita youtubesta, esimerkiksi <a href="https://www.youtube.com/watch?v=8fMFLqbd0-Y">tämä - IDOR - WebGoat</a> tai tämä <a href="https://www.youtube.com/watch?v=B2xw7EB7hJg">toinen</a>. Löysin jopa WebGoatin GitHubista ratkaisun, <a href="https://github.com/WebGoat/WebGoat/wiki/Main-Exploits#insecure-direct-object-reference-lesson-5-exercise">WebGoat - Main Exploits - IDOR lesson 5 exercise</a>. En saanut kuin alhaalla näkyvät ilmoitukset. Kokeilin myös vaihtaa content-type "application/json":ksi (ei kuvassa), mutta sekään ei auttanut:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/e01cdcb2-0793-4cb0-af3e-dfd8958423b1)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/dec32cc6-bd71-41ac-b4b6-32c48d5e8315)

Kokeilin uudelleenkäynnistää kaiken: ZAPin, WebGoatin, virtuaalikoneen,... Ei apua. Kokeilin tehdä saman GET hakupyynnön muokkauksen myös developer toolseissa, mutta sama virhe:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/02672516-3f44-4b7a-8584-64e3ebb81423)


  - Missing Function Level Access Control (3)

Tässä yritetään löytää sivuston lähdekoodiin piilotettuja ominaisuuksia. Piti etsiä sellaisia, jotka voisivat olla mahdollisen hyökkääjän mielestä kiinnostavia. Avasin firefoxin developer tools painamalla **F12**. *Inspector* välilehdellä tutkin sivun lähdekoodia, kunnes löytyi jotakin kiinnostavaa:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/13a7ade5-5f6c-4e26-bec1-25eb3918b0cc)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/37725974-865b-4861-9aaf-067c3dc1592c)

Seuraavassa vaiheessa piti löytää Jerry nimisen käyttäjän "hash" arvo. Tehtäväsivulla, painoin "submit" ja etsin ZAPista tähän liittyvän pyynnön, kuva alla:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/03b3ddca-158a-4732-9d5f-3cbc2b10a4db)

Seuraavaksi avasin sen "manual request editorissa" ja muutin sen GET pyynnöksi ja lisäksi hyödynsin edellisessä vaiheessa löydetyt ```/access-control/users``` ja ```/access-control/config```. Muokkasin URLin loppuosan näin ```/WebGoat/access-control/users``` ja **content-type** parametrin arvoksi muutin ```application/json```. Myönnän, että vinkin content-type parametrin muuttamiseksi löysin tehtävän vinkeistä, mutta näin jälkeenpäin se on täysin järkeenkäyvä. Käyttäjäprofiilin tiedot ovat käsittääkseni usein tallennettuna json-formaatissa.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/ce77a5c5-ccd9-474f-b2ba-2c75867a653d)

Tässä tulos: Jerryn hash-arvo on ```SVtOlaa+ER+w2eoIIVE5/77umvhcsh5V8UyDLUa1Itg=```.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/3793f229-4e8d-49be-a009-8d2e6d97162a)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/cbe51e16-f06d-4496-8ff1-b9c00b2f08a1)

Viimeisessa vaiheessa on ajatuksena, että tuo äsken hyödynnetty haavoittuvuus korjattiin - eli piti lisätä uusi käyttäjä, kirjautua uutena käyttäjänä ja sitten löytää Jerryn hash-arvo. Hiukan apua tehtävän vinkeistä hyödynnettiin tässäkin :)

Ekana painoin "submit" nappulaa ja ZAPissa avasin siihen liittyvän POST hakupyynnön *manual request editorissa*. URL muokkasin tällaiseksi ```http://localhost:8888/WebGoat/access-control/users``` - eli halutaan päästä muokkaamaan sivuston "käyttäjiä". Content-type ```application/json```ksi, siinä formaatissa ovat käyttäjätiedot tallennettuna, ja payload ```{"username":"newUser2","password":"newUser12","admin": "true"}```. Viimeisin kohta tarkoittaa, että luodaan uusi käyttäjä nimellä *newUser2*, hänen salasanansa on *newUser12* ja hän on *admin*. Sitten vain "Send" nappulaa ja tuloksena on uusi käyttäjä:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/a506d8eb-ceb2-4431-b328-9e048d599530)

Viimeisessa kohdassa piti löytää uuden käyttäjän hash-arvo. Vinkeissä sanottiin, että pitäisi kirjautua uutena käyttäjänä (äsken luotu) Painoin "Submit" tehtäväsivulla ja avasin ZAPissa siihen liittyvän POST hakupyynnön "manual request editorissa". Muutin sen GET hakupyynnöksi ja URLin loppuosan muutin näin ```.../access-control/users```. Content-type vaihdoin tälläkin kertaa ```application/json``` ja sitten "Send" -> 

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/157ef158-64cd-495a-9c3c-1654d9842bfb)

Uuden käyttäjän, eikä Jerryn hash-arvo eivät kelvanneet ratkaisuksi. Pitkän googlettelun jälkeen löysin <a href="https://github.com/WebGoat/WebGoat/issues/1424">tämän GitHub ketjun</a>. Siinä joku valitti, että tämän osatehtävän vinkit ovat osittain jokseenkin harhaanjohtavia. Ajattelin samaa, mutta epäilin, etten vain osaa. Ketjun viimeisessä kommentissa neuvottiin luomaan uusi käyttäjä samalla nimellä kuin oikeasti käyttää webgoatissa ja asettaa sille admin-parametrille arvoksi "true". Sitten piti olla mahdollista listata käyttäjät kuten aiemmassa osatehtävässä, PAITSI nyt tulisi käyttää GET hakupyynnön URLin lopussa ```/users``` sijaan ```/users-admin-fix```.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/3b97d172-f684-470c-8031-f2e4580d1850)

Seurasin ohjetta ja muokkasin GET hakupyynnön sen mukaiseksi ja tulos oli...erilainen:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/c47e7a0e-2193-40d3-992b-7d93ef5cc13e)

Käyttäjällä Jerry oli tällä kertaa eri hash-arvo: ```d4T2ahJN4fWP83s9JdLISio7Auh4mWhFT1Q38S6OewM=```. Tämä sitten kelpasi ratkaisuksi myös webgoatille :)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/3f117d48-3b2d-4270-afb6-459ca9a79318)

  - Spoofing an Authentication Cookie (1)

Tässä tehtävässä pitäisi pystyä arvaamaan cookies:n luontialgorytmi ja ohittamaan autenkikaatio kirjautumalla sisään eri käyttäjänä.

Tehtävänannossa on kahden käyttäjän tunnukset:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/48c028cd-f77b-4113-8954-f1178600f8e7)

Kirjauduin ensimmäisenä (webgoat) ja uusi cookie käyttäjälle webgoat luotiin: ```spoof_auth=NDc3NzcyNDQ2YTc2NzY2ZDQ5NTI3NDYxNmY2NzYyNjU3Nw=```.

Painoin *Delete cookie*.

Sitten sama toisena käyttäjänä (admin): ```spoof_auth=NDc3NzcyNDQ2YTc2NzY2ZDQ5NTI2ZTY5NmQ2NDYx```.

Painoin *Delete cookie*.

Spoofaamalla cookie piti kirjautua käyttäjänä "Tom". Laitoin kirjautumislomakkeeseen pelkkä Tom, ilman mitään salasanaa. Toki, tuloksena oli ***Login failed***.

Uusi yritys.

Kirjauduin webgoat-käyttäjänä ja avasin siihen liittyvän POST hakupyynnön ZAPissa. Pysäytin ZAPin ja painoin "Send", jolloin tulos oli tämä:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/a1f15393-8244-4029-9950-996c83abcfe3)

Eli webgoat-käyttäjän spoof_auth on ```spoof_auth=NDc3NzcyNDQ2YTc2NzY2ZDQ5NTI2ZTY5NmQ2NDYx```. 

Edelleen "manual request editorissa", vaihdoin käyttäjätunnukset admin-tunnuksiksi ja painoin "Send". Tuloksena saatiin adminin spoof_auth ```NDc3NzcyNDQ2YTc2NzY2ZDQ5NTI2ZTY5NmQ2NDYx```. 

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/da5ca9a9-6d9b-4876-80e4-0bdb151322ed)

Ymmärsin kyllä, että niitä on luotu jollain salausmenetelmällä, mutta en tiennyt millä -> löysin tämän <a href="https://www.youtube.com/watch?v=-n4OmhUN3vA">videon</a>, jossa sanottiin, että salausmenetelmä oli ***BASE64***. En ole ihan varma, mistä sen tunnistaa. Joten sivulla <a href="https://www.base64decode.org">Base64decode</a> laitoin adminin spoof_auth:n ekaan tekstikenttään ja painoin *decode*. Tuloksena ```477772446a76766d49526e696d6461```.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/72948dd2-44b7-4150-b2b1-0e220923c435)

Videossa neuvottiin, että kun niitä numeroita jakaa 2 numeron pareiksi, niin nehän ovat hex-numeroita: ```47 77 72 44 6a 76 76 6d 49 52 6e 69 6d 64 61```.  Joten seuraavaksi käytettiin <a href="https://cryptii.com/pipes/hex-decoder">hex-dekooderiin</a>:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/01fadd91-7778-412c-8f2c-5532743d9431)

Tuloksesta ```GwrDjvvmIRnimda``` voi huomata, että rimpsun lopussa lukee "nimda" -> "admin". ja loput jotain satunnaisia numeroita. Koko rimpsu sitten laitettiin <a href="https://string-functions.com/reverse.aspx">string reverseriin</a>. Tuloksena ```adminRImvvjDrwG```:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/c4776aef-3383-4e84-91aa-918b35748572)

```adminRImvvjDrwG```:n vaihdoin ```TomRImvvjDrwG```:ksi, laitoin string reverseriin ja tuloksena sain ```GwrDjvvmIRmoT```. 

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/75477117-2b04-42a5-ab8b-193c53a6019e)

Sitten tuo arvo laitettiin hex-dekooderiin, mutta oikeanpuoleiseen tekstikenttään, jolloin vasemmalle tuli sen hex-arvo ```47 77 72 44 6a 76 76 6d 49 52 6d 6f 54```. Tämä sitten laitetiin (ilman tyhjiä välejä) <a href="https://www.base64encode.org">base64encodeen</a> ja tulos oli ```NDc3NzcyNDQ2YTc2NzY2ZDQ5NTI2ZDZmNTQ=```. 

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/f0381d85-55b1-423f-8e83-f1b33872dd71)

### n) (A7) Identity & Auth Failure (WebGoat 2023.4)

  - Authentic Bypasses

Tehtävässä ollaan kuvitteellisesti resetoimassa salasanaa. Käytössä on kaksivaiheinen autentikointi, mutta meillä ei ole mahdollisuutta saada tesktari, joten valitaan vaihtoehtoinen tunnistautuminen - turvakysymykset. Ongelmana on, että ne turvakysymykset on tallennetu toiselle laitteelle kuin mistä ollaan kirjautumassa, emmekä kuitenkaan edes muista vastauksia niihin.

Tehtäväsivulla syötin sanat "testi1" ja "testi2" vastauskenttiin. Painoin "Submit", jotta sain POST hakupyynnön ZAPiin. Avasin sen pyynnön "manual request editorissa" ja pysäytin ZAPin. Tässä nähdään parametrit, joihin haetaan lomakkeesta syötteitä ja niiden avulla pitäisi autentikoita käyttäjä:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/10c23ee8-5ce0-46f5-b9ed-560f2b690195)

Noita parametreja varmaan pitää jotenkin muokata, mutta en ole varma miten, joten kurkkaan vinkkeihin. Siinä kerrotaan, että sivusto odottaa, että vastataan kahteen kysymykseen, mutta toteutuksessa on virhe. Yhtenä vinkkinä oli myös **secQuestion** parametrien uudelleennimeäminen. Kokeilen sitä. Muutam ko. parametrien nimiksi **secQuestionA** ja **secQuestionB** ja painan "Send". Tehtävä ratkaistu. En ole ihan täysin varma miten tämä onnistui. En saanut mielenrauhaa, joten oli pakko tutkia miksi. Tässä vaiheessa löysin Youtubesta <a href="https://www.youtube.com/watch?v=hrEB9L2oyic">videon</a>, jossa kerrotaan, että sivusto ei odota juuri *näitä parametreja*, vaan se odottaa mitä vaan parametreja, jotka sisältävät sanan "SEC". 

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/ff5321af-e9a2-4f14-9ff2-1f29d020fdb6)

  - Insecure Login (1)

Tässä tehtävässä kaapataan toisen käyttäjän tunnuksia.

Tehtäväsivulla painoin ohjeiden mukaisesti "Log in" ja ZAPista sitten etsin tähän kirjautumisyritykseen liittyvän POST hakupyynnön. Sieltähän ne käyttäjätunnukset heti löytyivät:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/73761d96-3553-4fa5-b5c4-559a5791039f)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/bb1563b0-6b0f-4482-a9b4-85d8a694843d)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/9ea23a33-5dc2-43a4-81d3-98bca7ed1f9f)

Koska tunnukset liikkuivat selkeänä tekstinä, eikä kryptattuna, niin niitä oli helppo löytää.

### o) (A10) Server-Side Request Forgery (WebGoat 2023.4)

***Server-Side Request Forgery (SSRF)*** haavoittuvuutta hyödyntämällä päästä lukemaan ja/tai muokkaamaan sellaisia palvelimen resursseja, joihin hänellä ei pitäisi olla pääsyä. Hyökkääjä voi lähettää tai muokata URLia, jota sitten palvelimella pyörivä ohjelma lukee tai lähettää dataa vastauksena. Sekin on mahdollista, että hyökkääjä voi näin päästä lukemaan palvelimen konfiguraatioon liittyviä tiedostoja, päästä käsiksi palvelimella olevaan tietokantaan, lähettää POST hakupyyntöjä sisäisiin palveluihin, joiden ei pitäisi olla näkyvillä ulkopuolisille.

  - Server-Side Request Forgery (2)
    
![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/0771e743-56e1-4f21-896e-b3d80c4b023f)

Luulen, että "Steal the Cheese" napin painaminen kutsuu funktion, joka avaa URLin joka osoittaa/hakee Tomin kuvaa. Muokkasin URLia ```url=images%2Ftom.png``` -> ```url=images%2Fjerry.png```.

Seuraava osatehtävä alla. Uskon, että vaihtamalla kuvan alaosassa olevaa URLia ```url=images%2Fcat.png```sta ```url=http://ifconfig.pro```ksi tehtävä ratkeaa:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/81e9b8f4-65e1-4fff-ad7c-25e409f36fb6)

...ja niin se olikin :)  Sama periaate kuin Tomin ja Jerryn kuvan kanssa.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/ff275d7c-f592-4f77-acfe-400ac164f71e)


### p) Client side (WebGoat 2023.4)

  - Bypass front-end restrictions (2)

Eka osatehtävä - Field Restrictions. Tarkoituksena on palvelimelle lähetettävää POST hakupyyntöä siten, että se ohittaa kaiken neljän kysymyksen/kentän vaatimuksia. Painoin "Submit" ja avasin siihen liittyvää POST hakupyyntöä "manual request editorissa": 

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/5ae39318-de67-4e0c-bb98-e4197119fd36)

Seuraavaksi muokkaan parametrien *select*, *radio*, *checkbox*, *shortInput*, *readOnlyInput* arvot pelkäksi ```option```ksi. Eli lähetetään palvelimelle POST hakupyyntöä muokatuilla parametriarvoilla ja ohitetaan sillä millaisia vastauksia se käyttäjältä odottaa. Tällaista haavoittuvuutta voinee ehkäistä sillä, että käyttäjän syöte jollain lailla validoidaan ennen kuin hakupyyntö lähtee palvelimelle/ennen kuin palvelin vastaa siihen.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/a8875a78-bd07-46d7-86b8-f5d7939569d4)

Toisessa osatehtävässä pitäisi lähettää palvelimelle pyyntöä, jossa syöte yhdessäkään kentässä ei täsmää regexiin niiden yläpuolella. 

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/4559b819-8e8a-496e-90e4-63bf5926de20)

Muutin syötteet "manual request editorissa" tällaisiksi ```field1=abc1&field2=123p&field3=abc!+123?+ABC-&field4=seven123&field5=01101abc&field6=90210##-1111&field7=30a1-6b04-4c882&error=0```. Eli jokaisessa kirjoitin jotakin, mikä poikkesi määrityksistä.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/7246c1fe-642e-4f1d-913b-2e8b850f2e04)

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

https://www.youtube.com/watch?v=YO8rsCMVUyY&t=1s

https://portswigger.net/web-security/server-side-template-injection

https://docs.djangoproject.com/en/4.2/

https://portswigger.net/web-security/cross-site-scripting/cheat-sheet

https://portswigger.net/web-security/cross-site-scripting/stored

https://security.stackexchange.com/questions/119989/typical-search-box-xss-attack

https://www.youtube.com/watch?v=_MdvIWvh7rM

https://www.zaproxy.org/docs/burp-to-zap-feature-map/

https://www.youtube.com/watch?v=8fMFLqbd0-Y

https://github.com/WebGoat/WebGoat/wiki/Main-Exploits#insecure-direct-object-reference-lesson-5-exercise

https://thehackerish.com/idor-tutorial-hands-on-owasp-top-10-training/

https://www.youtube.com/watch?v=K5BBP88kBjU

https://github.com/WebGoat/WebGoat/issues/1424

https://www.youtube.com/watch?v=-n4OmhUN3vA

https://www.base64decode.org

https://cryptii.com/pipes/hex-decoder

https://string-functions.com/reverse.aspx

https://www.youtube.com/watch?v=hrEB9L2oyic
