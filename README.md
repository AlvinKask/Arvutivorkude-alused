# Networking for Web Developers
#
# FROM PING TO HTTP  
**ping -c3 8.8.8.8 - käsklus saadab vastavale netilehele 3 testsõnumit, lõpetab ning kuvab vastused.**  
- Meil on olemas internet.  
-  8.8.8.8 on "üleval" ja töötab ning meie suudame "rääkida" temaga.  
-  Meie ISP (internet service provider) suudab suhelda Google võrguga.  

**Ping ja HTTP ei ole samad asjad.**  
- Ping on lihtsam ning HTTP ei baseeru sellel.  
- HTTP saab tagasi interneti lehekülje.  
- Ping suhtleb teise poole operatsioonisüsteemiga (serverit pole).  
- Iga operatsioonisüsteem, mis toetab internetti, toetab pingimist ka.  

**printf 'HEAD / HTTP/1.1\r\nHost: en.wikipedia.org\r\n\r\n' | nc en.wikipedia.org 80**  
- Tegu HTTP (port 80) päisega leheküljelt Wikipedia.org.  
- SSH server (port 22).  
- printf (formatted string) kasutatakse, et mitu käsklust kokku panna. Printf on targem ning suudab kasutada endas teisi käsklusi (\r, \n) .  
- nc (netcat) loeb ja kirjutab võrguühenduste kohta. Ühendub pordiga ja saadab sinna käskluse. **netcat tegeleb ühe käsklusega korraga.**  
  - man nc - kuvab nc manuali, kust saab käsklused, mida kasutada.  

- | (pipe) eraldab väljundi sisendist.  
  - Seega, meil 2 programmi: printf ja |. printf on HTTP päring, mille tulemust kasutatakse nc sisendina. Nc saadab selle lehele ja seejärel kuvab tulemuse.  

**Kõik võrgukihid sõltuvad alumistest pakkudes tuge ülemistele (Vaata allolevat pilti).**  
![Kihid](https://user-images.githubusercontent.com/115221752/196022094-709f5e60-f4cd-49b0-b392-82833ccc6c8c.JPG)

**Kasutades port 3456 suhtlemiseks:**  
- Tuleb avada 2 klienti: nc -l 3456 & nc localhost 3456.  
    - Pordi kuulamiseks on käsk: nc -l 3456 
- Mõlemad saavad kirjutada, lugeda ja lõpetada vestlust (ctrl D).  

**Pordinumbrid eristavad erinevaid rakendusi ja sessioone samade host'ide juures.**  
- Kõrgeim port, mida nc saab kuulata: 65535  
- Madalaim port, mida nc saab kuulata: 1024  

**Pordid 1023 on reserveeritud root kasutajale.**  
- Sama pordi kuulamine annab veateate, et port kasutuses.  

**sudo lsof -i - käsklus annab edasi kuulatavad või juba ühendatud võrguühendusi.**  

**printf 'HTTP/1.1 302 Moved\r\nLocation: https://www.eff.org/' | nc -l 2345**  
- Kuna me netcat'iga kuulame port 2345, siis veebilehitsejat kasutades on meil võimalik minna samale pordile ning seejärel näeme me vastavat lehekülge.  
- Põhimõtteliselt ühendame pordi veebilehe külge ning seda ühendust on võimalik veebilehitsejaga "kuulata" meie IP aadressi ja 2345 porti (192.168.1001:2345).  
#
#
# NAMES AND ADDRESSES
**Packet, sisaldab nii saatva, kui ka sihtkoha masina IP'd.**  
**Host, masin internetis, mis pakub teenust.**  
**Endpoint, 2 masinat või programmi, mis suhtlevad omavahel üle võrgu.**  

**ping yahoo.com**  
- Käsklus ping peab otsima host'i IP aadressi enne, kui saab saata ping sõnumit.  
- Töötab, kuni vajutatakse Ctrl+C.  
- Lisades ping käsklusele -c 7 saadetakse yahhoo.com lehele 7 ping sõnumit (ping -c 7 yahoo.com)  

**DNS - Domain Name System**  
- Hostname "tõlgitakse" IP-aadressiks (www.example.net -> 93.184.216.34  
  - DNS A-record hoiab endas aadressi infot ja kliendiprogramm (veebibrauser) otsivad sealt aadressi.  

**DNS kliendi kood "resolver"**  
- On sisseehitatud kõigisse operatsioonisüsteemidesse ning nc ja ping saavad seda kasutada.  

**host - käsklus, mis otsib andmed meie DNS kohta**  
- host -t a - otsib aadressi.  
  - man host - kuvab host manuali, kust saab käsklused, mida kasutada.  
    - dig - käsklus, mis otsib scriptilaadseid andmed DNS kohta  

**CNAME - canonical name ehk alias**  
**AAAA ("quad-A") - IPv6 aadress**  
**NS - DNS name server**  

**Klient küsib example.com aadressi**
![Nimeotsing](https://user-images.githubusercontent.com/115221752/196024194-001593ca-e5ae-4089-aec1-91f38e393136.JPG)

**IPv4 - 4 numbrit mida eraldavad punktid** 
**IPv6 - 8 numbri ja tähekombinatsiooni mida eraldavad koolonid** 

**IPv4 erinevates numeratsioonides**
![IPv4](https://user-images.githubusercontent.com/115221752/196024749-1c8b5315-50ed-402e-ac2d-d226bf0f3e32.JPG)

#
#
# ADDRESSING AND NETWORKS
**1/8 IPv4 aadressidest on kõrvale pandud testimiseks, dokumentatsiooniks jne.**  
- IPv4 aadress on punktidega eraldatud numbrite jada (et oleks lihtsam jälgida).  
	- Arvuti näeb seda 32-bit'ise numbrite jadana.  

**IPv4 address map - EI OLE ÕIGE PILT AGA ANNAB IDEE EDASI**  
![address map](https://user-images.githubusercontent.com/115221752/196353026-a9f941ce-77aa-401b-b441-794a3d1ccd8f.JPG)

Helerohelised ruudud (0, 10 ja 127) on plokid, mis on täielikult reserveeritud.  
Tumerohelised ruudud on osaliselt reserveeritud plokid. Näiteks ei ole 192 plokist kõik reserveeritud, kuid osa on reserveeritud.  
Kogu tsüaanrida (alates 224-st) on IP multisaadete jaoks ette nähtud.  
Ja kogu oranž alumine rida (alates 240-st) eraldati algselt "edaspidiseks kasutamiseks", kuid kaotati käibelt, kuna see blokeeriti kehtetuks. Vale planeerimise tõttu kaotasime 1/16 kõigist IPv4 aadressidest.  

**IPv4 aadresse on maksimaalselt 4294967296 (32bit) - seda on peaaegu 2x vähem, kui planeedil elanikke.**  

**Kaldkriipsuga saab määratleda ära, kui palju kohti on IPv4 aadressil jäetud host'i jaoks**  
![22 host](https://user-images.githubusercontent.com/115221752/196352222-311e3e99-2050-41e8-b3d6-4f47aa7b3756.JPG)

- /22 võrgu puhul on 10bit'i jäetud host'ile ehk siis 2 astmel 10 = 1024 aadressi.  
	- Kasutatav on 1021 (esimene ja viimane on reserveeritud ja sellest esimene on harilikult ruuterile).  
- /24 võrgu puhul on 8bit'i jäetud host'ile ehk siis 2 astmel 8 = 256 aadressi.  
	- Kasutatav on 253 (esimene ja viimane on reserveeritud ja sellest esimene on harilikult ruuterile). 

**/24 võrgu subnet mask on 255.255.255.0**  
![24 subnet mask](https://user-images.githubusercontent.com/115221752/196352249-2b2116a6-3476-497e-bcfe-7a001bef9fa4.JPG)

**171.64.0.0/14 Stanford University subnet IPv4 näide**  
![Stanford Uni](https://user-images.githubusercontent.com/115221752/196352271-775ee9cb-29f2-40ae-9c5c-2a480b7af006.JPG)

**Ühel arvutil võib olla mitu liidest, mis kõik omavad aadressi (ip addr show):**  
- eth0 = kaabliga internet  
- wlan0 = wifi  
- lo = loopback interface (suhtleb arvuti erinevate programmidega)
- tunnel interface
- virtual machine interface

**Ruuter ühendab erinevad IP-võrgud ning host'id teavad default gateway'd**  
![Gate1](https://user-images.githubusercontent.com/115221752/196423817-d21e247f-6088-4e58-8fa0-d18ea1d1bbea.JPG)
![Gate2](https://user-images.githubusercontent.com/115221752/196423854-187c21eb-c369-45dd-9970-b08a21e5c32a.JPG)

**ip route show default (Linux) & netstat -nr (Linux, Mac, Unix)**  
- näitab default gateway IPv4 aadressi

**Kodune võrk suhtleb välismaailmaga läbi ruuteri avaliku IPv4 aadressi**  
![Gate1](https://user-images.githubusercontent.com/115221752/196427425-eec72ae4-ca16-41b4-97b1-fe3569098b76.JPG)

**Kodune võrk jaguneb nii** 

![Võrk2](https://user-images.githubusercontent.com/115221752/196427566-d6674d51-6156-4731-ba21-feff6ef4ecc7.JPG)

**Privaatseid IPv4 aadresse kasutatakse koos NAT (network aadress translation) süsteemiga** 
- Ükskõik millal käib liiklus privaatse ja avaliku võrgu vahel, peab ruuter kirjutama või tõlkima ümber sellel olevad aadressid.  
- Ruuteri sees on "kaart", mis teab milline sisemine aadress (koos pordiga) on seotud millise avaliku aadressiga (koos pordiga).  
	- Kuna IPv4 aadresse on vähem, kui inimesi Maal ning iga inimene kasutab mitut seadet, siis NAT aitab "petta" süsteemi ning loob workaround'i IP vähesuse probleemile.  
![Võrk3](https://user-images.githubusercontent.com/115221752/196429163-3b3c78e9-3910-48e6-8c0b-01a65238fb34.JPG)

**Privaataadresse saab kasutada ainult koduses võrgus**  
- Maailma mõistes pole nad unikaalsed  
- Kõik nad peituvad NAT taha  

**IPv6 vs IPv4**  
- 128bit vs 32bit  
- 16 okdeti (8 kohalist 1- ja 0 jada) vs 4 okdeti  
- 2^128 aadressi vs 2^32 aadressi  
- Kõige väiksem osa, mida lõppkasutajale anda on /64 vs 32 terve IPv4 peale kokku  
- hex vs kümnendsüsteem  
![vs](https://user-images.githubusercontent.com/115221752/196766977-a0065cb8-6f35-4e4a-b568-8c580744fa1a.jpg)

**http://test-ipv6.com/**
#
#
# Teema 4
The Alcázar of Segovia is a medieval castle located in the city of Segovia in Castile and León, Spain. Rising out on a rocky crag above the confluence of two rivers near the mountains of Guadarrama, it is one of the most distinctive castle-palaces in Spain by virtue of its shape, resembling the bow of a ship. The alcázar was originally built in the 11th or 12th century by the Almoravid dynasty as a fortress, but has since served as a royal palace where twenty-two kings reigned, a state prison, a royal artillery college, and a military academy. The castle overlooks a valley with the Eresma River and is a symbol of the old city of Segovia. It was declared a UNESCO World Heritage Site in 1985. Today, the alcázar is used as a museum and a military archives building since its declaration as a national archive by royal decree in 1998.
#
#
# Teema 5
The Alcázar of Segovia is a medieval castle located in the city of Segovia in Castile and León, Spain. Rising out on a rocky crag above the confluence of two rivers near the mountains of Guadarrama, it is one of the most distinctive castle-palaces in Spain by virtue of its shape, resembling the bow of a ship. The alcázar was originally built in the 11th or 12th century by the Almoravid dynasty as a fortress, but has since served as a royal palace where twenty-two kings reigned, a state prison, a royal artillery college, and a military academy. The castle overlooks a valley with the Eresma River and is a symbol of the old city of Segovia. It was declared a UNESCO World Heritage Site in 1985. Today, the alcázar is used as a museum and a military archives building since its declaration as a national archive by royal decree in 1998.
#
#
#
# ÕPPEJÕU MÄRKMED (Toivo Pärnpuu)

#Google Cloud shell  
Kursuse keskkonnas kasutatavate Linuxi käskude jaoks soovitan kasutada: https://cloud.google.com/shell  
Cloud Shelli saab kasutada 50 tundi nädalas.  

#Enne kasutamist käivita käsud:  
sudo apt-get update  
sudo apt-get install netcat-openbsd tcpdump traceroute mtr iputils-ping lsof  


10.1.2.3

0.0.0.0
255.255.255.255 

0 - välja lülitatud / false  
1 - sisse lülitatud / true  

2^3  
2*2*2  
				  040.E3C.C31.D3B  
MAC				  04:0E:3C:C3:1D:3B  
Physical Address: 04-0E-3C-C3-1D-3B  

IPv4 Address:     192.168.000.113  
Subnet Mask :     255.255.255.000  
Default Gateway:  192.168.000.001 < väljapääs teistesse võrkudesse ja internetti.  
DNS Servers . . : 192.168.000.001 < osutab meile IP ja nimede teisendamise teenust  

DNS resolver  
- hosts?  
- võtis esimese DNS serveri ja saatis talle küsimuse, mis on err.ee aadress?  

DHCP Server
annab võrgukonfi 

unicast
broadcast
multicast

printf 'HTTP/1.1 302 Moved\r\nLocation: https://www.eff.org/' | nc -l 2345

IPv4 88.196.5.163  
IPv6 2001:db8:3333:4444:5555:6666:7777:8888  

192.168.000.113  
0000 0000. 0000 0000. 0000 0000 . 0000 0000  
1111 1111. 1111 1111. 1111 1111 . 1111 1111  
2^32 = 4 294 967 296  
7 979 443 621  
4 294 967 296  
- iga arvuti / seade aadress peab olema unikaalne?  
mure 1995?  

IPv6 - töörühm
NAT 

avalikud IP-d (public IP)
privaatvõrgu IP-d (private IP)


TTL - Time To Live

toivo.parnpuu@tptlive.ee 

kasutajnimi@server 
kasutajanimi@ipaadress

Kõik maailma domeeninime tipud:
https://www.iana.org/domains/root/db

DNS-ist
https://tparnpuu.webhosting.tptlive.ee/h5p/wp-admin/admin-ajax.php?action=h5p_embed&id=1

Wireshark
https://www.wireshark.org/

TCPDUMP
#
#
#
# MUUD MÄRKMED
- https://github.com/Tallinna-Polutehnikum/KTA-22E-networks
- https://www.markdownguide.org/basic-syntax
- https://www.udacity.com/course/networking-for-web-developers--ud256
- https://cloud.google.com/shell
- https://webhosting.tptlive.ee/
- https://education.github.com/pack

- siin on jutt
  -  siin ka


## kood
`hello world()`

## koodi blokk
```
rida 1
rida 2
rida 3
```

https://www.markdownguide.org/basic-syntax

Teha 1-5 teemade jaotus, oma jutt, mõtted ja kommentaarid

Näitab liidese IP aadressi
- IPv4 Address. . . . . . . . . . . : 192.168.0.33        Esimesed 3 jaotust sümboliseerivad minu koduaadressi; viimane osa sümboliseerib kindlat arvutit
- Subnet Mask . . . . . . . . . . . : 255.255.255.0       
- Default Gateway . . . . . . . . . : 192.168.0.1         Väljapääs teistesse võrkudesse ja internetti
- DNS Servers . . . . . . . . . . . : 192.168.0.1         Osutab meile IP ja nimede teisendamise teenust
- MAC . . . . . . . . . . . . . . . : 04:0E:3C:C3:1C:5A   
- Physical Address. . . . . . . . . : 04-0E-3C-C3-1C-5A   16-süsteemis

DHCP Server - annab meile võrgukonfiguratsiooni
DNS resolver  - otsib hosts?
              - võttis vastu soovi minna lehele www.err.ee, võtab serveriga ühendust ning seejärel suunab meid aadressile 141.101.90.16
