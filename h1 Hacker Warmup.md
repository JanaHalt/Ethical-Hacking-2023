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





***Lähteet:***
https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/ 
https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf 

