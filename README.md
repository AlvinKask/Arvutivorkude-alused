# Networking for Web Developers
#
# FROM PING TO HTTP  
**ping -c3 8.8.8.8 käsklus saadab vastavale netilehele 3 testsõnumit, lõpetab ning kuvab vastused.**  
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
- nc (netcat) loeb ja kirjutab võrguühenduste kohta. Ühendub pordiga ja saadab sinna käskluse.  
- | (pipe) eraldab väljundi sisendist.  
  - Seega, meil 2 programmi: printf ja |. printf on HTTP päring, mille tulemust kasutatakse nc sisendina. Nc saadab selle lehele ja seejärel kuvab tulemuse.  

**Kõik võrgukihid sõltuvad alumistest pakkudes tuge ülemistele (Vaata allolevat pilti).**  
![Kihid](/assets/images/Kihid.JPG)

*
man nc - kuvab nc manuali, kust saab käsklused, mida kasutada.
- pordi kuulamiseks on käsk: nc -l 3456

*
Kasutades port 3456 suhtlemiseks:
- Tuleb avada 2 klienti: nc -l 3456 & nc localhost 3456.
- Mõlemad saavad kirjutada, lugeda ja lõpetada vestlust (ctrl D).

*
Pordinumbrid eristavad erinevaid rakendusi ja sessioone samade host'ide juures.
Kõrgeim port, mida nc saab kuulata: 65535
Madalaim port, mida nc saab kuulata: 1024

*
Pordid 1023 on reserveeritud root kasutajale.
- Sama pordi kuulamine annab veateate, et port kasutuses.

*
sudo lsof -i
Käsklus annab edasi kuulatavad või juba ühendatud võrguühendusi.

*
printf 'HTTP/1.1 302 Moved\r\nLocation: https://www.eff.org/' | nc -l 2345
- Kuna me netcat'iga kuulame port 2345, siis veebilehitsejat kasutades on meil võimalik minna samale pordile ning seejärel näeme me vastavat lehekülge.
- Põhimõtteliselt ühendame pordi veebilehe külge ning seda ühendust on võimalik veebilehitsejaga "kuulata" meie IP aadressi ja 2345 porti (192.168.1001:2345)

*
netcat tegeleb ühe käsklusega korraga.
### Tulemus
#
#
# Teema 2
The Alcázar of Segovia is a medieval castle located in the city of Segovia in Castile and León, Spain. Rising out on a rocky crag above the confluence of two rivers near the mountains of Guadarrama, it is one of the most distinctive castle-palaces in Spain by virtue of its shape, resembling the bow of a ship. The alcázar was originally built in the 11th or 12th century by the Almoravid dynasty as a fortress, but has since served as a royal palace where twenty-two kings reigned, a state prison, a royal artillery college, and a military academy. The castle overlooks a valley with the Eresma River and is a symbol of the old city of Segovia. It was declared a UNESCO World Heritage Site in 1985. Today, the alcázar is used as a museum and a military archives building since its declaration as a national archive by royal decree in 1998.
### Tulemus
#
#
# Teema 3
The Alcázar of Segovia is a medieval castle located in the city of Segovia in Castile and León, Spain. Rising out on a rocky crag above the confluence of two rivers near the mountains of Guadarrama, it is one of the most distinctive castle-palaces in Spain by virtue of its shape, resembling the bow of a ship. The alcázar was originally built in the 11th or 12th century by the Almoravid dynasty as a fortress, but has since served as a royal palace where twenty-two kings reigned, a state prison, a royal artillery college, and a military academy. The castle overlooks a valley with the Eresma River and is a symbol of the old city of Segovia. It was declared a UNESCO World Heritage Site in 1985. Today, the alcázar is used as a museum and a military archives building since its declaration as a national archive by royal decree in 1998.
### Tulemus
#
#
# Teema 4
The Alcázar of Segovia is a medieval castle located in the city of Segovia in Castile and León, Spain. Rising out on a rocky crag above the confluence of two rivers near the mountains of Guadarrama, it is one of the most distinctive castle-palaces in Spain by virtue of its shape, resembling the bow of a ship. The alcázar was originally built in the 11th or 12th century by the Almoravid dynasty as a fortress, but has since served as a royal palace where twenty-two kings reigned, a state prison, a royal artillery college, and a military academy. The castle overlooks a valley with the Eresma River and is a symbol of the old city of Segovia. It was declared a UNESCO World Heritage Site in 1985. Today, the alcázar is used as a museum and a military archives building since its declaration as a national archive by royal decree in 1998.
### Tulemus
#
#
# Teema 5
The Alcázar of Segovia is a medieval castle located in the city of Segovia in Castile and León, Spain. Rising out on a rocky crag above the confluence of two rivers near the mountains of Guadarrama, it is one of the most distinctive castle-palaces in Spain by virtue of its shape, resembling the bow of a ship. The alcázar was originally built in the 11th or 12th century by the Almoravid dynasty as a fortress, but has since served as a royal palace where twenty-two kings reigned, a state prison, a royal artillery college, and a military academy. The castle overlooks a valley with the Eresma River and is a symbol of the old city of Segovia. It was declared a UNESCO World Heritage Site in 1985. Today, the alcázar is used as a museum and a military archives building since its declaration as a national archive by royal decree in 1998.
### Tulemus
#
#

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
