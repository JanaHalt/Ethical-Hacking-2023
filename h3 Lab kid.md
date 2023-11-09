## <a href="https://terokarvinen.com/2023/eettinen-hakkerointi-2023/#h3-lab-kid">h3 Lab kid</a>

Kolmas viikko käyntiin hyökkäyslabran rakentelun merkeissä. Lisäksi edetään kybertappoketjussa tiedustelusta uusin vaiheisiin: aseistus, toimitus, exploit, asennus ja kontrollikanava. 

### Lue/katso ja tiivistä:

#### <a href="https://learning.oreilly.com/library/view/mastering-kali-linux/9781801819770/Text/Chapter_10.xhtml#_idParaDest-257">Velu 2022: Mastering Kali Linux for Advanced Penetration Testing 4ed: Chapter 10 - Exploitation</a>

Kyseinen kirja ei löydy HH-Finnasta:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/dd6fa373-066e-47aa-8a8e-ba7428d50d8c)

Lisäksi, koska aiemmin kurssilla käytettiin tiivistämistehtävän lähteenä O-reilly:n aineistoa ja sen yhteydessä rekisteröin O-reilly:in, niin saan tämän ilahduttavan kuvan ruudulleni:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/8a8c7124-ee9c-4e8c-9f89-1586d1a9a773)

   *- The Metasploit Framework*

   *- The exploitation of targets using Metasploit*

   *- Using public exploits*

#### Vapaavalintainen läpikävely <a href="https://0xdf.gitlab.io/">0xdf</a> tai <a href="https://www.youtube.com/channel/UCa6eh7gCkpPo5XXUDfygQQA/videos">ippsec</a>.

Valitsin <a href="https://www.youtube.com/watch?v=XB8CbhfOczU&list=PLidcsTyj9JXJfpkDrttTdk1MNT6CDwVZF&index=40">HackTheBox - Help</a>, <a href="https://www.youtube.com/@ippsec">IppSec - YouTube kanava</a>lta.

- tässä "boxissa" murtauduttiin helpdeskz portaaliin

- käytettiin nmapia avoimien porttien löytämiseen

- <a href="https://www.kali.org/tools/gobuster/">gobuster</a> työkalun (*scans websites for directories*) avulla löydettiin **/support** sivu, jota kokeiltiin selaimessa lisäämällä **/support** sivun ip-osoitteen perään -> johti oikeannäköiseen *helpdesk/support* sivuun

- *searchsploiti*in avulla löydettiin 2 explotia: *a file upload* ja *SQL injection*. Komennon muoto ```searchsploit helpdeskz```. Hyödynnettiin ensimmäistä mainittua lataamalla sivulle luodun tiketin mukana myös muokattu PHP skripti

- selvitettiin Helpdeskz githubin sivulta, mihin ladatatut tiedostot tallennetaan

- selvisi, että sivusto käyttää graphql. Tämä tieto hyödynnettiin sellaisen käyttäjätunnuksen ja salasanan etsimisessä, jolla sitten pystyttiin kirjautumaan helpdeskz sivulle.

- Helpdeskz sivulle kirjautumisen jälkeen sieltä löytyi myös boolean pohjainen sql injektio (haavoittuvuus)

- kopioitiin query, josta aiemmin huomattiin tuo sql injektio, tiedostoon ja tarkistettiin se **SQLMap** työkalulla (*open source pentest tool that automates the process of detecting and exploiting SQL injection flaws and taking over of database servers*)

- lopussa vielä tehtiin python skripti tämän haavoittuvuuden hyödyntämiseen (exploit)

#### Nyrkkeilysäkki ei kuulu. Etsi hakukoneesta kotitehtäväraportti, jossa Kali ja Metasploitable on asennettu samaan verkkoon VirtualBoxiin ja testaamalla on osoitettu, että ne ovat irti internetistä. 

Löysin Sami Kulonpään aiheesta kertovan <a href="https://kulonpaa.com/?p=87">raportin</a>. 

- metasploitable asennettiin virtualboxiin ja sen verkkokortti määriteltiin *host-only adapter* tilaan

- kali-virtuaalikoneen verkkoasetuksissa otettiin "nettipiuha" irti ja kone asetettiin samaan verkkoon (host-only) metasploitablen kanssa

- lopuksi käynnistettiin metasploitable kone ja komentorivillä tarkistettiin ettei kone näy internettiin (komennolla **ping 8.8.8.8** -> **network unreachable**)

### a) Asenna Kali virtuaalikoneeseen

Minulla oli Kali jo valmiiksi asennettuna virtuaalikoneeseen, joten käytän sitä tässä.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/aee78056-8829-4b70-adda-20611d74c948)

### b) Asenna Metasploitable 2 virtuaalikoneeseen

Aloitin lataamalla Metasploitable 2:n ja koska se oli zippinä, niin purin sen auki:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/418c3763-5fbb-4cdf-b49e-f1c22866c8f7)

Ja jatkoin asennukseen ja sitten kirjautumiseen, ei ongelmia:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/a7685db5-704d-45e1-84cb-38d18160b2b3)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/69d747dd-ff2c-4ee0-aff3-ac1be1ef7802)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/94730e72-d4e8-44c6-a10e-9fc60119bd3e)

### c) Tee koneille virtuaaliverkko, jossa:
  *- Kali saa yhteyden Internettiin, mutta sen voi laittaa pois päältä*

  ![kalicableoff](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/03dc58d2-6350-4ceb-b107-98c01afc801c)

  *- Kalin ja Metasploitablen välillä on host-only network, niin että porttiskannatessa ym. koneet on eristetty internetistä, mutta ne saavat yhteyden toisiinsa*

Tässä näkyy, että molemmat koneet ovat host-only verkossa:

  ![kalihostonly](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/9c165e44-2e08-4bb7-8aa1-7515a248c43f)

  ![metasploitablehostonly](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/8863683d-e14c-403c-bfd5-f9764b5ca0f2)

  Tarkistin vielä, että verkkokortilla, johon molemmat yhdistin, on DHCP Server enabloitu:

  ![hostonlydhcpok](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/6e46ba1b-cb27-4e62-abfb-6b5046164cb3)

Tarkistetaan, mitkä ovat kalin ja metasploiten IP-osoitteet ja että ne varmasti saavat yhteyden toisiinsa:

Kali 192.168.12.4/24 ja Metasploit 192.168.12.3

![iposoitteet1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/ccf28fd0-d334-405b-8fe1-caa97cf33d29)

![kalimetasploitOK](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/e96131f7-5b5b-4867-b8f0-13e621302061)


  *- Osoita eri komennoilla, että internet-yhteys katkeaa (esim. ping tai curl komennoilla)*

![nettionirti](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/fb95c522-f054-4af7-9b4e-c9160b6f1e5c)

### d) Etsi Metasploitable porttiskannaamalla (db_nmap -sn). Tarkista selaimella, että löysit oikean IP:n - Metasploitablen weppipalvelimen etusivulla lukee Metasploitable. Katso, ettei skannauspaketteja vuoda internettiin - kannattaa irrottaa koneet netistä skannatessa.

Jotta pystytään tallentamaan tulevien testausten tuloksia tietokantaan, pitää alustaa PostgreSQL tietokanta. Eli ekana käynnistin postgren kali-koneella, komennolla ```sudo systemctl start postgresql``` ja komennolla ```sudo systemctl status postgresql``` tarkistin, että se on käynnissä:

![postgreup](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/1c305989-8f47-47e9-8260-352031277d10)

Alustin tietokannan komennolla ```sudo msfdb init```

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/b9d01dcc-afd0-4845-9c7b-6ba7db1af01c)

Käynnistin msfconsolin/metasploitin komennolla ```sudo msfconsole``` ja tarkistin, että se on yhdistetty postgre tietokantaan ```db_status```:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/88217a5b-eb1e-48de-9a3b-26d1c4fc5a76)

Kalin komentorivillä etsin metasploitable-virtuaalikonetta komennolla ```db_nmap -sn 192.168.12.0/24```. Tuloksissa näkyy, että löydettiin 4 hostia, jotka ovat päällä. **192.168.12.5** on kali-virtuaalikone, josta käsin tein skannausta. **192.168.12.3** on etsimäni metasploitable:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/4b49e62b-38ec-46d2-8ef7-63d0d60eb75e)

Tarkistin vielä selaimessa, että löydetty IP-osoite on oikea:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/853c9774-5cbc-4480-8a0f-7e84aee6004e)


### e) Porttiskannaa Metasploitable huolellisesti (db_nmap -A -p0-). Analysoi tulos. Kerro myös ammatillinen mielipiteesi (uusi, vanha, tavallinen, erikoinen), jos jokin herättää ajatuksia.

### f) Murtaudu Metasploitablen VsFtpd-palveluun Metasploitilla (search vsftpd, use 0, set RHOSTS - varmista osoite huolella, exploit, id)

### g) Parempi sessio. Tee vsftpd-hyökkäyksestä saadusta sessiosta parempi. (Voit esimerkiksi päivittää sen meterpreter-sessioksi, laittaa tty:n toimimaan tai tehdä uuden käyttäjän ja ottaa yhteyden jollain tavallisella protokollalla)

### h) Etsi, tutki ja kuvaile jokin hyökkäys ExploitDB:sta. (Tässä harjoitustehtävässä pitää hakea ja kuvailla hyökkäys, itse hyökkääminen jää vapaaehtoiseksi lisätehtäväksi)

### i) Etsi, tutki ja kuvaile hyökkäys 'searchsploit' -komennolla. Muista päivittää. (Tässä harjoitustehtävässä pitää hakea ja kuvailla hyökkäys, itse hyökkääminen jää vapaaehtoiseksi lisätehtäväksi. Valitse eri hyökkäys kuin edellisessä kohdassa.)

### j) Kokeile vapaavalintaista haavoittuvuusskanneria johonkin Metasploitablen palveluun. (Esim. nikto, wpscan, openvas, nessus, nucleus tai joku muu)

### k) Kokeile jotain itsellesi uutta työkalua, joka mainittiin x-kohdan läpikävelyohjeessa.



#### Lähteet:

https://terokarvinen.com/2023/eettinen-hakkerointi-2023/#h3-lab-kid

https://www.youtube.com/watch?v=XB8CbhfOczU&list=PLidcsTyj9JXJfpkDrttTdk1MNT6CDwVZF&index=40

https://www.kali.org/tools/gobuster/

https://sqlmap.org/

https://kulonpaa.com/?p=87

https://sourceforge.net/projects/metasploitable/

https://subscription.packtpub.com/book/security/9781788623179/1/ch01lvl1sec19/configuring-postgresql
