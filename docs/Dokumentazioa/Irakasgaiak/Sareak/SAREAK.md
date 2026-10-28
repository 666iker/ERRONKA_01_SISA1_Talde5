# Sareak: Azpiegitura Fisikoa, Datu-Transmisioa eta Konektibitatea

Atal honetan sarearen arloan egindako lan tekniko guztiak biltzen dira: tailerreko kableatu egituratuaren instalazio fisikoa (Keystone roseta eta Patch Panela), zerbitzariko pasahitzaren transmisio segurua errore-kontrolarekin (**CRC-6-GSM**) eta modulazioekin, eta sareko gailu guztien arteko konexio osoa (Switch-a eta Wi-Fi sarbidea).

---

## 1. Datu-sareen Egitura eta Transmisio Printzipioak (IE1)

### 1.1. Sareko Gailuak eta OSI Eredua (1. eta 2. Mailak)

Sareak latentzia baxua eta komunikazio egonkorra bermatu behar ditu. Gailu bakoitzak funtzio zehatz bat betetzen du OSI ereduaren lehen geruzen arabera:

| Gailua / Osagaia | OSI Geruza | Datu-Unitatea (PDU) | Funtzio Nagusia Gure Sarean |
| :--- | :--- | :--- | :--- |
| **UTP Cat6 Kablea, Keystone eta Patch Panela** | 1. Maila (Fisikoa) | Bita | Seinale elektrikoak bideratzen dituzte lan-estazioetatik rack-era. |
| **Switch-a (Konmutadorea)** | 2. Maila (Lotura) | Trama (*Frame*) | MAC helbideak aztertzen ditu datuak dagokion ekipoaren atakara bakarrik bidaltzeko. |
| **Sare-Txartela (NIC)** | 1. eta 2. Mailak | Bita / Trama | Ekipoari konexio fisikoa eta hardware identifikazio bakarra (MAC) ematen dizkio. |
| **Wi-Fi Access Point-a (AP)** | 1. eta 2. Mailak | Bita / Trama | Uhin elektromagnetikoen eta sare kableatuaren arteko zubi gardena egiten du. |
| **Routerra (Bideratzailea)** | 3. Maila (Sarea) | Paketea (*Packet*) | IP helbideen bidez azpisare desberdinen arteko komunikazioa kudeatzen du. |

---

### 1.2. Pasahitzaren ASCII Kodeketa eta CRC-6-GSM Errore-Detekzioa

Zerbitzarian sartzeko ezarri dugun pasahitz sendoa hartu dugu ariketarako: **`Kuper-67`**. 

Pasahitza kabletik bidaltzean zaratarik edo erroreik egon den kontrolatzeko, **CRC-6-GSM** teknika kalkulatu dugu eskuz:

#### 1. Karaktereak bitetara bihurtzea (ASCII):
ASCII taula erabiliz karaktere bakoitzaren balio bitarra atera dugu (guztira 64 bit):

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

**Datu-sekuentzia osoa ($M$):**  
`01001011 01110101 01110000 01100101 01110010 00101101 00110110 00110111`

#### 2. Polinomio sortzailea (CRC-6-GSM):
Eskatutako polinomioaren formula:
$$G(x) = x^6 + x^5 + x^3 + x^2 + x + 1$$

Bitarrez adierazita:
$$1\cdot x^6 + 1\cdot x^5 + 0\cdot x^4 + 1\cdot x^3 + 1\cdot x^2 + 1\cdot x^1 + 1\cdot x^0 \implies \mathbf{1101111} \text{ (7 bit)}$$

Polinomioak 6ko gradua duenez ($r = 6$), mezuari **6 zero** gehitu dizkiogu bukaeran zatiketarekin hasteko (`000000`).

#### 3. Zatiketa bitarra koadernoan (XOR logika):
Koadernoan zatiketa luzea egin dugu modulo-2 (XOR) bidez: bit berdinak badira 0 jartzen da, eta desberdinak badira 1. Zatiketa osoa amaitzean lortu dugun hondarra **`0101001`** izan da:

![CRC-6-GSM kalkulua koadernoan](img/crc_kalkulua.jpg)

Mezuaren atzean 6 biteko hondar hau erantsita bidaltzen da. Jasotzen duen ekipoak datu guztiak $1101111$ polinomioaz zatituko ditu berriro; hondarra `000000` bada, mezua osorik eta akatsik gabe iritsi dela baieztatzen da.

---

### 1.3. Modulazio Digitalak (ASK, FSK, PSK eta Konbinatua)

Seinale analogikoen bidez bitak transmititzeko hiru modulazio nagusi daude:
* **ASK (Anplitudea):** Seinalearen tentsio/altuera aldatzen da biten arabera.
* **FSK (Maiztasuna):** Seinalearen ziklo-kopurua edo abiadura aldatzen da tarte bakoitzean.
* **PSK (Fasea):** Seinalearen abiapuntua aldatzen da ($0^\circ, 90^\circ, 180^\circ, 270^\circ$).

#### Modulazio Konbinatua: `PSK(2) + ASK(2) + FSK(2)`
Transmisioaren efizientzia handitzeko hiru modulazioak batu ditugu: sinbolo bakoitzean **6 bit aldi berean** kodetu ditugu (2 bit fasearentzat, 2 anplitudearentzat eta 2 maiztasunarentzat). Horri esker, aldaketa bakoitzeko $2^6 = 64$ egoera posible adieraz daitezke:
* **ASK (2 bit):** 4 tentsio-maila ardatzean (`00`, `01`, `10`, `11`).
* **FSK (2 bit):** 4 maiztasun desberdin zikloen arabera.
* **PSK (2 bit):** 4 fase desberdin uhinaren hasieran.

Jarraian koadernoan eskuz marraztutako uhinak eta sinbolo bitarrak ikus daitezke:

![Modulazio grafikoa 1](img/modulazioa_grafikoa_1.jpg)

![Modulazio grafikoa 2 - PSK, ASK, FSK](img/modulazioa_grafikoa_2.jpg)

---

### 1.4. Oinarrizko Bandako Kodifikazioa: Manchester

Kable bidezko Ethernet transmisioan (10BASE-T) erloju-sinkronizazioa ez galtzeko eta tentsio konstantea ez metatzeko **Manchester kodifikazioa** erabiltzen da:
* Bit bakoitzaren erdian beti gertatzen da tentsio-aldaketa bat:
  * **'0' bita:** Goi-mailatik behe-mailara jaisten da ($+V \rightarrow -V$).
  * **'1' bita:** Behe-mailatik goi-mailara igotzen da ($-V \rightarrow +V$).

Modu honetan, jarraian '0' edo '1' asko bidalita ere, hargailuak uneoro daki noiz hasten eta amaitzen den bit bakoitza.

---

## 2. Kableatu Egituratua eta Wi-Fi Integrazioa (IE2)

### 2.1. Kableak Ziztatzea eta Muntaia Fisikoa (TIA/EIA-568B)

Klaseko lan-estazioen eta komunikazio-armairuaren (Rack) arteko konexioa egiteko, UTP Cat6 kablea erabili dugu eta **TIA/EIA-568B** araua jarraituz konektatu dugu:
1. Zuri-Laranja | 2. Laranja | 3. Zuri-Berdea | 4. Urdina | 5. Zuri-Urdina | 6. Berdea | 7. Zuri-Marroia | 8. Marroia

#### A) Keystone Konektorea (Mahaiko Roseta Emea):
Lan-estazioan geratzen den muturrean Keystone eme bat ziztatu dugu:
* Kablearen kanpoko babes-azala kendu dugu barruko hariak ez ebakitzeko kontuz ibiliz.
* Hariak 568B kolore-sekuentziaren arabera kokatu ditugu alboko gidetan.
* Inpaktagailuarekin (Krone inserter horiarekin) hariak beheraino bultzatu ditugu konexio sendoa eginez eta muturretako soberakinak moztuz.

![Keystone konektorea ziztatuta](img/keystone_jack.jpg)

#### B) Patch Panela (Rack-eko 18. Ataka):
Kablearen beste muturra rack-eko Patch Panelera eraman dugu eta 18. atakan finkatu dugu:
* Atzeko IDC zirrikituetan kableak koloreen arabera jarri eta inpaktu-tresnarekin ziztatu ditugu kontaktu garbia lortuz.

![Patch panela 18. atakan](img/patch_panel.jpg)

#### C) LAN Tester Egiaztapena:
Kablea prest zegoenean probagailura (LAN Tester) konektatu dugu. Hasieran 3. pina (zuri-berdea) ez zen pizten haria ez zelako guztiz sartu; baina berriro inpaktagailuarekin bultzatu ondoren, 1etik 8rako argi guztiak ordenan eta berdez piztu ziren.

---

### 2.2. Sare-moldagailuak eta Haririk Gabeko Sarea (Wi-Fi)

* **Sare Kableatua:** Zerbitzariari eta lan-estazioei IP helbide estatikoak konfiguratu dizkiegu IPv4 protokoloaren propietateetan.
* **Wi-Fi Sarea:** Hari gabeko gailuak konektatzeko Access Point (AP) bat lotu dugu switch-era kablez. Wi-Fi sare bat sortu dugu WPA2 enkriptazioarekin eta mugikorretik konektatu gara sarbidea frogatzeko.

---

## 3. Konmutadoreen Administrazioa eta Konektibitate Osoa (IE3)

### 3.1. Topologia eta Sare-Eskema

Sareak **izar-topologia** jarraitzen du: Switch nagusia erdian kokatuta dago eta bertara lotzen dira elementu guztiak kable bidez:

```text
[ Windows Server ] ──(kablea)──┐
                               │
                               v
[ Lan-Estazioa ] ──[Keystone]──[Patch Panel 18]──> [ SWITCH NAGUSIA ] <──(kablea)── [ Wi-Fi Access Point ]
                                                                                         :
                                                                                      (Wi-Fi)
                                                                                         v
                                                                                [ Bezero Mugikorra ]
```

---

### 3.2. IP Helbideratze Taula

Gailu guztiak `192.168.1.0/24` azpisare berean daude konfiguratuta:

| Gailua / Rola | Konexio Mota | IP Helbidea | Azpisare-Maskara | Atebidea (Gateway) | DNS Zerbitzaria |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Windows Server** | Kablez switch-era | `192.168.1.10` | `255.255.255.0` | `192.168.1.1` | `127.0.0.1` |
| **PC1 (Lan-Estazioa)** | Keystone bidez | `192.168.1.20` | `255.255.255.0` | `192.168.1.1` | `192.168.1.10` |
| **Wi-Fi Access Point** | Kablez switch-era | `192.168.1.2` | `255.255.255.0` | `192.168.1.1` | `192.168.1.10` |
| **Bezero Mugikorra** | Wi-Fi bidez | `192.168.1.50` | `255.255.255.0` | `192.168.1.1` | `192.168.1.10` |

---

### 3.3. Konektibitate Egiaztapena (Ping Probak)

Errubrikaren arabera, edozein gailutatik edozein gailura `ping` egitean erantzuna lortu behar da:

#### 1. Lan-Estaziotik Zerbitzarira (`192.168.1.10`):
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

#### 3. Mugikorretik (Wi-Fitik) Zerbitzarira:
Mugikorra Wi-Fira konektatuta zegoela ping egin zaio zerbitzariaren IPra (`192.168.1.10`): 4 paketeak ondo iritsi dira (%0 galera) eta latentzia 2-3 ms ingurukoa izan da.

---

## 4. Laburpena eta Errubrikaren Betetze Maila

Sareen atalean egindako lanekin errubrikako irizpide tekniko guztiak maila gorenean bete dira:

| Ikasketa Emaitza (IE) | Errubrikako Irizpidea | Lortutako Maila | Taldearen Ebidentzia Nagusia |
| :--- | :--- | :---: | :--- |
| **IE1 (10%)** | CRC errore-kontrola menperatzea | **Aurreratua (10)** | `Kuper-67` pasahitzaren CRC-6-GSM zatiketa eskuz koadernoan ebatzia (`0101001` hondarra). |
| **IE1 (10%)** | Modulazioak eta Manchester | **Aurreratua (10)** | $PSK(2)+ASK(2)+FSK(2)$ modulazio konbinatuaren grafikoak (6 bit sinboloko) eta Manchester seinalea. |
| **IE1 (10%)** | Sare-gailuak OSI ereduetan | **Aurreratua (10)** | 1., 2. eta 3. mailen araberako taula eta gailu bakoitzaren zeregina. |
| **IE2 (10%)** | Kableatua eta Wi-Fi integrazioa | **Aurreratua (10)** | Keystone eta Patch Panel 18. ataka T568B bidez zolatuta, tester egiaztapena eta Wi-Fi AP konexioa. |
| **IE3 (10%)** | Switch-a eta konektibitate osoa | **Aurreratua (10)** | Izar-topologia, IP taula eta noranzko guztietako `ping` arrakastatsuak (%0 galera). |
