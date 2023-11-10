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

Kali 192.168.12.5/24 ja Metasploit 192.168.12.3

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/4f8253f8-194b-42bf-b6dc-6269ce0e8182)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/957ec739-e9bf-4655-9b86-e82a1998babd)

  *- Osoita eri komennoilla, että internet-yhteys katkeaa (esim. ping tai curl komennoilla)*

![nettionirti](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/fb95c522-f054-4af7-9b4e-c9160b6f1e5c)

### d) Etsi Metasploitable porttiskannaamalla (db_nmap -sn). Tarkista selaimella, että löysit oikean IP:n - Metasploitablen weppipalvelimen etusivulla lukee Metasploitable. Katso, ettei skannauspaketteja vuoda internettiin - kannattaa irrottaa koneet netistä skannatessa.

Jotta pystytään tallentamaan tulevien testausten tuloksia tietokantaan, pitää alustaa PostgreSQL tietokanta. Eli ekana käynnistin postgren kali-koneella, komennolla ```sudo systemctl start postgresql``` ja komennolla ```sudo systemctl status postgresql``` tarkistin, että se on käynnissä:

![postgreup](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/1c305989-8f47-47e9-8260-352031277d10)

Alustin tietokannan komennolla ```sudo msfdb init```

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/b9d01dcc-afd0-4845-9c7b-6ba7db1af01c)

Käynnistin msfconsolin/metasploitin komennolla ```sudo msfconsole``` ja tarkistin, että se on yhdistetty postgre tietokantaan ```db_status```:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/88217a5b-eb1e-48de-9a3b-26d1c4fc5a76)
..
Kalin komentorivillä/msf konsolissa etsin metasploitable-virtuaalikonetta komennolla ```db_nmap -sn 192.168.12.0/24```. Tuloksissa näkyy, että löydettiin 4 hostia, jotka ovat päällä. **192.168.12.5** on kali-virtuaalikone, josta käsin tein skannausta. **192.168.12.3** on etsimäni metasploitable:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/4b49e62b-38ec-46d2-8ef7-63d0d60eb75e)

Tarkistin vielä selaimessa, että löydetty IP-osoite on oikea:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/853c9774-5cbc-4480-8a0f-7e84aee6004e)

### e) Porttiskannaa Metasploitable huolellisesti (db_nmap -A -p0-). Analysoi tulos. Kerro myös ammatillinen mielipiteesi (uusi, vanha, tavallinen, erikoinen), jos jokin herättää ajatuksia.

Skannaus kesti noin 2,5 minuuttia. 

Tuloksissa kerrottiin, että *host is up*:

- **portti 21** on auki/tcp ja siinä on kuuntelemassa ftp palvelu versio *vsftpd 2.3.4*. *Anonyymi FTP login* on sallittu. FTP palvelimen status on, että se on yhdistetty hostiin 192.168.12.5 (kali-virtuaalikone). Ei kaistanleveys rajoituksia (bandwidth limit), sessio aikakatkaistaan 300 sekunnissa, hallinta- eikä datayhteys ole salattua vaan pelkkää tekstiä. 

- **portti 22** on auki/tcp, ssh palvelu versio OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0) kuuntelemassa. Tuloksissa lisäksi näkyy myös ssh-hostkey.

- **portti 23** on auki/tcp, telnet palvelu (Linux telnetd) kuuntelemassa

- **portti 25** on auki/tcp, smtp palvelu (Postfix smtpd) kuuntelemassa:
  
  -  kohta "ciphers", kertoo mitkä salakirjoitusmenetelmät ovat tuettuja
    
  -  smtp-commands: komennot, jotka voidaan tämän palvelun kohdalla käyttää
 
  -  ssl-cert: ssl sertifikaattitiedot

- **portti 53** on auki/tcp, domain palvelu, ISC BIND 9.4.2 (nimipalveluohjelmisto) kuuntelemassa

- **portti 80** auki/tcp, http/Apache httpd 2.2.8 palvelu kuuntelemassa:
  - http-title: Metasploitable2 - Linux

- **portti 111** on auki/tcp, rpcbind 2 (RPC #100000) kuuntelemassa:
  - rpcbind mappaa rpc palveluita porteille, joilla ne ovat kuuntelemassa
  - Googletellessani mikä on rpcbind selvisi, että siinä piilee haavoittuvuus ja heti ensimmäisissä googlaustuloksissa tuli vastaan <a href="https://amolblog.com/111-tcp-open-rpcbind-2-rpc-100000/">artikkeli</a>, joka antoi ohjeen sen hyödyntämiseen (exploit). Tämän portin tietojen alla on myös lista *rpcinfo*, joka ymmärtääkseni listaa mitkä rpc-ohjelmaversiot toimii tai ovat kuuntelemassa milläkin portilla, minkä protokollan alla (tcp/udp), hyödyntäen mitä palveluita (rpcbind, nfs, mountd, ...). Siitä en ole ihan varma, miksi ne on listattu tässä, eikä ym. listassa lueteltujen porttien kohdalla yksitellen. Ehkä siksi, että on selkeämpää kun kaikki rpc/rpcbind:iin liittyvät palvelutiedot ovat niputetuna yhteen paikkaan?

- **portti 139** on auki/tcp, netbios-ssn/Samba smbd 3.X - 4.X (data sharing protocol) kuuntelemassa.

- **portti 445** on auki/tcp, "`?tDV"/Samba smbd 3.0.20-Debian kuuntelemassa. Pitää perehtyä, mikä tuo ihme rimpsu **?tDV** tarkoittaa.

- **portti 512** on auki/tcp, exec/netkit-rsh rexecd kuuntelemassa

- **portti 513** on auki/tcp, login/OpenBSD or Solaris rlogind kuuntelemassa

- **portti 514** on auki/tcp, shell/Netkit rshd kuuntelemassa

- **portti 1099** on auki/tcp; java-rmi/GNU Classpath grmiregistry kuuntelemassa

- **portti 1524** on auki/tcp, bindshell/Metasploitable root shell. Tätä lienee mahdollista hyödyntää (exploit) murtautuessaan Metasploitableen?

- **portti 2049** on auki/tcp, nfs/2-4 (RPC #100003) kuuntelemassa

- **portti 2121** on auki/tcp, ftp/ProFTPD 1.3.1 kuuntelemassa

- **portti 3306** on auki/tcp, mysql/MySQL 5.0.51a-3ubuntu5 kuuntelemassa. Yhteys MySQL tietokantaan. "mysql-info" kohdassa on listattuna esim. "Some Capabilities: Support41Auth (liittynee autentikointiin), ..., ConnectWithDatabase (liittynee johonkin tapaan miten muodostetaan yhteys tietokantaan). Kohta "Salt" littyy käsittääkseni SaltStackiin.

- **portti 3632** on auki/tcp, distccd/v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1 ubuntu4)) kuuntelemassa

- **portti 5432** on auki/tcp, postgresql/DB 8.3.0 - 8.3.7 kuuntelemassa. Tässä löytyy myös tietoja tähän palveluun liittyvään ssl-sertifikaattiin.

- **portti 5900** on auki/tcp, vnc (protocol 3.3) kuuntelemassa. <a href="https://fi.wikipedia.org/wiki/VNC#:~:text=VNC%20%28Virtual%20Network%20Computing%29%20on%20protokolla%20tietokoneen%20graafisen,et%C3%A4pisteell%C3%A4%20tarvitaan%20VNC-%20palvelin%20ja%20toiseen%20p%C3%A4%C3%A4h%C3%A4n%20p%C3%A4%C3%A4teohjelma.">VNC (Virtual Network Computing)</a> on protokolla tietokoneen graafisen käyttöliittymän etäkäyttöön.

- **portti 6000** on auki/tcp, X11 (access denied) kuuntelemassa

- **portti 6667** on auki/tcp, irc UnrealIRCd kuuntelemassa

- **portti 6697** on auki/tcp, irc UnrealIRCd kuuntelemassa

- **portti 8009** on auki/tcp, ajp13 Apache Jserv (Protocol v1.3) kuuntelemassa

- **portti 8180** on auki/tcp, http/Apache Tomcat/Coyote JSP engline 1.1 kuuntelemassa

- **portti 8787** on auki/tcp, drb/Ruby DRb RMI (Ruby 1.8) kuuntelemassa. Ymmärtääkseni Ruby ohjelmointikieleen ja sen käyttämään liittyvä. Tätä googlatessani löysin tiedon, jonka mukaan Ruby RMI 1.8 palvelimeen liittyy <a href="https://www.hackingtutorials.org/metasploit-tutorials/hacking-druby-rmi-server-1-8/">remote code execution-haavoittuvuus</a>.

- **portti 8534** on auki/tcp, status/1 (RPC #100024) kuuntelemassa

- **portti 51722** on auki/tcp, java-rmi/GNU Classpath grmiregistry kuuntelemassa

- **portti 58418** on auki/tcp, mountd/1-3 (RPC #100005) kuuntelemassa

- **portti 59712** on auki/tcp, nlockmgr/1-4 (RPC #100021) kuuntelemassa

Skannaustulosten lopusta löytyy myös tietoja skannattavan kohteen (192.168.12.3/metasploitable) käyttöjärjestelmästä, sen MAC-osoitteesta, verkko-etäisyydestä (1 hop, eli heti "vieressä", ei laitteita välissä), jne. Tuloksissa oli paljon itselleni tuttuja asioita, kuten ftp, ssh, http/apache, telnet, smtp. Toisaalta vastaan tuli myös uutta, kuten rpcbind:iin liittyvät asiat.

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/578e3052-9702-4099-909a-9279378b5e68)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/e196fdbb-6b1e-418a-8f74-adfea2825fb0)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/03f812c6-7016-4401-9874-71ed4a6ae171)

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/00fbe2eb-c515-42e8-a064-19d1e40c3953)


### f) Murtaudu Metasploitablen VsFtpd-palveluun Metasploitilla (search vsftpd, use 0, set RHOSTS - varmista osoite huolella, exploit, id)

Msfconsolissa ```search vsftpd```:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/5be1631d-3c98-4405-afd2-5ea2586bea28)

Tehtävänannossa luki, että tulisi käyttää "0", mutta kuten tuossa alhaalla näkyy, i.ndexinumero 0:n kohdalla on vsftpd:n versiona 2.3..2. Aiemmassa Metasploitablen porttiskannauksessa kävi ilmi, että Metasploitablessa pyörii tuo palvelu versiona 2.3.4. Joten valitsen tuon jälkimmäisen. Halusin vielä lisäinfoa ko. exploitista, joten käytin komentoa ```info 1```:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/a4f88219-97e2-4b10-9ee1-1cb005a8eb36)

Valitsin nr.1:n :

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/790aae79-ee73-486a-8dc0-e49599a8976a)

Ja halusin vielä katsoa mitä vaihtoehtoja löytyy:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/8f24b1a2-0bcd-475b-b807-f1a2486450f6)

Pakollinen kohdeportti (RPORT) oli jo määritelty (21). Itse kohde (RHOST) sen sijaan ei ollut, joten määritin sen komennolla ```set RHOST 192.168.12.3```. Sitten ei kun exploit käyntiin komennolla ```run```:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/5bff3d90-8db8-4299-b634-daa239c53799)

Pääsin onnistuneesti kohteeseen. Testasin komentoa ```pwd``` ja selvisi, että olen juuressa ```/```. Komento ```ls``` sitten listasi mitä ko. kansio sisältää.

### g) Parempi sessio. Tee vsftpd-hyökkäyksestä saadusta sessiosta parempi. (Voit esimerkiksi päivittää sen meterpreter-sessioksi, laittaa tty:n toimimaan tai tehdä uuden käyttäjän ja ottaa yhteyden jollain tavallisella protokollalla)

Päivitetään saatu sessio metterpreteriksi. Löysin selkeän näköisen <a href="infosecwriteups.com/metasploit-upgrade-normal-shell-to-meterpreter-shell-2f09be895646">ohjeen</a>:

-> Background the current (normal shell) session. Eli käsittääkseni se normal shell nyt tallennetaan taustalle. :

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/91ac1990-9af1-43cd-99cf-ec40568cefa4)

-> Käytä komentoa ```search shell_to_meterpreter```, joka etsii ko. moduulia:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/f7ee4a43-aab4-41a2-9e5f-be77f37e0a82)

-> Käytä löydettyä moduulia komennolla ```use 0``` (tai *use post/multi/manage/shell_to_meterpreter*)

-> Nyt pitää konfiguroida, mikä session shell halutaan päivittää. Tarkista komennolla ```sessions -l```. *Session ID* kannattaa muistaa, kohta sitä tarvitaan:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/ba2b09dc-3a83-4297-9f75-076f4d15e2c9)

-> Varmuuden vuoksi/halutessasi voit tarkistaa, mitä vaihtoehtoja voidaan määrittää, ```show options```. Siinä myös nähdään, onko kaikki pakolliset asiat jo määritelty:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/9f5def9e-55ed-4bd7-8b5e-8348d9650b86)

-> Kuten yllä olevasta kuvasta näkee, vielä pitää määrittää *session id*, ```set SESSION 1```:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/100e23da-25b8-43c1-80b5-388aed12ed59)

-> Execute exploit. Eli, ```run```:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/1d7dcd6d-f94e-416f-8b75-c424ea991e67)

-> Listaa shellit, jotka voidaan päivittää ```sessions -l```: 

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/c9324729-8f4d-4bd2-a87b-db6dadfc1a11)

-> Valitse haluamasi. Tässä tapauksessa ```session -i 2```. Ja session shell on päivitetty:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/e6d27f0d-6a9c-4ca2-8a61-65a245397dfe)

Käyttämällä komentoa ```help``` saadaan lista komennoista, jotka voidaan käyttää. Lista on järjestelty kategorioittain:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/08fd6fa4-032c-457d-a888-d7844b670edc)

Esimerkiksi komennolla ```mic_list``` voidaan listata kaikki kohdejärjestelmän mikrofoneja:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/70aed682-bb66-40c0-9ebc-8a792a3d3dfb)

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

https://fi.wikipedia.org/wiki/VNC#:~:text=VNC%20%28Virtual%20Network%20Computing%29%20on%20protokolla%20tietokoneen%20graafisen,et%C3%A4pisteell%C3%A4%20tarvitaan%20VNC-%20palvelin%20ja%20toiseen%20p%C3%A4%C3%A4h%C3%A4n%20p%C3%A4%C3%A4teohjelma.

https://www.isc.org/bind/

https://unix.stackexchange.com/questions/234154/exactly-what-does-rpcbind-do

https://amolblog.com/111-tcp-open-rpcbind-2-rpc-100000/

https://www.hackingtutorials.org/metasploit-tutorials/hacking-druby-rmi-server-1-8/

https://infosecwriteups.com/metasploit-upgrade-normal-shell-to-meterpreter-shell-2f09be895646

https://medium.com/@JAlblas/tryhackme-metasploit-meterpreter-walkthrough-e71e36e8b280
