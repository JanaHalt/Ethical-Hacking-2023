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
  

