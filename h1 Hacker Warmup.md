# h1 Hacker Warmup - viikon 43 läksyt

Tällä viikolla päästään alkuun matkalla hakkeroinnin maailmaan. Tässä linkki alkuperäiseen tehtävänantoon <a href="https://terokarvinen.com/2023/eettinen-hakkerointi-2023/#h1-hacker-warmup">Ethical Hacking 2023 - h1</a>.

Alkuun piti *tehdä tiivistelmä* kahdesta aiheesta. Molemmat lähteet olivat englanniksi, joten koin helpommaksi kirjoittaa myös tiivistelmät englanniksi:

**<a href="https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/">Active reconnaissance</a>**:
- in passive reconnaissance no packets/data is being sent directly towards target environment ***passive recon is invisible to logs***
- active reconnaissance follows after passive recon ***active recon "sets alarms" - visible to logs***
  - information/data being sent to the target environment
  - important to make clear picture of all details in target environment - to avoid being overwhelmed and to save time
    - port scanning (what ports/services found in passive recon are actually listening)
      - Nmap, Masscan, Udpprotoscanner
    - web service review (which web apps are available? which ones to focus on first?)
      - EyeWitness
    - vulnerability scanning
      - network vulnerability scanners: OpenVAS, Nessus, Nexpose, Qualys, Nmap
      - web vulnerability scanners: Nikto, WPScan, SQLMap, Burp Suite, Zed Attack Proxy

 **<a href="https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf">Intrusion Kill Chain</a>**:
 
U.S.military defines kill chain as following: find adversary targets suitable for engagement -> fix their location -> track and observe -> target with suitable weapon/asset to create desired effects -> engage adversary -> assess effects.
Applied in cybersecurity on intrusion, the kill chain (*intrusion kill chain*) goes as following:
1. Reconnaissance: research, identify and select the targets
2. Weaponization: combile a remote access trojan with an exploit to get a deliverable payload
3. Delivery: delivering the weapon to the target environment
4. Exploitation: trigger the code
5. Installation: to gain remote access or backdoor on the victim system
6. Command and control (C2): to set up a communication channel (C2), compromised hosts must send signals out to an Internet controller server
7. Actions on Objectives: all first six phases have to pass through so that intruder can proceed and achieve their goal

______________________________________________________________

Tehtävät teen kannettavallani:
- Acer Swift 3
- AMD Ryzen 7 4700U with Radeon Graphics, 2000 Mhz, 8 ydin
- RAM 16 GB
- Windows 11 Home

Varsinainen toteutus VirtualBoxissa virtuaalikoneella, johon on asennettu Fedora 38 desktop sekä toinen virtuaalikone, johon on asennettu Kali.

### a) Ratkaise <a href="https://overthewire.org/wargames/bandit/">Over The Wire: Bandit</a> kome ensimmäistä tasoa (0-2):

Sivulla <a href="https://overthewire.org/wargames/bandit/bandit0.html">Bandit Level 0</a> oli selkeä ohje miten menetellä. Avasin virtuaalikoneellani komentorivin ja kirjauduin peliin:
![1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/fcfb0ad8-dae8-46c2-af07-f865dc75f5f0)

Kirjautuminen onnistui, eli taso 0 suoritettu onnistuneesti:

![2](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/5f8a261a-121e-4240-8819-4cc457846ce6)

Alkutekstissä luki muutamia ohjeita miten alustalla toimitaan, esimerkiksi mistä löytyy tasot ja mistä salasanat yms. 

Tästä sitten jatkoin tasolle *Level 1*. Suoritettu onnistuneesti:

![3](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/ad69bb3a-9911-401c-b54f-93da855e3072)

Taso 2 suoritettu onnistuneesti:

![4](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/58afaa16-edab-450a-ae69-80bfd4ed33c3)

Tämän tason suorituksessa piti opetella miten avataan/luetaan tiedostoja, jotka alkavat "-":lla. Apu löytyi googlen avulla täältä <a href="https://stackoverflow.com/questions/42187323/how-to-open-a-dashed-filename-using-terminal">How to open a dashed filename using terminal</a>.

## b) Ratkaise <a href="https://challenge.fi/">Challenge.fi:sta</a> yksi tehtävä.

Piti ratkaista yksi tehtävä, mutta kun oli niin mukavaa ja pääsin vauhtiin, niin ratkaisin kaksi. Ensimmäistä mietin aika pitkään ja kun en millään meinannut päästä eteenpäin, turvauduin yhteen vinkkiin. Sitä kautta pääsin kärryille, millaisen salakirjoitussäännön (cipher) mukaan oli kirjaimia vaihdettu toisiin. Hankaluutta tuotti se, kun ensin yritin vaihtaa kirjaimia suomalaisten aakkosten mukaan. Se ei tuottanut järkevää tulosta, sillä lopputuloksena oli mitään sanomaton rivi kirjaimia. Vasta kun vaihdon kirjaimet pelkästään perusaakkosten mukaan (eli jätin å, ä, ö pois), sain oikean lopputuloksen.

Toinen tehtävä oli myös mielenkiintoinen. Avasin kuvan ja ajattelin, että tämähän on helppoa - siinä oli sen verran tutun näköinen paikka. Väärässä olin :D Yritin kirjoittaa paikan nimen monessa eri muodossa, mutta yksikään ei ollut oikein. Lopulta tutkin vielä kuvan ominaisuuksia (properties, kuten milloin otettu, gps, millä kameralla, jne) ja sitä kautta pääsin oikeaan lopputulokseen.

![16a](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/3f379bce-7c56-446a-85e6-3b059dadc306)

## c) Ratkaise PortSwigger Labs: <a href="https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data">Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data.</a>

Muistan, että SQL injektioita käsiteltiin Tietoturvan perusteet kurssilla viime keväänä. Muistin virkistämiseksi tutustuin kertausmateriaaliin <a href="https://portswigger.net/web-security/sql-injection">SQL injection</a>. 


![17a](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/0ae4e2e1-38da-4c14-af2c-c9513caf3d32)

Kertausmateriaalista oli apua. Lisäksi luin aiheesta myös <a href="https://www.hakatemia.fi/courses/sql-injektio/mita-ovat-sql-injektiot">Hakatemia - Mitä ovat SQL-injektiot?</a>. Eli käytännössä SQL-injektio tarkoittaa, että hyökkääjä pääsee muokkaamaan SQL-kyselyn rakennetta ja pääsee siten näkemään ja/tai muokkaamaan sellaisiakin osia tietokannasta, joihin hänellä tavallisesti ei olisi pääsyä. Laittamalla ehdon "OR 1=1" (eli aina totta), saisimme näkyviin kaikki kategorian tuotteet - ja sehän oli haasteen tavoite. Ongelma tuli siinä, että ei tuota ehtoa tietenkään voinut laittaa sellaisenaan internetselaimen osoiteriville. Aikani tätä pohtiessani ja googlea ahkerasti käytettyäni, laitoin sen kommentin sisälle ```--``` (kommentin indikaattori SQL:ssä) ja sain näkyviin julkaisemattomatkin tuotteet: *Lab Solved*.

## d) Asenna Linux virtuaalikoneeseen. Kali (viimeisin) tai Debian 12-Bookworm.:

Päädyin Kaliin. Sen sai näppärästi valmiiksi säädettynä virtuaalikonepakettina <a href="https://www.kali.org/get-kali/#kali-virtual-machines">Get Kali - Pre-built virtual machines</a>. 

![18kaliA](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/3012bf92-b410-47e5-96e1-57aafcea17f2)

_________________________________

### e) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi (localhost). Analysoi tulokset.

### f) Porttiskannaa kaikki koneesi (localhost) tcp-portit. Analysoi tulokset.

### g) Tee laaja porttiskanaus (nmap -A) omalle koneellesi (localhost), kaikki portit. Selitä, mitä -A tekee. Analysoi tulokset.

Edit. 27.10. klo 18:25// Tässä alla tehty kohdat e, f ja g! Otsikot vain tuolla ylempänä niputettu yhteen :)

En tiennyt mitä parametreja laittaisin nmapille, jotta se skannaisi 1000 tavallisinta tcp-porttia, joten kurkkasin ```man nmap``` ja <a href="https://www.redhat.com/sysadmin/nmap-info">Nmap info</a>, joista selvisi, että etsimäni parametri on ```--top-ports <number>```. Lisäksi, jotta skannaisin tcp-portteja, niin parametriksi pitää laittaa myös ```-sT```.

Kali-virtuaalikoneella sain kaikilla skannaustavoilla käytännössä samat tulokset:


![kali2nmap](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/90e89954-405f-4a42-9cba-d9d1100527b2)


![kali3nmap](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/e5be7ca6-b266-4a2f-b95f-ecb9fdc6b71d)


Tuloksissa näkyy, että host (eli oma kone/kali) on päällä. Kaikki 1000 tavallisinta tcp porttia on kiinni ja *ignored state(conn-refused)*. Tämä tarkoittaa, että vaikka nmap pystyi toteuttamaan TCP handshaken skannatun kohteen kanssa (tässä tapauksessa localhost), niin kohde aktiivisesti kieltäytyi muodostamasta yhteyttä. Sen taustalla voi olla joko se, että yhdelläkään portilla ei olisi mikään palvelu "kuuntelemassa" tai sitten järjestelmän palomuuri estää yhteyden porttiin.
Laaja skannaus ```nmap -A``` tarkoittaa ```-A: Enable OS detection, version detection, script scanning, and traceroute```. Sen osalta skannaustuloksessa todetaan, että OS (käyttöjärjestelmä) ja palveluiden tunnistus suoritettiin, mutta ei voitu tunnistaa OS tarkasti.

Tein samat skannaukset myös Fedora-virtuaalikoneellani ja tulokset olivat samansuuntaiset. Sillä erolla Kaliin, että Fedoralla skannaus (*nmap -sT --top-ports 1000 localhost* ja *nmap -sT localhost*) palautti myös tiedon, että yksi portti on auki ```portti 631```, palvelu ```ipp - internet printing protocol``` kuuntelemassa. 

![fedoraAnmap](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/caf828cc-5ad9-48a7-9bc2-0bd33ca9eb0e)


Laaja skannaus (*nmap -A localhost*) palautti aiemmin mainittujen lisäksi sen, että portilla 631 aktiivisena olleen palvelun (ipp) versio on ```CUP 2.4```. <a href="https://www.scaler.com/topics/linux-cups/">CUPS</a> lyhenne tulee sanoista *Common Unix Printing System*. Se on open-source järjestelmä, joka mahdollistaa tulostamisen Linux ja Unix ympäristöissä. Kohta ```http-robots.txt: 1 disallowed entry``` viittaa robots.txt-rajausstandardiin. Ko. standardi kertoo hakukoneille (esim. google tai bing), mitkä kansiot/URLit sivustolla ne voivat käyttää.

![fedoraBnmap](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/e8639830-4169-42c5-8cd2-0f71aa7f8ae8)

Sitten tein vielä kaikki kolme skannausta myös omalla kannettavallani (Windows 11 Home). Asensin sitä varten erillisen sovelluksen <a href="https://nmap.org/download">Nmap - download</a>. 
```nmap -sT --top-ports 1000 localhost``` tuloksena sain tiedon, että kaksi tcp porttia on auki:
- 135, palvelu *msrpc*
- 445, palvelu *microsoft-ds*

**msrpc** - protokolla. Käyttää client-server mallia, joka mahdollistaa ohjelman pyytää palvelua toisella tietokoneella olevalta ohjelmalta, tarvitsematta tietoja toisen koneen verkosta.

**microsoft-ds** - "nimi", joka on annettu SMB:n (*Server Message Block*) käyttämälle portille 445. SMB on verkkoprotokolla, jota käytetään yleensä Windows-verkoissa resurssien (esim. tiedostot ja tulostimet) jakamiseen verkon kautta. Sen kautta on mahdollistaa suorittaa komentoja etänä. Windows 7:lla / Windows Server 2008:lla hyökkääjät saattoivat hyödyntää tätä väylää käyttääkseen ransonware WannaCrypt:ä (mato; tunnettu myös nimillä WannaCry tai WCRY). Haavoittuvuus korjattiin tietoturvatiedotteessa *MS17-010*, joka julkaistiin vuonna 2017. Tämä ei enää vaikuta Windows 10 ja sitä tuorempiin tietokoneisiin.

![windowsAnmap](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/59b0e897-37ef-4d61-9042-b82b181e1229)

Seuraavaksi kokeilin ```nmap -sT localhost```, mutta odotettuani yli 5 minuuttia, jolloin vieläkään ei ollut tapahtunut mitään, keskeytin skannauksen ja siirryin seuraavaan.

Sitten oli vuorossa ```nmap -A localhost``` laaja skannaus. Se tuotti seuraavanlaisen tuloksen:

![windowsBnmap](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/62c17835-44e8-40b7-b834-7cc42c32faab)

Eli samansuuntainen tulos kuin edellinen. Lisäksi saatiin tietää *msrpc*-palvelun versio, käyttöjärjestelmän versio sekä kohdejärjestelmässä (tässä siis oma kannettavani) suoritettavien skriptien tulokset. Tuloksissa oli virheellisesti tunnistettu kannettavani käyttöjärjestelmä - minulla ei ole Windows 10, vaan Windows 11 Home.

## h) Asenna ja käynnistä jokin palvelin (apache, ssh...) koneellesi. Vertaile, miten porttiskannauksen tulos eroaa.

Kalissa on apache palvelin jo valmiiksi asennettuna, mutta *disabled* tilassa. Joten käynnistin sen komennolla ```sudo systemctl start apache2.service``` ja tarkistin tilan komennolla ```systemctl status apache2``` - tila oli *active*. Sitten tein kaikki nmap-skannaukset kuten aikaisemmin. Tulokset:
- ```sudo nmap -sT --top-ports 1000 localhost```: portti 80 auki, palvelu http
- ```sudo nmap -sT localhost```: kuten edellinen
- ```sudo nmap -A localhost```: kuten edellinen. *Plus* http palvelun versio Apache httpd 2.4.57 (Debian), kernel versio linux 2.6.32

![kaliapache3](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/9df91cdf-2d44-4cfd-a34e-3471481fc3d2)

## i) Kokeile ja esittele jokin avointen lähteiden tiedusteluun sopiva weppisivu tai työkalu.

Valitsin verkkosivuston <a href="https://www.shodan.io">Shodan</a>. Tällä sivustolla voi hakea ja/tai selailla verkkoon kytkettyjä laitteita, kuten kamerajärjestelmiä tai IoT-laitteita (esim. lämpösensorit?). Hakua voi muokata esimerkiksi avoimien porttien tai protokollien mukaan. Sivustoa voi siis hyödyntää osana verkon haavoittuvuuksien kartoittamista. Googlasin hakusanaa *shodan queries*, sillä halusin vinkkejä mitä kaikkea yleensä ko. sivun kautta haetaan. Hakutuloksissa tuli vastaan <a href="https://github.com/jakejarvis/awesome-shodan-queries">Jake Jarvis - Awesome Shodan Search Queries</a> sivu. Webkamerat-kategoria vaikutti mielenkiintoiselta, joten kurkkasin sinne. Otin heti ensimmäisen haun listalta ```"Server: yawcam" "Mime-Type: text/html"``` ja laittoin sen hakukenttään shodan.io-sivulla. Tuloksena sain listan webkameroita, jotka ovat avoinna suoraan nettiin. 

![shodankamera](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/2a6e50f4-4686-431b-9f3e-fc6e8433bd59)

#### Lähteet:

https://terokarvinen.com/2023/eettinen-hakkerointi-2023/

NMAP man-sivut

https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/ 

https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf 

https://stackoverflow.com/questions/42187323/how-to-open-a-dashed-filename-using-terminal

https://en.wikipedia.org/wiki/ROT13 

https://www.gps-coordinates.net/gps-coordinates-converter/

https://portswigger.net/web-security/sql-injection#retrieving-hidden-data

https://en.wikipedia.org/wiki/SQL_injection

https://www.invicti.com/blog/web-security/sql-injection-cheat-sheet/

https://www.redhat.com/sysadmin/nmap-info

https://en.wikipedia.org/wiki/Internet_Printing_Protocol

https://www.scaler.com/topics/linux-cups/

http://www.robotstxt.org/robotstxt.html

https://developers.google.com/search/docs/crawling-indexing/robots/intro

https://nmap.org/book/inst-windows.html

https://www.extrahop.com/resources/protocols/msrpc/

https://security.stackexchange.com/questions/229820/microsoft-ds-vulnerability

https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/smb-sharing-not-accessible-when-tcp-port-445-listen#cause

https://www.microsoft.com/en-us/wdsi/threats/malware-encyclopedia-description?Name=Ransom%3AWin32%2FWannaCrypt

https://subscription.packtpub.com/book/security/9781785888427/1/ch01lvl1sec14/network-services-in-kali-linux
