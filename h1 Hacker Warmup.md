***h1 Hacker Warmup - viikon 43 läksyt***

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
- AMD Ryzen 7 4700U with Radeon Graphics, 2000 Mhz, 8 ydin(tä)
- RAM 16 GB
- Windows 11 Home

Varsinainen toteutus VirtualBoxissa virtuaalikoneella, johon on asennettu Fedora 23 desktop.

**a) Ratkaise <a href="https://overthewire.org/wargames/bandit/">Over The Wire: Bandit</a> kome ensimmäistä tasoa (0-2):**

Eli <a href="https://overthewire.org/wargames/bandit/bandit0.html">Bandit Level 0</a> oli selkeä ohje miten menetellä. Avasin virtuaalikoneellani komentorivin ja kirjauduin peliin:
![1](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/fcfb0ad8-dae8-46c2-af07-f865dc75f5f0)

Kirjautuminen onnistui, eli taso suoritettu onnistuneesti:

![2](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/5f8a261a-121e-4240-8819-4cc457846ce6)

Alkutekstissä luki muutamia ohjeita miten alustalla toimitaan, esimerkiksi mistä löytyy tasot ja mistä salasanat yms. 

Tästä sitten jatkoin tasolle *Level 1*. Suoritettu onnistuneesti:

![3](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/ad69bb3a-9911-401c-b54f-93da855e3072)

Taso 2 suoritettu onnistuneesti:

![4](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/58afaa16-edab-450a-ae69-80bfd4ed33c3)

Tämän tason suorituksessa piti opetella miten avataan/luetaan tiedostoja, jotka alkavat "-":lla. Apu löytyi googlen avulla täältä <a href="https://stackoverflow.com/questions/42187323/how-to-open-a-dashed-filename-using-terminal">How to open a dashed filename using terminal</a>.

**b) Ratkaise <a href="https://challenge.fi/">Challenge.fi:sta</a> yksi tehtävä.**

Piti ratkaista yksi tehtävä, mutta kun oli niin mukavaa ja pääsin vauhtiin, niin ratkaisin kaksi. Ensimmäistä mietin aika pitkään ja kun en millään meinannut päästä eteenpäin, turvauduin yhteen vinkkiin. Sitä kautta pääsin kärryille, millaisen salakirjoitussäännön (cipher) mukaan oli kirjaimia vaihdettu toisiin. Hankaluutta tuotti se, kun ensin yritin vaihtaa kirjaimia suomalaisten aakkosten mukaan. Se ei tuottanut järkevää tulosta, sillä lopputuloksena oli mitään sanomaton rivi kirjaimia. Vasta kun vaihdon kirjaimet pelkästään perusaakkosten mukaan (eli jätin å, ä, ö pois), sain oikean lopputuloksen.

Toinen tehtävä oli myös mielenkiintoinen. Avasin kuvan ja ajattelin, että tämähän on helppoa - siinä oli sen verran tutun näköinen paikka. Väärässä olin :D Yritin kirjoittaa paikan nimen monessa eri muodossa, mutta yksikään ei ollut oikein. Lopulta tutkin vielä kuvan ominaisuuksia (properties, kuten milloin otettu, gps, millä kameralla, jne) ja sitä kautta pääsin oikeaan lopputulokseen.

![16a](https://github.com/JanaHalt/Ethical-Hacking-2023/assets/78509164/3f379bce-7c56-446a-85e6-3b059dadc306)


***Lähteet:***

https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/ 
https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf 
https://stackoverflow.com/questions/42187323/how-to-open-a-dashed-filename-using-terminal
https://en.wikipedia.org/wiki/ROT13 
https://www.gps-coordinates.net/gps-coordinates-converter/


