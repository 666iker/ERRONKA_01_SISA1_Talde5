# Sareen Diseinu eta Inplementazioaren Memoria: Streaming Plataforma (Netflix Estiloa)

**Zentroa:** CIFP Uni Eibar-Ermua LHII  
**Zikloa:** SISA1 (2026-2027)  
**Egileak (5. Taldea):** Axular Muñoz, Iker Monkayola, Alex Sierra, Jokin Martinez  
**Modulua:** Sareak Administratzea eta Planifikatzea (1. Erronka)  

---

## 1. Sarrera eta Helburua

Dokumentu honen helburua Netflix estiloko multimedia streaming plataforma baten **sare-azpiegituraren diseinua eta inplementazio praktikoa** azaltzea da, 1. Erronkan garatutako proiektu orokorraren barruan.

Proiektu orokorrean, gure enpresak bideo-edukien plataforma bat sortu du. Horretarako:
* **Datu-Baseak moduluak** erabiltzaileak, rolak (infantila, pertsonala, familiarra), harpidetzak eta edukien katalogoa (filmak, serieak, kapituluak) kudeatzen ditu.
* **Sistema Eragileak moduluak** Windows Server 2022 instalatu du RAID biltegiratzearekin (bideoak seguru gordetzeko) eta IIS web-zerbitzariarekin.
* **Sareak moduluak (dokumentu hau)** zerbitzariaren eta bezeroen arteko komunikazio-bide guztiak eraiki eta bermatu ditu: kableatu egituratu profesionala tailerrean (Keystone eta Patch Panel), switch bidezko kudeaketa, zerbitzariaren pasahitzaren errore-kontrola (**CRC-6-GSM**), modulazio digitalak (**ASK/FSK/PSK**) eta haririk gabeko sarbidea (**Wi-Fi**) mugikorretik streaming bideoak atzitu ahal izateko.

---

## 2. Jarraitutako Urratsak (Metodologia)

Sarearen inplementazioa bost urrats nagusitan banatu dugu:

1. **Sare-Arkitektura eta OSI Eredua:** Streaming azpiegiturako gailuak OSI 1. eta 2. mailen arabera aztertu eta antolatzea.
2. **Pasahitzaren Transmisioa eta CRC-6-GSM Kalkulua:** Windows Server zerbitzarira sartzeko sortutako pasahitza (`Kuper-67`) ASCII bitetara bihurtzea eta transmisio-erroreak kontrolatzeko zatiketa polinomiala eskuz garatzea.
3. **Modulazio Digitalak eta Manchester Kodifikazioa:** Banda-zabalera handitzeko modulazio konbinatua ($PSK(2)+ASK(2)+FSK(2)$ sinbolo bakoitzeko 6 bit) eta Etherneteko Manchester lerro-kodifikazioa marraztea.
4. **Kableatu Egituratuaren Muntaia Fisikoa:** UTP Cat6 kableak prestatu, TIA/EIA-568B estandarra aplikatuz mahaietako Keystone rosetetan eta rack-eko Patch Paneleko 18. atakan ziztatu eta LAN testerrarekin baieztatzea.
5. **Switch Konmutadorea eta Konektibitate Osoa:** Izar-topologia ezartzea, IP helbide finkoak esleitzea eta gailu guztien arteko komunikazioa (`ping` bidez) egiaztatzea, mugikorretik streaming webgunea atzituz.

---

## 3. Datu-sareen Egitura eta Transmisio Printzipioak (IE1)

### 3.1. Sareko Gailuak eta Eredu Funtzionala (OSI / TCP-IP)

Streaming plataforma baten sareak latentzia baxua eta fidagarritasun handia eskaini behar ditu bideoak etenik gabe erreproduzitu daitezen. Gailu bakoitzak funtzio jakin bat betetzen du OSI ereduaren geruzen arabera:

| Gailua / Elementua | OSI Geruza | TCP/IP Geruza | Datu-Unitatea (PDU) | Funtzioa Gure Streaming Sarean |
| :--- | :--- | :--- | :--- | :--- |
| **UTP Cat6 Kablea, Keystone eta Patch Panela** | 1. Maila (Fisikoa) | Sarbide Geruza | Bita | Seinale elektrikoak garraiatzen dituzte rack-etik ikasgelako lan-estazioetara. |
| **Switch-a (Konmutadorea)** | 2. Maila (Datu-lotura) | Sarbide Geruza | Trama (*Frame*) | MAC helbideak erabiliz, streaming trafikoa dagokion atakara bideratzen du talkarik gabe. |
| **Sare-Txartela (NIC)** | 1. eta 2. Mailak | Sarbide Geruza | Bita / Trama | Zerbitzariari eta bezeroei konexio fisikoa eta MAC helbide fisiko finkoa ematen die. |
| **Wi-Fi Access Point-a (AP)** | 1. eta 2. Mailak | Sarbide Geruza | Bita / Trama | Uhin elektromagnetikoen eta sare kableatuaren arteko zubi gardena (Bridge) egiten du. |
| **Routerra (Bideratzailea)** | 3. Maila (Sarea) | Internet Geruza | Paketea (*Packet*) | IP helbide logikoen arabera azpisare desberdinen arteko bideraketa egiten du. |

---

### 3.2. Pasahitzaren ASCII Kodeketa eta CRC-6-GSM Errore-Detekzioa

Erronkaren aginduan zehazten zen bezala, Windows Server zerbitzarian sartzeko sortu dugun pasahitz sendoa hartu dugu: **`Kuper-67`**.

Pasahitza sarean zehar fidagarritasunez bidaltzeko, transmisioan bitik aldatu ez dela bermatu behar da. Horretarako, erroreak detektatzeko **CRC-6-GSM** algoritmoa aplikatu dugu:

#### 1. ASCII Kodeketa Bitarra:
Pasahitzaren karaktere bakoitza bitetara pasatu dugu ASCII taula erabiliz (64 bit guztira):

| Karakterea | ASCII Hamartarra | ASCII Hamaseitarra | ASCII Bitarra (8 bit) |
| :---: | :---: | :---: | :---: |
| **`'K'`** | 75 | `0x4B` | `01001011` |
| **`'u'`** | 117 | `0x75` | `01110101` |
| **`'p'`** | 112 | `0x70` | `01110000` |
| **`'e'`** | 101 | `0x65` | `01100101` |
| **`'r'`** | 114 | `0x72` | `01110010` |
| **`'-'`** | 45 | `0x2D` | `00101101` |
| **`'6'`** | 54 | `0x36` | `00110110` |
| **`'7'`** | 55 | `0x37` | `00110111` |

**Datu-kate osoa ($M$):**  
`01001011 01110101 01110000 01100101 01110010 00101101 00110110 00110111`

#### 2. Polinomio Sortzailea (CRC-6-GSM):
Erronkan eskatutako polinomioaren formula:
$$G(x) = x^6 + x^5 + x^3 + x^2 + x + 1$$

Koefizienteak bitarrez adierazita:
$$1\cdot x^6 + 1\cdot x^5 + 0\cdot x^4 + 1\cdot x^3 + 1\cdot x^2 + 1\cdot x^1 + 1\cdot x^0 \implies \mathbf{1101111} \text{ (7 bit)}$$

Polinomioak 6ko gradua duenez ($r = 6$), mezuaren atzean **6 zero** gehitu ditugu zatiketa egin aurretik: `...000000`.

#### 3. Zatiketa Bitarra Koadernoan (Modulo-2 XOR Logika):
Zatiketa eskuz egin dugu koadernoan XOR eragiketaren bidez ($1 \oplus 1 = 0$, $0 \oplus 0 = 0$, $1 \oplus 0 = 1$, $0 \oplus 1 = 1$). Prozesu luzea izan zen kate guztiak lerrokatzen, baina azkenean hondarra lortu genuen:

$$\text{Hondarra (CRC-6-GSM)} = \mathbf{0101001}$$

![Gure pasahitzaren CRC kalkulua koadernoan](img/crc_kalkulua.jpg)

Transmisioan pasahitzaren bit-kateari 6 biteko hondar hau erantsiko zaio. Zerbitzariak jasotzen duenean, datu guztiak $1101111$ polinomioaz zatituko ditu berriro; hondarra `000000` bada, pasahitza inolako interferentziarik gabe iritsi dela bermatzen da.

---

### 3.3. Modulazio Digitalak (ASK, FSK, PSK eta Konbinatua)

Streaming bezalako aplikazioetan kanal analogikoen bidez datu asko azkar transmititzeko modulazio-teknikak behar dira. Hiru oinarrizko modulazio daude:
* **ASK (Anplitudea):** Uhinaren altuera aldatzen da biten arabera.
* **FSK (Maiztasuna):** Uhinaren abiadura/maiztasuna aldatzen da tarte bakoitzean.
* **PSK (Fasea):** Uhinaren abiapuntua aldatzen da ($0^\circ, 90^\circ, 180^\circ, 270^\circ$).

#### Modulazio Konbinatua: `PSK(2) + ASK(2) + FSK(2)`
Klaseko ariketan hiru metodoak bateratu ditugu: sinbolo bakoitzean **6 bit aldi berean** kodetu ditugu (2 bit fasearentzat, 2 anplitudearentzat eta 2 maiztasunarentzat). Horri esker sinbolo bakar batean $2^6 = 64$ egoera posible transmititu daitezke:
* **ASK (2 bit):** 4 tentsio-maila ardatzean (`00`, `01`, `10`, `11`).
* **FSK (2 bit):** 4 maiztasun desberdin ziklo kopuruaren arabera.
* **PSK (2 bit):** 4 fase desberdin uhinaren abiatzean.

Koadernoan eskuz marraztutako uhinak eta sinbolo bitarrak:

![Modulazio grafikoa 1](img/modulazioa_grafikoa_1.jpg)

![Modulazio grafikoa 2 - PSK, ASK, FSK](img/modulazioa_grafikoa_2.jpg)

---

### 3.4. Oinarrizko Bandako Kodifikazioa: Manchester

Kable bidezko Ethernet transmisioan (10BASE-T) erloju-sinkronizazioa ez galtzeko eta korronte zuzenaren (DC) polarizazioa ekiditeko **Manchester kodifikazioa** erabiltzen da:
* Bit bakoitzaren erdian beti dago tentsio-trantsizio bat:
  * **'0' bita:** Goi-mailatik behe-mailara jaisten da ($+V \rightarrow -V$).
  * **'1' bita:** Behe-mailatik goi-mailara igotzen da ($-V \rightarrow +V$).

Horri esker, streaming datuetan jarraian 0 edo 1 asko egon arren, hargailuak erlojua birsinkronizatuta mantentzen du uneoro.

---

## 4. Kableatu Egituratua eta Wi-Fi Integrazioa (IE2)

### 4.1. Kableak Ziztatzea eta Muntaia Fisikoa (TIA/EIA-568B)

Streaming plataformaren zerbitzaria eta gela arteko lotura fisikoa egiteko, UTP Cat6 kablea erabili dugu. Kablearen bi muturrak **TIA/EIA-568B** araua jarraituz konektatu ditugu:
1. Zuri-Laranja | 2. Laranja | 3. Zuri-Berdea | 4. Urdina | 5. Zuri-Urdina | 6. Berdea | 7. Zuri-Marroia | 8. Marroia

#### A) Keystone Konektorea (Erabiltzailearen Mahaiko Roseta):
Mahaian geratzen den muturrean Keystone eme konektore bat instalatu dugu:
* Kablearen kanpoko babes-plastikoa kontu handiz zuritu dugu hari txirikordatuak ez kaltetzeko.
* Bikoteak askatu eta keystone-ko zirrikituetan kokatu ditugu 568B kolore-sekuentzian.
* Inpaktu-tresnarekin (Krone estiloko inserter horiarekin) kableak finkatu eta soberakinak modu garbian moztu ditugu.

![Keystone konektorea ziztatuta](img/keystone_jack.jpg)

#### B) Patch Panela (Rack-eko 18. Ataka):
Kablearen beste muturra komunikazio-armairuko (Rack) Patch Panelera bideratu dugu:
* 18. atakako IDC blokeetan hariak koloreen arabera jarri eta inpaktagailuaz zolatu ditugu kontaktu optimoa lortuz.

![Patch panela 18. atakan](img/patch_panel.jpg)

#### C) LAN Tester Egiaztapena:
Muntaiaren ondoren kablearen bi muturrak probagailura (LAN Tester) lotu genituen. Hasieran 3. pina (zuri-berdea) ez zen pizten haria ez zelako guztiz hondoraino sartu; baina berriro inpaktu-tresnarekin klik egin eta estutu ondoren, 1etik 8rako LED guztiak berdez eta sekuentzia zuzenean piztu ziren.

---

### 4.2. Haririk Gabeko Sarea (Wi-Fi) eta Bezeroen Konexioa

Streaming plataforma moderno batean erabiltzaile gehienak eramangarrietatik edo telefono mugikorretatik konektatzen dira. Erabilera-kasu hori integratzeko:
* Access Point (AP) bat konektatu dugu switch-era kable bidez.
* Wi-Fi sare bat sortu dugu **WPA2-PSK (AES)** segurtasunarekin.
* Mugikorretik Wi-Fi sare honetara konektatu gara, sare lokaleko zerbitzariaren baliabideak atzituz.

---

## 5. Konmutadoreen Administrazioa eta Konektibitate Osoa (IE3)

### 5.1. Topologia eta Sare-Diseinua

Sareak **izar-topologia hedatua** dauka:
* **Switch-a (Konmutadorea):** Azpiegituraren erdigunean kokatuta dago.
* Bertara konektatzen dira:
  1. **Windows Server 2022:** Streaming bideoak eta webgunea (IIS) ostatatzen dituena.
  2. **Patch Panela (18. ataka):** Keystone bidez gure ikasgelako lan-estaziora doan kablea.
  3. **Access Point-a:** Wi-Fi estaldura ematen duena mugikorrentzat.

```text
[ Windows Server 2022 ] ──(kablea)──┐
(Streaming IIS Zerbitzaria)         │
                                    v
[ Gure Lan-Estazioa ] ──[Keystone]──[Patch Panel 18]──> [ SWITCH NAGUSIA ] <──(kablea)── [ Wi-Fi Access Point ]
                                                                                               :
                                                                                            (Wi-Fi)
                                                                                               v
                                                                                      [ Bezero Mugikorra ]
```

---

### 5.2. IP Helbideratze Taula

Nodo guztiek `192.168.1.0/24` azpisare logiko bera partekatzen dute:

| Gailua / Rola | Interfazea | IP Helbidea | Azpisare-Maskara | Atebidea (Gateway) | DNS Zerbitzaria |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Windows Server (IIS Streaming)** | Ethernet (Kableatua) | `192.168.1.10` | `255.255.255.0` | `192.168.1.1` | `127.0.0.1` |
| **PC1 (Gure Lan-Estazioa)** | Ethernet (Keystone) | `192.168.1.20` | `255.255.255.0` | `192.168.1.1` | `192.168.1.10` |
| **Wi-Fi Access Point** | Ethernet (Kudeaketa)| `192.168.1.2` | `255.255.255.0` | `192.168.1.1` | `192.168.1.10` |
| **Bezero Mugikorra** | Wi-Fi (WLAN) | `192.168.1.50` | `255.255.255.0` | `192.168.1.1` | `192.168.1.10` |

---

### 5.3. Konektibitate Egiaztapena (Ping Probak eta Web Sarbidea)

Errubrikako eskakizun gorena betetzeko, sareko nodo guztien arteko `ping` probak burutu ditugu:

#### 1. Lan-Estaziotik Streaming Zerbitzarira (`192.168.1.10`):
```text
C:\Users\Ikaslea> ping 192.168.1.10

Haciendo ping a 192.168.1.10 con 32 bytes de datos:
Respuesta desde 192.168.1.10: bytes=32 tiempo<1m TTL=128
Respuesta desde 192.168.1.10: bytes=32 tiempo<1m TTL=128
Respuesta desde 192.168.1.10: bytes=32 tiempo<1m TTL=128
Respuesta desde 192.168.1.10: bytes=32 tiempo<1m TTL=128

Estadísticas de ping para 192.168.1.10:
    Paquetes: enviados = 4, recibidos = 4, perdidos = 0 (0% perdidos)
```
Erantzuna berehalakoa izan da (<1ms) eta %0 pakete-galerarekin.

#### 2. Zerbitzaritik Lan-Estaziora (`192.168.1.20`):
```text
PS C:\Users\Administrator> ping 192.168.1.20
Respuesta desde 192.168.1.20: bytes=32 tiempo<1m TTL=128
Paquetes: enviados = 4, recibidos = 4, perdidos = 0 (0% perdidos)
```

#### 3. Mugikorretik (Wi-Fi) Zerbitzarira eta Webguneko Proba:
Mugikorretik lehenik ping bidali diogu zerbitzariari (2-3 ms inguruko latentziarekin, wifiaren ezaugarriengatik), eta ondoren nabigatzailean `http://192.168.1.10` helbidera joan gara. Webgunea arin eta akatsik gabe kargatu da haririk gabeko gailutik, zerbitzariaren IIS atzipena frogatuz.

---

## 6. Ondorioak eta Errubrikaren Betetze Maila

Egindako lan guztien bidez, **Sareak Administratzea eta Planifikatzea** moduluaren helburu guztiak maila gorenean bete dira:

| Ikasketa Emaitza (IE) | Errubrikako Irizpidea | Lortutako Maila | Taldearen Ebidentzia Nagusia |
| :--- | :--- | :---: | :--- |
| **IE1 (10%)** | CRC errore-kontrola menperatzea | **Aurreratua (10)** | Zerbitzariko pasahitzaren (`Kuper-67`) kalkulua koadernoan CRC-6-GSM erabiliz (`0101001` hondarra). |
| **IE1 (10%)** | Modulazioak eta Manchester | **Aurreratua (10)** | $PSK(2)+ASK(2)+FSK(2)$ modulazio konbinatuaren grafikoak (6 bit sinboloko) eta Manchester teoria. |
| **IE1 (10%)** | Sare-gailuak OSI ereduetan | **Aurreratua (10)** | 1., 2. eta 3. mailen araberako taula eta gailu bakoitzaren eginkizuna streaming sarean. |
| **IE2 (10%)** | Kableatua eta Wi-Fi integrazioa | **Aurreratua (10)** | Keystone eta Patch Panel 18. ataka T568B bidez zolatuta, tester egiaztapena eta AP Wi-Fi konexioa. |
| **IE3 (10%)** | Switch-a eta konektibitate osoa | **Aurreratua (10)** | Izar-topologia, IP taula eta noranzko guztietako `ping` arrakastatsuak (%0 galera), mugikorra barne. |
