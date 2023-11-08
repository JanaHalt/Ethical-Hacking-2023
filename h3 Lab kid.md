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

- tässä "boxissa" murtauduttiin helpdeskz portaaliin

- käytettiin nmapia avoimien porttien löytämiseen

- <a href="https://www.kali.org/tools/gobuster/">gobuster</a> työkalun (*scans websites for directories*) avulla löydettiin **/support** sivu, jota kokeiltiin selaimessa lisäämällä **/support** sivun ip-osoitteen perään -> johti oikeannäköiseen *helpdesk/support* sivuun

- *searchsploiti*in avulla löydettiin 2 explotia: *a file upload* ja *SQL injection*. Komennon muoto ```searchsploit helpdeskz```. Hyödynnettiin ensimmäistä mainittua lataamalla sivulle luodun tiketin mukana myös muokattu PHP skripti

- selvitettiin Helpdeskz githubin sivulta, mihin ladatatut tiedostot tallennetaan

- selvisi, että sivusto käyttää graphql. Tämä tieto hyödynnettiin sellaisen käyttäjätunnuksen ja salasanan etsimisessä, jolla sitten pystyttiin kirjautumaan helpdeskz sivulle.

- Helpdeskz sivulle kirjautumisen jälkeen löytyi myös boolean sql haavoittuvuus ja 

