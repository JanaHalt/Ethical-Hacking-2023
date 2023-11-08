## <a href="https://terokarvinen.com/2023/eettinen-hakkerointi-2023/#h3-lab-kid">h3 Lab kid</a>

Kolmas viikko käyntiin hyökkäyslabran rakentelun merkeissä. Lisäksi edetään kybertappoketjussa tiedustelusta uusin vaiheisiin: aseistus, toimitus, exploit, asennus ja kontrollikanava. 

### Lue/katso ja tiivistä:

- <a href="https://learning.oreilly.com/library/view/mastering-kali-linux/9781801819770/Text/Chapter_10.xhtml#_idParaDest-257">Velu 2022: Mastering Kali Linux for Advanced Penetration Testing 4ed: Chapter 10 - Exploitation</a>

Kyseinen kirja ei löydy HH-Finnasta:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/dd6fa373-066e-47aa-8a8e-ba7428d50d8c)

Lisäksi, koska aiemmin kurssilla käytettiin tiivistämistehtävän lähteenä O-reilly:n aineistoa ja sen yhteydessä rekisteröin O-reilly:in, niin saan tämän ilahduttavan kuvan ruudulleni:

![image](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/8a8c7124-ee9c-4e8c-9f89-1586d1a9a773)

**1) The Metasploit Framework**

**2) The exploitation of targets using Metasploit**

**3) Using public exploits**

- Vapaavalintainen läpikävely <a href="https://0xdf.gitlab.io/">0xdf</a> tai <a href="https://www.youtube.com/channel/UCa6eh7gCkpPo5XXUDfygQQA/videos">ippsec</a>.

Valitsin <a href="https://www.youtube.com/watch?v=XB8CbhfOczU&list=PLidcsTyj9JXJfpkDrttTdk1MNT6CDwVZF&index=40">HackTheBox - Help</a>, <a href="https://www.youtube.com/@ippsec">IppSec - YouTube kanava</a>lta.

- nmapilla tutkittiin, mitkä portit ovat auki ```nmap -sC -sV -oA nmap/help 10.10.10.121```

- <a href="https://www.kali.org/tools/gobuster/">gobuster</a> työkalua (*scans websites for directories*) hyödyntäen löydettiin **/support** sivu, jota kokeiltiin selaimessa lisäämällä **/support** sivun ip-osoitteen perään -> johti oikeannäköiseen *helpdesk/support* sivuun

- *searchsploit* -> ```searchsploit helpdeskz``` komentorivillä. Löydettiin 2 exploitia: *a file upload* ja *SQL injection*. Ekan mainitun kohdalla luki, että exploit esiintyi helpdeskz sivun versiolla 1.0.2 (joka oli myös sen hetkinen versio)

- tutkittuaan aiemmin mainittua *a file upload* exploitia selvisi, että se tarvitsee palvelimen (jolla helpdeskz on) ajan

- luotiin tiketti helpdeskz sivulla ja ladattiin sen mukana muokattu PHP skripti

- 

