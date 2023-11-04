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

   *- -sS (TCP SYN scan)*: "oletusskannaus", suosituin. Nopea suorittaa, skannaa tuhansia portteja sekunnissa. Se on melko huomaamaton, sillä se ei suorita TCP-yhteyksiä (loppuun) ja siksi sitä kutsutaan myös "puoli avoimeksi". *Kts <a href="https://nmap.org/misc/split-handshake.pdf">The TCP Split Handshake</a>*.
   
   *- -sT (TCP connect scan)*: oletus TCP skannaus, silloin kun SYN skannaus ei ole sopiva vaihtoehto. Tätä tekniikkaa käyttäen Nmap pyytää lähdekoneen käyttöjärjestelmää muodostamaan yhteyden kohdejärjestelmän koneeseen ja porttiin käyttämällä ```connect system call```. **-sT** on hitaampi kuin **-sS** ja siitä todennäköisimmin jää myös merkintä skannattavan järjestelmän lokiin.
   
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

### b) Fuffme. Asenna <a href="https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/">Ffufme harjoitusmaali</a> paikallisesti omalle koneellesi. Ratkaise tehtävät (kaikki, paitsi ei "Content Discovery - Pipes")

Otsikossa linkitetyssä artikkelissa asennetaan **Fuffme** harjoitusmaali Debianille. Koska Kali on Debian pohjainen distro, lähden kokeilemaan ohjetta sellaisenaan.

Eli päivitys käyttöjärjestelmän päivitys ja edellytysten/riippuvuuksien asennus:

```sudo apt-get update``` Tämä sujui ongelmitta.

```sudo apt-get install docker.io git ffuf``` Tässä tuli vastaan pieni ongelma. Sen ratkaisu oli helppo, se vain kesti hetken... :)

![targetinstall1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/d88ce3e8-c9ba-46e3-82b8-e4373cabd33d)

Sen jälkeen kuitenkin pystyin jatkamaan, eli ei kun ```sudo apt-get install docker.io git ffuf``` uudestaan. Tällä kertaa se sujui ongelmitta:

![targetinstall2](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/5d6efab1-73d8-4ce6-a24d-42ec49fb9151)

Harjoitusmaalin Docker kontainerin pystyyn ja käyntiin:

```git clone https://github.com/adamtlangley/ffufme```

```cd ffufme/```

```sudo docker build -t ffufme```

```sudo docker run -d -p 80:80 ffufme```

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/130e1ec5-ff67-4a7f-8f88-a55acb70df76)

![targetinstall3a](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/3c1d4d7e-3121-476c-8b77-18cc093acce2)

Ja sama vielä selaimessa testattuna:

![targetinstalled1a](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/e9d33095-8ea9-49f7-9f30-5b3215fd585c)

Ennen varsinaisten haasteiden suorittamista piti toki asentaa sanakirjat/wordlists. Joten:

```cd ~``` - jotta olen varmasti kotikansiossa

```mkdir wordlists``` kansion luominen

```wget http://ffuf.me/wordlist/common.txt``` sanakirjan lataaminen. Kaksi muuta latasin samalla komennolla, paitsi ***common.txt*** tilalla oli ***parameters.txt*** ja ***subdomains.txt***.

![wordlists2](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/d7e4f8f2-fc57-4d76-b4a1-0fbac4dd9edd)

Seuraavaksi haasteet:
#### Basic Content Discovery
  
***FFUF.me*** sivulta ohjeena komento ```ffuf -w ~/wordlists/common.txt -u https://localhost/cd/basic/FUZZ``` tuotti juuri toivotun tuloksen:

![basicfuzz](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/be31d729-1b52-4d53-beee-adbccfd882d7)

#### Content Discovery - Recursion
  
Tässä käytetään parametria ***-recursion***, joka kertoo ffufille, että jos se löytää kansion, niin sen tulee jatkaa skannaamista sen kansion sisällä jne jne, kunnes se ei löydä mitään tuloksia. Tällä kertaa ohjekomento oli ```ffuf -w ~/wordlists/common.txt -recursion -u http://localhost/cd/recursion/FUZZ``` ja jälleen kerran saatiin toivottu lopputulos. Kuten kuvasta näkyy, ensin ffuf löysi kansion **/admin**, sitten kansion **/users** ja lopulta kansion **96**.

![recursion1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/e3956632-3783-4412-8273-6a73cd79c1e5)

#### Content Discovery - File Extensions

Tässä käytetään parametria ***-e***, jonka avulla voidaan määrittää millaisen tiedostopäätteen ffuf lisää jokaisen sanakirjasta löytyvän sanan loppuun ja näin pyritään löytämään oikea lokitiedosto (log-file). Nyt komentona oli ```ffuf -w ~/wordlists/common.txt -e .log -u http://localhost/cd/ext/logs/FUZZ``` ja lopputulos odotusten mukainen.

![fileextensions1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/4a595108-b1ca-4888-9f08-dc61a0555d8b)

#### Content Discovery - No 404 Status

Nyt harjoitellaan ***-fs*** parametrin käyttöä, jonka avulla voidaan suodattaa pois kaikki tulokset, joilla on tietty, määrittämämme, koko. Tässä tapauksessa halutaan suodattaa pois tulokset, joiden koko on 669 bittiä. Kuten alempana ensimmäisestä kuvasta näkyy, niin "Page Cannot Be Found" sivu on aina kooltaa 669 bittiä. Komentoina käytetään ensin ```ffuf -w ~/wordlists/common.txt -u http://localhost/cd/no404/FUZZ``` ja sitten ```ffuf -w ~/wordlists/common.txt -u http://localhost/cd/no404/FUZZ -fs 669```. Jälkimmäinen komento tuottaa ainoastaan yhden tuloksen, tiedoston ***secret***.

![no404a](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/abaafc5c-6982-4f50-b71a-0ac5edef6aac)

![no404b](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/28107762-b4b2-490a-b8e7-9d9e71bb90d2)

#### Content Discovery - Param Mining

Nyt etsitään puuttuvaa parametria! Ohjeen mukaisesti avasin sivun ```/cd/param/data``` (kuva alempana), jossa näin viestin *Required Parameter Missing* ja sivun HTTP status koodi oli 400 - Bad Request. Tämän komennon ```ffuf -w ~/wordlists/parameters.txt -u http://localhost/cd/param/data?FUZZ=1``` avulla lähdettiin etsimään puuttuvaa parametria. Alempana näkyy myös tulos, joka se mitä etsinkin :)

![missingpar1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/ac9a4e87-cfd0-4fe4-9ae7-1af6f125c56e)

![missingpar2](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/3779ab81-4075-43a4-97fe-d5b32c74e13d)

#### Content Discovery - Rate Limited

Seuraavana käytetään parametria ***-mc***, jonka avulla rajataan, mitkä http statukset skannaaminen tuottaa. Parametri ***-p*** tekee sen, että ffuf pitää taukoa 0,1 sekuntia (tässä tapauksessa) pyyntöjen välissä ja parametri ***-t*** luo ffufista 5 versiota - siten luodaan max 50 pyyntöä sekunnissa.

Ekana piti laittaa komentoa ```ffuf -w ~/wordlists/common.txt -u http://ffuf.test/cd/rate/FUZZ -mc 200,429```, josta tuloksena olisi pitänyt olla paljon 429 virheitä. En saanut tulosta, joka olisi ollut edes sinne päinkään. Ajattelin, että no jokin ihme juttuhan tässä on, kokeillaan sitä seuraavaa komentoa, joka ohjeessa oli, eli ```ffuf -w ~/wordlists/common.txt -t -p 0.1 -u http://ffuf.test/cd/rate/FUZZ -mc 200,429```. En ole tälläkään kertaa saanut toivottua tulosta. Nyt aloin jo miettimään, että mikä tässä mättää. 

![rate1a](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/9512b35d-99ce-4b4f-843a-802f39d83801)

![rate2a](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/86c53354-2881-42eb-8e74-5430a2417677)


Testasin yhteyttä ***ffuf.test***iin:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/6dca85a2-e7d0-4d34-aad6-cb169246e36a)

Ei pelita, ei. Sitten vaihdoin ***ffuf.test*** siihen, mitä oli edellisissä tehtävissä: ***localhost***. Ja hurraa hurraa, nyt alkoi Lyyti kirjoittaa!

```ffuf -w ~/wordlists/common.txt -u http://localhost/cd/rate/FUZZ -mc 200,429```

![rate3a1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/cf469a36-1b6e-4ee6-8f3f-313f5c877c76)

```ffuf -w ~/wordlists/common.txt -t -p 0.1 -u http://localhost/cd/rate/FUZZ -mc 200.429```

![rateoracle1a](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/b1a591d9-1210-42c8-9e0f-0c544bb1f8c7)

#### Subdomains - Virtual Host Enumeration

Tässä harjoituksessa etsitään alasivuja / subdomains käyttämällä virtuaalisia host-koneita ja muttamalla niiden host-otsikkoja.

Ensin käytetään komentoa ```ffuf -w ~/wordlists/subdomains.txt -H "Host: FUZZ.ffuf.me" -u http://127.0.0.1``` . Tulokset olivat samat kuin harjoituksen tehtävänannossa: eli jokainen tulos oli kooltaan 1495 bittiä.

![subdomains1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/fda4aa30-ff6e-458a-a9cf-26a480f58e92)

Seuraavaksi piti käyttää seuraavanlaista komentoa: ```ffuf -w ~/wordlists/subdomains.txt -H "Host: FUZZ.ffuf.me" -u https://127.0.0.1 -fs 1495```

***-fs 1495*** rajaa pois kaikki tulokset, jotka ovat kooltaan 1495 bittiä. Tulos oli jälleen toivotunlainen - löydettiin subdomain ***redhat***.

![subdomains2](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/80661935-1b79-426d-89cf-4e18d5a3d316)

### Porttiskannaa paikallinen kone (127.0.0.2 tms), sieppaa liikenne snifferillä, analysoi.

Virtuaalikoneellani, jossa on Kali Linux ovat oletuksena kaikki portit kiinni. Tätä harjoitusta varten käynnistin Apache palvelimen, jotta edes yksi portti (80) olisi auki. Ennen kun aloin tehtäviä, tarkistin, että palvelin on käynnissä:

![apachekäynnissä](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/cf8e1141-1be9-4a5f-a572-9955e1eb7936)

#### c) nmap TCP connect scan -sT

**-sT** on "TCP connect scan", eli se muodostaa yhteyden skannattavan kohteen kanssa. Kuvassa alla näkyy oikealla alhaalla, Wiresharkin ikkunassa, "Conversation completeness: Complete, NO_DATA". Tämä tarkoittaa, että yhteys muodostettiin (TCP handshake), mutta lähetetyissä ja vastaanotetuissa paketeissa ei ollut mitään dataa. 

```SYN``` Wiresharkin ikkunassa ylhäällä tarkoittaa synkronointisekvenssin (synchronize sequence) numeroa. Vain ensimmäisillä paketeilla, jotka lähetettiin kultakin yhteyden puolelta, pitäisi olla tämä määritettynä.

```ACK```samassa kohdassa osoittaa, että paketti on vastaanotettu/hyväksytty. Paketeilla, jotka lähetetään SYN paketin jälkeen, tulisi olla tämä numero/tunnus määritettynä.

```RST```, joka näkyy Wiresharkin ikkunassa punaisella korostettuna, tarkoittaa "reset the connection".

Virtuaalikoneen komentokehotteessa puolestaan näkyy, että kohdejärjestelmä on "päällä" ja sen portti 80 on auki ja siinä on http-palvelu kuuntelemassa.

![nmapst1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/1e23c664-aa4b-422c-9b06-20a1de523e71)

#### d) nmap TCP SYN "used to be stealth" scan, -sS (tätä käytetään skannatessa useimmin)

Tämä skannaustekniikka on nopeampi kuin **-sT**. Se ei muodosta yhteyttä skannattavaan kohteeseen "loppuun asti", vaan se *katkaisee* yhteyden. Kts <a href="https://nmap.org/misc/split-handshake.pdf">TCP - Split handshake</a>. Tämä ilmenee myös Wiresharkin kaapatuista paketeista. Ekana lähti TCP paketti kohti kohteen porttia 80 (SYN, "heei, haluan jutella"). Kohde vastasi TCP paketilla (SYN, ACK; "minäkin haluan jutella, sopii"). Sitten ensimmäinen osapuoli katkaisi yhteyden (RST).

Virtuaalikoneen komentokehotteessa näkyy samat asiat kuin edeltävässä kohdassa - kohde "päällä" ja portti 80 auki, http-palvelu kuuntelemassa.

![nmapss1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/d7e1f868-f560-4b99-a455-ac0ff07a541d)

#### e) nmap ping sweep -sn

Tämä skannaustekniikka "skannaa" *ping*in avulla, portteja ei skannata. Virtuaalikoneen komentokehotteessa jälleen näkyy, että kohde on "päällä". Wireshark ei jostain syystä kaapannut mitään paketteja. En ole varma miksi näin. Olisiko minun kenties pitänyt muuttaa jotain asetuksia Wiresharkissa.

![nmapsn1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/52ecbc03-cc99-4c0a-9234-61fe5ec1b99c)

Kun virtuaalikoneen skannaus ei tuottanut sellaista tulosta kuin olisin toivonut, ajattelin kokeilla vastaavaa oman kannettavani Windowsissa ja skannasin omassa verkossani sijaitsevaa toista Windows kannettavaa. Tulokset alla. ```192.168.100.14```on kannettava, jota skannasin. Nmapin skannaus totesi sen olevan "päällä. Wireshark (tässä kuvassa vasemmalla) kysyy rivillä *85*, ARP kyselyllä "missä on *192.168.100.14? Kertokaa 192.168.100.15:lle". Rivillä *90* näkyy vastaus, jossa on *192.168.100.14*n MAC-osoite. Tämän kahden rivin välissä ja niiden jälkeen näkyy mm. TCP ja DNS kyselyjä/paketteja, jossa kannettavani keskustelee esim. osoitteiden *13.33.243.37* (kts kuva alla) ja *192.168.100.1* (verkkoni reititin) kanssa. 

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/4dbd5fe7-0b76-4573-b253-633398ae685f)


![nmapsn2](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/d7bd1fe4-b236-48f8-bbf5-553ed4060d05)


#### f) nmap don't ping -Pn

Tässä skannaustekniikassa nmap kohtelee kaikkia kohteita (hosts) kuin ne olisi päällä, eikä suorita *etsintää* (host discovery). Kuvassa näkyy, että komentokehotteessa skannasin oman virtuaalikoneeni portit 80, 23, 443 ja 8080. Niistä vain portti 80 oli auki, http-palvelu kuuntelemassa. 

![nmapPn1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/21848cb9-99fd-4780-a51c-209eb4187e0b)


#### g) nmap version detection -sV (esimerkki yhdestä palvelusta yhdessä portissa riittää)

Tämä skannaustekniikka kertoo auki olevien porttien ja niissä kuuntelevien palveluiden lisäksi myös ko. palveluiden version. Eli tässä tapauksessa portti 80 auki, siinä http-palvelu / Apache2 kuuntelemassa. Wiresharkin ikkunassa puolestaan näkyvät jo aiemmissa kohdissa mainitut ```SYN```, ```ACK``` ja ```RST```. Ne kuvaavat sen, että virtuaalikoneeni muodostaa yhteyden paikalliseen palvelimeeni portin 80 kautta. Protokollista ovat kuvakaappauksessa edustettuina TCP ja HTTP (ja siitä GET pyyntö). 

![nmapsV1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/7cebc900-86cd-4cf4-9f61-5ef79c131805)

#### h) nmap output files -oA foo. Miltä tiedostot näyttävät? Mihin kukin tiedostotyyppi sopii?

Kuvassa näkyy, että nmap output-tiedostot ovat (alemmassa kuvassa *results.gnmap/.nmap/.xml*):

![nmapoA1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/7f6c568f-6b43-4043-b9ef-a8d31f1232f3)

```.gnmap``` - tämän kanssa voi käyttää **grep** komentoa, selkeä teksti.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/6d2a9797-27bc-45b6-b729-f32f6be7007b)

```.nmap``` - tämä on "normaali", selkeää tekstiä tämäkin.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/c1ee6e0c-9ecb-43fb-b774-d5c106d9e95b)

```.xml``` - formaattina XLS stylesheets. Toimii softan kanssa, voidaan käyttää tulosten muokkaamiseen html-muotoon. 

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/edf4bbef-05d8-489f-bee9-e5047f09acba)


#### i) nmap ajonaikaiset toiminnot (man nmap: runtime interaction): verbosity v/v, help ?, packet tracing p/P, status s (ja moni muu nappi)
#### j) Ninjojen tapaan. Piiloutuuko nmap-skannaus hyvin palvelimelta? 


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

https://www.loggly.com/ultimate-guide/apache-logging-basics/

https://en.wikipedia.org/wiki/Transmission_Control_Protocol 

https://nmap.org/book/man-runtime-interaction.html
