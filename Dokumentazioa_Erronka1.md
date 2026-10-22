# 1. Erronka — Dokumentazio Teknikoa (SISA1 - 5. Taldea)

**Zentroa:** CIFP Uni Eibar-Ermua LHII  
**Zikloa:** SISA1 (2026-2027)  
**Egileak (5. Taldea):** Axular Muñoz, Iker Monkayola, Alex Sierra, Jokin Martinez  

---

## Aurkibidea

1. [Proiektuaren Sarrera eta Web Sarbidea](#1-proiektuaren-sarrera-eta-web-sarbidea)
2. [Sareak: OSI 1. eta 2. Mailak, CRC-6-GSM eta Modulazioak](#2-sareak-osi-1-eta-2-mailak-crc-6-gsm-eta-modulazioak)
3. [Digitalizazioa: IT/OT, THD eta Cloud Kontzeptuak](#3-digitalizazioa-itot-thd-eta-cloud-kontzeptuak)
4. [Datu-Basea: ER Diagrama, Eredu Erlazionala eta Ofimatika](#4-datu-basea-er-diagrama-eredu-erlazionala-eta-ofimatika)
5. [Windows Server Azpiegitura, Planifikazioa eta Taldea](#5-windows-server-azpiegitura-planifikazioa-eta-taldea)

---

## 1. Proiektuaren Sarrera eta Web Sarbidea

Webgune estatiko honen bitartez, **SISA1 zikloko 5. Taldeak** 1. Erronkan garatutako dokumentazio teknikoa eta emaitzak publikatu ditu. Proiektuan honako lau arlo nagusi hauek landu dira:

- **Sareak (OSI 1-2):** Kableatua Patch Panelera eta Switchera, pasahitza ASCIIra, CRC-6-GSM, Manchester eta ASK/FSK/PSK marrazkiak eta Wi-Fi proba.
- **Digitalizazioa:** IT eta OT sistemen arteko desberdintasunak eta konbergentzia, THD teknologiak eta Cloud Computing ereduak.
- **Datu-Basea:** Streaming plataforma baten beharretarako ER diagrama, eredu erlazional normalizatua (taulak) eta ofimatika inplementazioa.
- **Windows Server:** IIS web-zerbitzaria, RAID bolumenen konfigurazioa, segurtasun politikak eta planifikazioaren jarraipena.

### Saio-Hasiera (`login.html`)

Webgunera sartzeko saio-hasiera orri bat prestatu da:

- **Erabiltzailea:** `5Taldea`
- **Pasahitza:** `Admin123`

---

## 2. Sareak: OSI 1. eta 2. Mailak, CRC-6-GSM eta Modulazioak

Atal honetan 1. Erronkako Windows Server zerbitzaria eta klaseko ekipoak sarera konektatzeko erabili den azpiegitura fisikoa (OSI 1. maila) eta datu-lotura maila (OSI 2. maila) azaltzen dira.

### 2.1. OSI Ereduaren Lehen Bi Mailak: Patch Panel eta Switch Lotura

Klasean ikasleok egindako UTP Cat6 kableekin (T568B estandarra jarraituz) ekipoak **Patch Panelera** konektatu dira, eta bertatik latiguillo bidez **Switchera**:

- **1. Maila — Maila Fisikoa (Physical Layer):** Bitak seinale elektriko edo elektromagnetiko bihurtzeaz arduratzen da. Proiektuan ikasleok grimpatutako RJ-45 konektoreak, UTP kableak, **Patch Panela**, Manchester kodifikazioa eta ASK/FSK/PSK modulazioak maila honetan kokatzen dira.
- **2. Maila — Datu-Lotura Maila (Data Link Layer):** Bitak trametan (frames) antolatzen ditu, gailuen **MAC helbideak** kudeatzen ditu **Switch**-aren bidez eta transmisio-erroreak detektatzen ditu **CRC (Cyclic Redundancy Check)** algoritmoa erabiliz.

### 2.2. Pasahitza ASCIIra Bihurtzea eta CRC-6-GSM Gehitzea

Sistemara sartzeko erabilitako pasahitzetako bat **`Kuper-67`** da. Karaktere bakoitzaren ASCII bihurketa:

| Karakterea | ASCII Hamartarra | ASCII Hamaseitarra (Hex) | ASCII Bitarra (8 bit) |
| :--- | :--- | :--- | :--- |
| **`'K'`** *(Kalkulurako aukeratua)* | 75 | `0x4B` | **`01001011`** |
| **`'u'`** | 117 | `0x75` | `01110101` |
| **`'p'`** | 112 | `0x70` | `01110000` |
| **`'e'`** | 101 | `0x65` | `01100101` |
| **`'r'`** | 114 | `0x72` | `01110010` |
| **`'-'`** | 45 | `0x2D` | `00101101` |
| **`'6'`** | 54 | `0x36` | `00110110` |
| **`'7'`** | 55 | `0x37` | `00110111` |

#### CRC-6-GSM Kalkulua Pausoz Pauso (`'K'` = `01001011`)

- **Polinomio sortzailea (CRC-6-GSM):** $x^6 + x^5 + x^3 + x^2 + x + 1$
- **Polinomio sortzailea bitarrez $G(x)$:** $1\cdot x^6 + 1\cdot x^5 + 0\cdot x^4 + 1\cdot x^3 + 1\cdot x^2 + 1\cdot x^1 + 1\cdot x^0 =$ **`1101111`** (7 bit, 6. mailakoa).
- **Mezua $M(x)$:** `01001011` (`'K'` letra).
- **6 zero gehituta ($2^6 \cdot M$):** `01001011 000000`.

```text
CRC-6-GSM ZATIKETA BITARRA (XOR eragiketa):
Mezua + 6 zero:  01001011000000  | Zatitzailea G(x): 1101111
                  1101111        |--------------------------
                  -------        |
                   001001000000  | 1. XOR urratsa
                     1101111     |
                     -------     |
                     0100111000  | 2. XOR urratsa
                      1101111    |
                      -------    |
                      010000100  | 3. XOR urratsa
                       1101111   |
                       -------   |
                       01011010  | 4. XOR urratsa
                        1101111  |
                        -------  |
                        0110101  | 5. XOR urratsa
                         1101111 |
                         ------- |
                         0001010 --> HONDARRA (CRC-6-GSM, 6 bit) = 001010

BIDALIKO DEN TRAMA OSOA (Mezua + CRC-6):
[ 01001011 ] + [ 001010 ] = 01001011001010 (14 bit)
```

### 2.3. Manchester Kodifikazioa eta Modulazioen Marrazkiak (ASK, FSK, PSK)

Lortutako 14 biteko trama (**`01001011 001010`**) kabletik eta uhinetatik bidaltzeko kodifikazio eta modulazio grafikoa:

![Manchester kodifikazioa eta ASK, FSK, PSK modulazioen marrazkia](img/modulazioa-manchester.svg)

| Teknika Mota | Izena | Funtzionamendua Marrazkian |
| :--- | :--- | :--- |
| Kodifikazio Digitala | **Manchester (IEEE 802.3)** | Bit bakoitzaren erdian trantsizio bat dago: **0** = Goitik-Behera ($\downarrow$), **1** = Behetik-Gora ($\uparrow$). Erlojuaren sinkronizazioa bermatzen du Ethernet kableetan. |
| Modulazio Analogikoa | **ASK (Amplitude Shift Keying)** | Uhin garraiatzailearen **anplitudea** aldatzen da: **1** denean uhina dago, eta **0** denean anplitudea zero da (lerro laua). |
| Modulazio Analogikoa | **FSK (Frequency Shift Keying)** | Uhinaren **maiztasuna (frekuentzia)** aldatzen da: **0** bitak maiztasun baxua du (ziklo gutxi) eta **1** bitak maiztasun altua (ziklo bikoitza). |
| Modulazio Analogikoa | **PSK (Phase Shift Keying)** | Uhin garraiatzailearen **fasea** 180 gradu aldatzen da bit-aldaketa dagoenean (**1** fase positiboan hasten da eta **0** alderantzizko fasean). |

### 2.4. Hari Gabeko Gailu Batetik Testeatzea (Mugikorra eta Eramangarria)

1. **Windows Server IIS konfigurazioa:** Webgune estatikoa zerbitzari lokalean jarri da martxan (`192.168.1.10:80`).
2. **Hari gabeko konexioa (Wi-Fi):** Telefono mugikorra eta ordenagailu eramangarria klaseko Wi-Fi sarera konektatu dira (sare bereko IP helbidea jasoz, adib. `192.168.1.55`).
3. **Konektagarritasun proba:** `ping 192.168.1.10` komandoarekin pakete-galerarik ez dagoela egiaztatu da.
4. **Nabigatzailean bistaratzea:** Mugikorreko eta eramangarriko nabigatzaileetan orri guztiak, taulak eta modulazio-marrazkiak ondo ikusten direla egiaztatu da (CSS `@media` erantzunkorrari esker).

---

## 3. Digitalizazioa: IT/OT, THD eta Cloud Kontzeptuak

Eraldaketa digitala osatzen duten hiru kontzeptu nagusiak aztertu eta dokumentatu dira: **IT/OT inguruneak**, **THD (Teknologia Gaitu Digitalak)** eta **Cloud Computing (Hodeia)**.

### 3.1. IT (Information Technology) eta OT (Operational Technology)

- **IT (Informazioaren Teknologiak):** Datuak sortu, prozesatu, gorde eta babesteko erabiltzen diren software eta hardware sistemak dira. Proiektuan: Windows Server web-zerbitzaria (IIS), erabiltzaileen eta rolen datu-base erlazionala, sareko protokoloak eta segurtasun-politikak.
- **OT (Eragiketa Teknologiak):** Ekipamendu fisikoak, prozesu industrialak eta azpiegiturak monitorizatu eta kontrolatzeko hardware eta softwarea da. Proiektuan: Zerbitzari-gelako (CPD) hozte-sistemak, tenperatura eta hezetasun sentsoreak, SAI/UPS energia-sistemak eta rack-eko sarbide fisikoak.

| Ezaugarria | IT (Information Technology) | OT (Operational Technology) |
| :--- | :--- | :--- |
| **Helburu Nagusia** | Datuen kudeaketa, informazioa eta aplikazioen logika | Ekipamendu fisikoen kontrola eta jarraitutasuna 24/7 |
| **Lehentasuna** | Konfidentzialtasuna, Osotasuna eta Erabilgarritasuna (CIA) | Erabilgarritasuna, Denbora Erreala eta Segurtasun Fisikoa |
| **Adibidea Proiektuan** | Windows Server IIS webgunea, RAID bolumenak eta datu-basea | Zerbitzari-gelako klimatizazioa, UPS/SAI eta energia-sentsoreak |

### 3.2. THD — Teknologia Gaitu Digitalak

1. **Adimen Artifiziala (AI) eta Machine Learning:** Datu-basean erabiltzaileak saioa hastean aukeratzen duen rolaren arabera (*Infantila, Pertsonala edo Familiarra*) eta bere bistaratze-erregistroaren arabera, eduki egokienak gomendatzeko algoritmoak.
2. **Big Data (Datu Masiboen Analisia):** Milaka bistaratze-erregistro, ikusitako minutuak eta ordutegiak aztertzen ditu, zerbitzarien karga aurreikusteko eta estatistikak ateratzeko.
3. **Zibersegurtasuna:** Windows Server-eko segurtasun-politikak, pasahitzen babesa, CRC bidezko errore-detekzioa eta baimenen kontrola bermatzeko.
4. **IoT (Gauzen Interneta) eta Hari Gabeko Sareak:** Sareko edozein gailu konektatutatik (mugikorra, eramangarria, Smart TV) edukia atzitzeko eta CPDko sentsoreak urrunetik monitorizatzeko.

### 3.3. Cloud Computing (Hodeiko Konputazioa)

#### Zerbitzu Ereduak (IaaS, PaaS eta SaaS)

| Cloud Eredua | Esanahia | Aplikazioa Proiektuko Arkitekturan |
| :--- | :--- | :--- |
| **IaaS** | Infrastructure as a Service (Azpiegitura zerbitzu gisa) | Zerbitzari birtualak, RAID biltegiratze bolumenak eta sare-azpiegitura (adib. Windows Server makina birtuala eta sareko diskoak). |
| **PaaS** | Platform as a Service (Plataforma zerbitzu gisa) | Datu-base erlazionalak eta web-garapeneko inguruneak kudeatzeko plataforma, sistema eragilearen mantentzeaz arduratu gabe. |
| **SaaS** | Software as a Service (Softwarea zerbitzu gisa) | Erabiltzaileak nabigatzailetik zuzenean erabiltzen duen aplikazioa (saioa hasi, rola aukeratu eta edukia bistaratu ezer instalatu gabe). |

#### Hedapen Ereduak

- **Cloud Pribatua (On-Premise / Zerbitzari Lokala):** Klasean konfiguratutako Windows Server zerbitzaria (IIS eta RAID bolumenekin), non erabiltzaileen datuak eta datu-basea modu pribatuan eta segurtasun-politika propioekin gordetzen diren.
- **Cloud Publikoa:** Hirugarren hornitzaile batek (AWS, Azure, Google Cloud) Internet bidez eskainitako baliabide partekatuak, eskaera handiko uneetan edukia banatzeko.
- **Cloud Hibridoa:** Zerbitzari pribatu lokala (datu sentikorretarako) eta hodei publikoa (edukien banaketarako) uztartzen dituen eredua.

---

## 4. Datu-Basea: ER Diagrama, Eredu Erlazionala eta Ofimatika

1. Erronkako jarraibideen arabera, diseinatu beharreko datu-baseak **streaming eduki plataforma baten beharrak** asetu behar ditu:

- **Erabiltzaileak eta Rolak:** Erabiltzaile batek hainbat rol izan ditzake bere kontuan: *Infantila*, *Pertsonala*, *Familiarra*...
- **Saio-hasiera eta Bistaratzea rolaren bidez:** Erabiltzaile batek saioa hasten duenean bere **rola** aukeratzen du, eta **rol horrekin egiten du edukiaren bistaratzea**. Horregatik, `BISTARATZEAK` taula zuzenean lotuta dago `ERABILTZAILE_ROLAK` taularekin (`id_profila` gako arrotzaren bidez).
- **Planak eta Suskripzioak:** Erabiltzaileek harpidetza-plan bat kontratatzen dute (*Oinarrizkoa, Estandarra, Premium*) suskripzio baten bidez.
- **Edukiak:** Katalogoak *Pelikulak*, *Serieak* eta *Dokumentalak* biltzen ditu.
- **Ofimatika Suitean Inplementatua:** Diseinatutako eredu erlazionala ofimatika suite batean (Microsoft Access / LibreOffice Base) inplementatu da.

### 4.1. Entitate-Erlazio Diagrama (ER) eta Eskema Erlazionala (3FN)

```text
EREDU ERLAZIONAL NORMALIZATUA (TAULEN ESKEMA):
1. PLANAK (id_plana [PK], plan_izena, hileko_prezioa, bideo_kalitatea, max_pantailak)
2. ERABILTZAILEAK (id_erabiltzailea [PK], izena, abizenak, emaila, pasahitza)
3. SUSKRIPZIOAK (id_suskripzioa [PK], id_erabiltzailea [FK], id_plana [FK], hasiera_data, amaiera_data, egoera)
4. ROLAK (id_rola [PK], rol_mota, iragazki_maila, deskribapena)
5. ERABILTZAILE_ROLAK (id_profila [PK], id_erabiltzailea [FK], id_rola [FK], profil_izena, pin_kodea)
6. EDUKIAK (id_edukia [PK], izenburua, mota, generoa, iraupena_min, sailkapena)
7. BISTARATZEAK (id_bistaratzea [PK], id_profila [FK], id_edukia [FK], bistaratze_data, ikusitako_minutuak, amaituta)
```

### 4.2. Eredu Erlazionaleko Taulak (Normalizatuak)

#### 1. `ERABILTZAILEAK` Taula

| id_erabiltzailea (PK) | izena | abizenak | emaila | pasahitza |
| :--- | :--- | :--- | :--- | :--- |
| **U01** | Axular | Muñoz | `munoz.axular@uni.eus` | `esneider2` |
| **U02** | Iker | Monkayola | `monkayola.iker@uni.eus` | `IkerQuesito10` |
| **U03** | Alex | Sierra | `sierra.alex@uni.eus` | `AlexMesen67` |
| **U04** | Jokin | Martinez | `martinez.jokin@uni.ikasleak.eus` | `Jokinki67` |

#### 2. `ROLAK` Taula

| id_rola (PK) | rol_mota | katalogo_iragazkia | deskribapena |
| :--- | :--- | :--- | :--- |
| **R01** | Infantila | Oinarrizko katalogoa (TP) | Eduki egokituak eta babestuak |
| **R02** | Familiarra | Familia katalogoa | Taldean edo familian ikusteko profila |
| **R03** | Pertsonala | Katalogo osoa | Erabiltzaile bakoitzaren profil pertsonala |

#### 3. `ERABILTZAILE_ROLAK` Taula (Profilak)

| id_profila (PK) | id_erabiltzailea (FK) | id_rola (FK) | profil_izena | pin_kodea |
| :--- | :--- | :--- | :--- | :--- |
| **PR01** | U01 (Axular) | R03 (Pertsonala) | Axular_Pertsonala | `1234` |
| **PR02** | U01 (Axular) | R01 (Infantila) | Axular_Infantila | `0000` |
| **PR03** | U01 (Axular) | R02 (Familiarra) | Etxekoak_Familia | `1111` |
| **PR04** | U02 (Iker) | R03 (Pertsonala) | Iker_Profila | `4321` |
| **PR05** | U03 (Alex) | R03 (Pertsonala) | Alex_Pertsonala | `2468` |
| **PR06** | U04 (Jokin) | R02 (Familiarra) | Jokin_Familia | `1357` |

#### 4. `PLANAK` Taula

| id_plana (PK) | plan_izena | hileko_prezioa | bideo_kalitatea | max_pantailak |
| :--- | :--- | :--- | :--- | :--- |
| **PL01** | Oinarrizkoa | 5,49 € | HD (720p) | 1 |
| **PL02** | Estandarra | 9,99 € | Full HD (1080p) | 2 |
| **PL03** | Premium 4K | 14,99 € | Ultra HD (4K) | 4 |

#### 5. `SUSKRIPZIOAK` Taula

| id_suskripzioa (PK) | id_erabiltzailea (FK) | id_plana (FK) | hasiera_data | amaiera_data | egoera |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **S001** | U01 (Axular) | PL03 (Premium 4K) | 2026-09-01 | 2027-09-01 | Aktibo |
| **S002** | U02 (Iker) | PL02 (Estandarra) | 2026-10-01 | 2026-11-01 | Aktibo |
| **S003** | U03 (Alex) | PL03 (Premium 4K) | 2026-09-15 | 2027-09-15 | Aktibo |
| **S004** | U04 (Jokin) | PL01 (Oinarrizkoa) | 2026-08-15 | 2026-09-15 | Iraungita |

#### 6. `EDUKIAK` Taula

| id_edukia (PK) | izenburua | mota | generoa | iraupena_min | sailkapena |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **E01** | Irati | Pelikula | Fantasia / Historia | 114 min | Pertsonala |
| **E02** | Txanpon Etxea | Seriea | Intriga | 50 min / atala | Pertsonala |
| **E03** | Go!azen 11.0 | Seriea | Musika / Abentura | 45 min / atala | Infantila |
| **E04** | Teknologiaren Ispilua | Dokumentala | Zientzia eta Sareak | 75 min | Familiarra |

#### 7. `BISTARATZEAK` Taula (Aukeratutako Rolarekin egindako Bisualizazioak)

| id_bistaratzea (PK) | id_profila (Aukeratutako Rola) (FK) | id_edukia (FK) | bistaratze_data | ikusitako_minutuak | amaituta |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **B001** | PR01 (Axular - **Rol Pertsonala**) | E01 (Irati - Pelikula) | 2026-10-18 21:30 | 114 min | Bai |
| **B002** | PR02 (Axular - **Rol Infantila**) | E03 (Go!azen 11.0 - Seriea) | 2026-10-19 17:15 | 45 min | Bai |
| **B003** | PR04 (Iker - **Rol Pertsonala**) | E02 (Txanpon Etxea - Seriea) | 2026-10-20 22:00 | 50 min | Bai |
| **B004** | PR05 (Alex - **Rol Pertsonala**) | E01 (Irati - Pelikula) | 2026-10-21 16:20 | 90 min | Ez |
| **B005** | PR06 (Jokin - **Rol Familiarra**) | E04 (Teknologiaren Ispilua - Dokumentala) | 2026-10-21 20:00 | 75 min | Bai |

### 4.3. Ofimatika Suitean Inplementazioa (Access)

1. **Datu-base fitxategia sortu:** LibreOffice Base (`Erronka1_Streaming_DB.odb`) edo Microsoft Access (`Erronka1_Streaming_DB.accdb`) ireki eta datu-base berria sortu da.
2. **7 Taulak diseinu-ikuspegian sortu:** Eremu bakoitzari bere datu-mota egokia (`VARCHAR`, `INTEGER`, `DECIMAL`, `DATE`, `BOOLEAN`) eta gako nagusia (`PK`) esleitu zaizkio.
3. **Erlazioak (Relationships) finkatu:** *Tresnak -> Erlazioak* leihoan gako arrotzak (`FK`) lotu dira osotasun erreferentziala (*Integridad Referencial*) aktibatuz.
4. **Formularioak eta Kontsultak:** Erabiltzaileek rola aukeratu eta bistaratzeak erregistratzeko inprimakia eta SQL kontsultak prestatu dira.

---

## 5. Windows Server Azpiegitura, Planifikazioa eta Taldea

### 5.1. Windows Server Azpiegitura: IIS, RAID Bolumenak eta Segurtasun Politika

| Azpiegitura Osagaia | Konfigurazio Teknikoa Windows Server-en | Helburua Proiektuan |
| :--- | :--- | :--- |
| **Web Zerbitzaria (IIS)** | Rol: `Web Server (IIS)` \| Direktorioa: `E:\Webgunea\Erronka1` \| Ataka: `TCP 80 (HTTP)` | Klaseko webgune estatikoa sare lokaleko kable eta Wi-Fi gailuetara zerbitzatzea. |
| **RAID-1 Bolumena (Ispilua)** | 2 disko dinamiko (*Mirrored Volume*) — Drive letra: `E:` (NTFS) | Webgunea eta datu-basea bikoiztuta gordetzea; disko bat hondatuz gero daturik ez galtzeko. |
| **RAID-5 Bolumena** | Parekidetasun bidezko bolumena — Drive letra: `F:` (NTFS) | Segurtasun-kopiak (*Backup*) eta erregistroak gordetzeko. |
| **Segurtasun Politika (Pasahitzak)** | Gutxienez 8 karaktere, konplexutasuna aktibatuta, blokeoa 3 saiakera okerren ondoren (15 min) | Baimenik gabeko sarbideak eta indarrezko erasoak (*brute-force*) saihestea. |
| **NTFS Baimenak eta Firewall** | Administratzaileak: *Kontrol Osoa* \| `IIS_IUSRS`: *Irakurketa soilik* \| Firewall: 80 ataka irekita | Erabiltzaileek webgunea ikusi ahal izatea baina fitxategiak aldatzeko aukerarik gabe. |

### 5.2. Planifikazioaren Digitalizazioa eta Taldekideen Ardurak

Paperean jasotako hasierako plangintza digitalizatu ondoren, hona hemen 5. Taldeko kide bakoitzaren arloa eta egindako lanaren jarraipena:

| Arlo Teknikoa | Arduraduna | Burututako Zereginak |
| :--- | :--- | :--- |
| **Sareak (OSI 1-2, CRC-6, Modulazioa)** | Axular Muñoz | Kableak (Patch Panel -> Switch), ASCII, CRC-6-GSM, Manchester/ASK/FSK/PSK eta Wi-Fi proba |
| **Datu-Basea (ER, 3FN, Ofimatika)** | Jokin Martinez | ER diagrama, 7 taula normalizatuak (rolak, bistaratzeak...) eta Access |
| **Digitalizazioa eta Windows Server** | Iker Monkayola, Alex Sierra | IT/OT, THD, Cloud, Windows Server IIS, RAID bolumenak eta Segurtasun Politika |
