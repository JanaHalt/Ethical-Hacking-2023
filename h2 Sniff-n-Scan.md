## Viikon 44 läksy - Sniff-n-Scan

Toinen viikko eettisen hakkeroinnin ihmeellisessä maailmassa. Tehtävänanto:

*Sniff-n-scan tutustuttaa uuteen lähteeseen, hakkeritapahtumien nauhoihin. Opit valvomaan hyökkäystyökalujen toimintaa snifferillä, ja tutustut Wiresharkiin. Wireshark osaa myös analysoida paketit automaattisesti. Weppiin murtautumista auttaa suomalainen, fuzzereiden huipulle noussut ffuf. Harjoitusmaalien asentamisesta kokeillaan paikallisia binäärejä (by yours truly) ja Dockeria.* Lähteenä <a href="https://terokarvinen.com/2023/eettinen-hakkerointi-2023/#h2-sniff-n-scan">Tero Karvinen - Ethical Hacking 2023</a>.

### Lue tai katso ja tiivistä
#### Hoikkala "joohoi" 2020: Still Fuzzing Faster (U fool). In HelSec Virtual meetup #1 <a href="https://www.youtube.com/watch?v=mbmsT3AhwWU">Katso video täältä - FFUF</a>

Joona puhui videolla luomastaan <a href="https://github.com/ffuf/ffuf">FFUF - fast web fuzzer</a> työkalusta.

- Web-fuzzing: laitetaan syötteeksi erilaista dataa kuin oletetusti pitäisi syöttää (numeroita, kokonaislukuja, ...) ja tekemällä se peräkkäisessä järjestyksessä ja testaamalla kaikki eri syötteet
- Ffuf:lla voidaan testata erilaisia asioita ja sen tulokset (matches) voidaan lähettää Burp suiteen (tietoturvatestausohjelmisto) <a href="https://portswigger.net/burp">Burp</a>:
  - HTTP REQUEST: Tavallisimmat kohteet: GET parametrit, otsikot (headers), POST data (form data, JSON, ...).
  - RESOURCE DISCOVERY:
  - BRUTEFORCE A PASSWORD
  - BRUTEFORCE A HTTP VERIFICATION
  - VIRTUAL HOST DISCOVERY
  - PARAMETER DISCOVERY
  - XXS HELPER
 
 #### Lyon 2009: Nmap Network Scanning: Chapter 15. Nmap Reference Guide

   ##### <a href="https://nmap.org/book/man-port-scanning-basics.html">Port Scanning Basics</a>

   *(opettele, mitä tarkoittavat: open, closed, filtered; muuten vain silmäily)*
  
   Porttien skannaaminen on Nmapin ydinfunktio. Nmapin peruskomento on ```nmap <target>```, ja se skannaa 1000 tavallisinta TCP porttia skannattavassa kohteessa. Nmapin mukaan portit voivat olla kuudessa eri tilassa (open, closed, filtered, unfiltered, open|filtered, closed|filtered). Yleisimmät ja tärkeimmät:
   
   *- open*: Portti on auki, ja siinä kuunteleva sovellus aktiivisesti hyväksyy TCP yhteydet, UDP datagrammit tai SCTP assosiaatiot (yhteys kahden SCTP päätelaitteen (endpoints; palvelimet) välillä.
   
   *- closed*: Portti on kiinni. Se on löydettävissä Nmapilla (Nmap probe packets), mutta tällaisella portilla mikään sovellus ei ole kuuntelemassa. Näiden porttien kohdalla Nmap kuitenkin pystyy toteamaan, onko kohdejärjestelmä (host) päällä (up) testauksessa käytetyllä IP:llä ja mikä on sen käyttöjärjestelmä.
   
   *- filtered*: Nmap ei pysty toteamaan, onko portti auki, sillä sen kohdejärjestelmän filtteri (dedicated firewall device, router rules, host-based firewall software) estää sen. Joskus tällaiset portit vastaavat ICMP virheviestillä esim. tyyppi 3 koodi 13 - kohde tavoittamaton: kommunikointi hallinnollisesti (administratively) kielletty).
   
   ##### <a href="https://nmap.org/book/man-port-scanning-techniques.html">Port Scanning Techniques</a>

   *(opettele, mitä ovat: -sS -sT -sU; muuten vain silmäily)*

   *- -sS (TCP SYN scan)*: "oletusskannaus", suosituin. Nopea suorittaa, skannaa tuhansia portteja sekunnissa. Palomuurit eivät yleensä haittaa sitä. Se on melko huomaamaton, sillä se ei suorita TCP-yhteyksiä (loppuun) ja siksi sitä kutsutaan myös "puoli avoimeksi". *Kts <a href="https://nmap.org/misc/split-handshake.pdf">The TCP Split Handshake</a>*.
   
   *- -sT (TCP connect scan)*: oletus TCP skannaus, silloin kun SYN skannaus ei ole sopiva vaihtoehto. Tämä siinä tapauksessa kun käyttäjällä ei ole "raw packet" oikeuksia. Silloin Nmap pyytää lähdekoneen käyttöjärjestelmää muodostamaan yhteyden kohdejärjestelmän koneeseen ja porttiin käyttämällä ```connect system call```. 
   
   *- -sU (UDP scans)*: TCP-protokollaa käyttävien palveluiden ohjella, myös UDP-palvelut ovat laajalti käytössä. Yleisimmät ovat DNS, SNMP ja DHCP (portit 53, 161/162 ja 67/68). UDP skannaus on hitaampaa, mutta sitä kannattaa silti huomioida ja hyödyntää osana tietoturvan kokonaissuunnittelua. Tämä skannaus lähettää UDP-paketin jokaiseen kohdistettuun porttiin. Jos vastauksena on ICMP portti tavoittamaton-virhe (tyyppi 3, koodi 3), portti todetaan suljetuksi, eli ```closed```. Joidenkin muiden ICMP virheiden kohdalla, portti voidaan todeta myös suodatetuksi, eli ```filtered```. Joskus palvelu vastaa UDP-paketilla, eli portti on ```open```. Mikäli vastausta ei saada uudelleenlähetyksistä huolimatta, portti todetaan olevan avoin|suodatettu ```open|filtered```. Parametrin ```-v```(versiontunnistus) avulla voidaan erottaa oikeasti avoimet portit suodatetuista.

### a) Fuff. Ratkaise <a href="https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/?fromSearch=ffuf#your-turn---challenge">Teron ffuf-haastebinääri</a>. Artikkelista <a href="https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/">Find Hidden Web Directories - Fuzz URLs with ffuf</a> voi olla apua.

Tavoitteena on löytää kaksi URLiä: admin sivu ja versionhallintaan liittyvä sivu. Aloitin kopioimalla **dirfuzt-1** linkin täältä <a href="https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/?fromSearch=ffuf#your-turn---challenge">Teron ffuf-haastebinääri dirfuzt-1</a>. Sitten menin virtuaalikoneeni komentoriville, latasin ko. binäärin ```wget``` komennolla ja aiemmin kopioidulla url:lla. Sitten muutin sen käyttöoikeudet siten, että sitä on mahdollista suorittaa ```chmod u+x dirfuzt-1``` (eli lisättiin sillä hetkellä kirjautuneelle käyttäjälle oikeus suorittaa ko. tiedosto).

![teronbinaari1a](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/6843f079-2cf4-49af-bdaa-3f55ec791a32)

Avasin näytölle tulleen linkin, jee:

![teronbinaari2a](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/9c34f8b0-3086-4cda-ba99-55ec2b4a8658)

Seuraavaksi Teron <a href="https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/">artikkelista</a> oppia ottaen, asensin **ffuf:n**. 

Tiedoston lataus:  ```wget https://github.com/ffuf/ffuf/releases/download/v2.0.0/ffuf_2.0.0_linux_amd64.tar.gz```

Purku: ```tar -xf ffuf_2.0.0_linux_amd64.tar.gz```

Sitten vielä: ```./ffuf```, tämä näyttää kaikki mahdolliset parametrit, joita voidaan ffufin kanssa käyttää.

 ![ffufinstal1a](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/e47aa8d8-a7b2-4719-8051-8f7f15304086)

 Jotta voisin käyttää **ffufia**, tarvitsin vielä listan/sanakirjan. Ja jotta sitä ei tarvinnut kirjoittaa käsin (mikä sekin on mahdollista!), niin latasin Daniel Miesslerin luoman <a href="https://github.com/danielmiessler/SecLists">SecLists</a> sanakirjan.

![ffufinstal2a](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/72efcfc1-9c7c-4742-a23f-2935bc8de876)

Tässä vaiheessa otin virtuaalikoneeni nettiyhteyden pois päältä - ennen kuin aloin **ffufilla** luomaan tuhansia ja tuhansia pyyntöjä palvelimelle. Näin myös Tero neuvoi aiemmin mainitsemassasi artikkelissa.

![nettipoikki1a](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/7b5a85a9-98cb-4823-b7c4-63b42ecb0ea8)


Ja sitten ei kun etsimään piilotettuja linkkejä! Ensin kokeilin ```./ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ -c```

```-w``` tarkoittaa "wordlist"

```-u``` taas tarkoittaa URLiä, josta halutaan etsiä

```-c```:n avulla saadaan tulokset värikoodatuiksi. Kuvassa alempana sen voi huomata vihreistä riveistä.

Tämä ei vielä tuottanut toivottua tulosta. Tai no luulenpa, että se mitä etsitään on tuolla jossain tuhansien vastausten joukossa. Sen etsimiseen menisi varmaan sata vuotta! 

![fuffB](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/3993d87f-8510-4ad4-871d-8a0dbf80cbf9)

Sitten kokeilin lisätä parametrin ```-fc 200``` (filter out vastauskoodia/http 200). Koodi 200 tarkoittaa, että pyyntö on suoritettu onnistuneesti, eli halusin saada esille ne palvelimen vastaukset, joiden kohdalla selaimeni palvelupyyntöä ei saatu suoritettua. Sain kaksi tulosta:

![fuffD](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/f28b0872-412a-4fd9-9f86-f92312629cdf)

Niistä ```.git``` liittyy versionhallintaan, joten kokeilin sitä. Laitoin **.git** selaimen osoiteriville ja siinä se oli. Eka kohde löydetty!

![ffufEKAb](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/ae69ad89-83fb-4e01-a026-e51708724cf5)

Nyt sitten etsimään toista kohdetta, admin sivua. Kokeeksi laitoin ```-fc``` parametrin arvoksi ***301***, eli juuri toisin kuin edeltävässä kohdassa. Siellä laitoin arvoksi ***200*** ja kaksi tulosta, jotka sain, olivat koodia ***301***. Päädyin tekemään näin, koska en ollut täysin varma miten lähtisin tätä toista kohdetta etsimään. Arvata saattaa, että tuloksia oli taas miljoonaa. Tässä vaiheessa mieleeni tuli, että mitä jos lähettäisin nämä tulokset tekstitiedostoon ja sieltä etsisin vaikkapa ```grep``` komennolla sanaa ***admin***. Eli pistin komentoriville komennon ```./ffuf -c -w common.txt -u http://127.0.0.2:8000/FUZZ -fc 301./ffuf -c -w common.txt -u http://127.0.0.2:8000/FUZZ -fc 301 > aarre.txt``` ja sitten ```grep "admin" aarre.txt```. 

Tuloksena oli edelleen pitkähkö lista (121 riviä). Ajattelin, että se on kuitenkin ihan kohtuullinen, joten skrollasin sitä alhaalta ylöspäin ja kokeilin pari kohtaa selaimen osoiteriville. Kunnes sitten osui SE OIKEA kohdalle! ```wp-admin```. 



![grepadminA](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/2f98c998-cf10-4eb6-b585-5f0e0ce536e1)

![grepadminB](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/2a8de96f-6d66-41d2-9199-d9f6b2e9fe7f)

![ffufTOKAb](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/a4d06156-0774-484c-9ed3-d92323c76700)

## Lähteet

https://www.youtube.com/watch?v=mbmsT3AhwWU 

https://github.com/ffuf/ffuf

https://nmap.org/book/man-port-scanning-basics.html

https://nmap.org/book/man-port-scanning-techniques.html

https://www.juniper.net/documentation/us/en/software/junos/gtp-sctp/topics/topic-map/security-gprs-sctp.html

https://nmap.org/misc/split-handshake.pdf

https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/?fromSearch=ffuf#your-turn---challenge

https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/

https://wheregoes.com/http-status-codes/


