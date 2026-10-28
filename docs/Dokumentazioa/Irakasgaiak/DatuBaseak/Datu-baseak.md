# Datu-Basearen Diseinuaren Memoria: Streaming Plataforma (Netflix Estiloa)

---

## 1. Sarrera eta Helburua

Dokumentu honen helburua Netflix estiloko streaming plataforma baten datu-basearen diseinua azaltzea da. Prozesua beharren analisitik hasi eta eredu kontzeptual nahiz erlazional normalizatura arte dokumentatzen da, azkenik Microsoft Access bidezko inplementazioarekin amaituz.
### 1.1. DBKS Erabiltzearen Abantailak eta Datu-Kudeaketa Desegokiaren Arazoak

Datu-baseen Kudeaketa Sistema (DBKS) bat erabiltzeak abantaila nagusi hauek eskaintzen ditu fitxategi-sistema tradizional baten (adibidez, Excel edo testu-fitxategiak) aurrean:

* **Erredundantzia eta Inkonsekuentzia Ekiditea:** Datuak behin bakarrik gordetzen dira eredu normalizatuan. Horrela, informazio bikoiztua eta datu kontrajarriak izatea saiesten da.
* **Erreferentzia-Osotasuna eta Segurtasuna:** DBKS-ak bermatzen du existitzen ez den entitate bati ezingo zaiola lotutako erregistrorik esleitu (adibidez, existitzen ez den erabiltzaile bati profil bat sortzea).
* **Datu-Sarbide Simultaneoa (Korkurrentzia):** Erabiltzaile askok batera atzitu dezakete datu-basea datuen osotasuna komprometitu gabe.

**Datuak modu ez-egokian gordetzearen arazoak (Anomaliak):**
* **Sarrera-anomaliak:** Ezin izatea plan berri bat erregistratu oraindik erabiltzailerik esleituta ez badago.
* **Ezabatze-anomaliak:** Profil bat ezabatzean, erabiltzailearen informazio orokor guztia ustekabean galtzea.
* **Eguneratze-anomaliak:** Erabiltzaile baten posta elektronikoa aldatzean, leku guztietan ez aldatzea eta datu kontrajarriak izatea.
---

## 2. Jarraitutako Urratsak (Diseinu Metodologia)

Diseinua lau urrats nagusitan banatu da:

1. **Beharren Analisia:** Plataformaren ezaugarriak eta erabilera-kasuak zehaztu (erabiltzaileak, planak, profilak, gailuak, edukien katalogoa eta ikustaldiak).
2. **Eredu Kontzeptuala (E/R Diagrama):** Entitateak, atributuak eta haien arteko erlazioak marraztu.
3. **Eredu Erlazionalera Pasatzea:** Diagramako osagaiak taula fisiko bihurtzeko arauak aplikatu.
4. **Normalizazioa:** Datuen erredundantzia eta anomaliak ekiditeko 1FN, 2FN eta 3FN arauak egiaztatu.

---

## 3. Eredu Kontzeptuala (Entitate-Erlazio Diagrama)

Hona hemen plataformaren egitura kontzeptuala erakusten duen diagramaren irudia:

![Er diagrama](/docs/Dokumentazioa/img/diagrama.jpg)


### 3.1. Diseinu Erabaki Nagusiak eta Haien Azalpena

* **`SUSKRIBZIOA` Erronbo Moduan:** 
  `SUSKRIBZIOA` erlazio moduan jarri da erabiltzailearen eta planaren artean (2 plan mota ezberdin: Estandar eta Premium). Ez da berezko entitate independente bat; erabiltzaile batek plan bat kontratatzean sortzen den lotura da. `hasiera_data` eta `amaiera_data` atributuak erlazio honen berezko datuak dira.

* **Herentzia Hierarkia (`ISA`) `EDUKIAK` Entitatean:** 
  `EDUKIAK` entitate orokorretik (supermota) hiru azpimota sortu dira: `PELIKULAK`, `SERIEAK` eta `DOKUMENTALAK`. Datu arruntai (`titulua`, `estreinatze_data`, `adina_gutxienekoa`) behin bakarrik gordetzeko `EDUKIAK` taulan, eta balio nulurik (NULL) ez izateko.

* **`EPISODIOAK` Entitate Ahula:** 
  `EPISODIOAK` entitate ahul gisa definitu da `Denboraldiak` entitatearekiko, kapitulu bat ezin delako bere kabuz existitu zein denboralditakoa den jakin gabe. Bere gako nagusia konposatua da (`denboraldi_id` + `episodio_zenbakia`).

* **Generoak `EDUKIAK` Entitateari Lotzea:** 
  `GENEROAK` erlazioa `EDUKIAK` kutxa nagusira konektatu da. Honela, pelikulez gain, serieek eta dokumentalek ere generoa izatea lortzen da lerro bakar batekin, herentzia baliatuz.

* **Generoen Errekurtsibitatea (`SUBGENEROA`):** 
  `GENEROAK` entitateari bucle bat (erlazio errekurtsiboa) jarri zaio, genero nagusien eta subgeneroen arteko hierarkiak sortzeko (adibidez: *Akzioa* $\rightarrow$ *Arte Marzialak*).

* **`BISTARATZEA` Erlazio Hirukoitza (Ternaria):** 
  Rombo bakar batek hiru entitate lotzen ditu: `PROFILA` barnean mota dago, hori rola izengo litzateke, hau da, Familiarra, infantila... `GAILUAK` eta `EDUKIAK`. Ikustaldi bat erregistratzeko hiru faktoreak batera jakin behar dira: **Nork** ikusi duen (Profila), **Zer** ikusi duen (Edukia) eta **Nondik** ikusi duen (Gailua).

---

### 3.2. Kardinalitateen Notazioa

Diagraman notazio bikoitz akademikoa erabili da:
* **Lerroetan (entitate bakoitzaren ondoan):** Parte-hartze mugak **`(gutxienekoa, gehienezkoa)`** formatuan.
* **Erronboen gainean:** Bi aldeen arteko gehienezko erlazio globala (**`1:N`**, **`N:M`**, **`N:M:P`**).

| Erlazioa | Entitatea A | Kardinalitatea A | Entitatea B | Kardinalitatea B | Erlazio Globala |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **`SUSKRIBZIOA`** | `Erabiltzailea` | `(1,1)` | `Planak` | `(0,N)` | **`1:N`** |
| **`DITU` (Profilak)** | `Erabiltzailea` | `(1,1)` | `Profila` | `(1,N)` | **`1:N`** |
| **`DITU` (Generoak)** | `Edukiak` | `(0,M)` | `Generoak` | `(1,N)` | **`N:M`** |
| **`SUBGENEROA`** | `Generoak` (Aita) | `(0,1)` | `Generoak` (Semea) | `(0,N)` | **`1:N`** |
| **`DITU` (Denboraldiak)**| `Serieak` | `(1,1)` | `Denboraldiak` | `(1,N)` | **`1:N`** |
| **`DITU` (Episodioak)** | `Denboraldiak` | `(1,1)` | `Episodioak` | `(1,N)` | **`1:N`** |

### 3.3. Baztertutako Alternatibak eta Diseinuaren Justifikazioa

* **Herentzia (`ISA`) vs Taula Bakarra:** `EDUKIAK` entitatean herentzia egitura erabili da. Alternatiba `EDUKIAK` taula bakarrean film, serie eta dokumentalen atributu guztiak sartzea zen, baina horrek balio nuluz (`NULL`) betetako zutabe asko sortuko zituzke (adibidez, dokumental batean denboraldi kopurua NULO izatea).
* **Erlazio Hirukoitza (Ternaria) vs Binarioak:** `BISTARATZEA` rombo bakarrarekin definitu da. Alternatiba binaka lotzea zen, baina hiru entitateak (`Profila`, `GAILUAK`, `EDUKIAK`) une berean lotzean bakarrik identifikatzen da ikustaldi gertaera zehatz bat.
---

## 4. Eredu Erlazionala

Eredu kontzeptuala taula fisikoetara pasatu ondoren, lortutako eredu erlazionalaren eskema:
# ERLAZIONALA

* **Plana** (**`PlanID`**, `izena`, `prezioa`, **`erabiltzaileID`**, `Hasiera data`, `amaiera data`)
* **Erabiltzaile** (**`Erabiltzaile ID`**, `sortze_data`, `pasahitza`, `Posta`)
* **Profila** (**`ProfilID`**, `Izena`, `mota`, `erabiltzaile ID`)
* **Gailuak** (**`GailuID`**, `mota`, `izena`)
* **Edukiak** (**`Edukia ID`**, `mota`, `adina_gutxienekoa`, `titulua`, `estrenatze data`)
* **Bistaratzea** (**`Ikustaldi_denbora`**, `ProfilID`, `GailuID`, `EdukiaID`)
* **Generoa** (**`GeneroID`**, `izena`, `subgenero`)
* **Eduki-Genero** (**`Genero_nagusia`**, `EdukiaID`, `generoID`)
* **Dokumentala** (**`Gaia`**, `EdukiaID`, `mota`, `adina_gutxienekoa`, `titulua`, `estrenatze data`)
* **Serieak** (**`Denboraldi_kopurua`**, `EdukiaID`, `mota`, `adina_gutxienekoa`, `titulua`, `estrenatze data`)
* **Pelikulak** (**`Iraupena`**, `EdukiaID`, `mota`, `adina_gutxienekoa`, `titulua`, `estrenatze data`)
* **Denboraldiak** (**`denboraldiID`**, `Sinopsia`, `estrenatze data`, `EdukiaID`)
* **Episodioak** (**`Titulua`**, `Episodio zenbakia`, `iraupena`, `denboraldiID`)

---

## 5. Normalizazioa

Datu-basearen taulak optimizatzeko eta erredundantziak ezabatzeko egindako normalizazio prozesuaren egiaztapena:



---

### 1. Plana
* **Hasierako egitura:** `Plana (Plan ID, izena, prezioa, Erabiltzaile ID, Hasiera Data, Amaiera Data)`
* **Hasierako egoera:** 1FN ✓ | 2FN ✗

**Mendekotasun Taula:**
```text
┌─────────────────┐
│ Plan ID         │ ──► izena, prezioa, Erabiltzaile ID, Hasiera Data, Amaiera Data
├─────────────────┤
│ Erabiltzaile ID │
└─────────────────┘
```

* **Emaitza:** `Plana (Plan ID, izena, prezioa, Hasiera D, Amaiera D)`
* **Emaitzaren egoera:** 1FN ✓ | 2FN ✓ | 3FN ✓

---

### 2. Erabiltzaile
* **Hasierako egitura:** `Erabiltzaile (Erabiltzaile ID, sortze_Data, pasahitza, Posta)`
* **Egoera:** 1FN ✓ | 2FN ✓ | 3FN ✓

---

### 3. Profila
* **Hasierako egitura:** `Profil (Profil ID, izena, mota, Erabiltzaile ID)`
* **Hasierako egoera:** 1FN ✓ | 2FN ✗

**Mendekotasun Taula:**
```text
┌─────────────────┐
│ Profil ID       │
├─────────────────┤ ──► izena, mota
│ Erabiltzaile ID │
└─────────────────┘
```

* **Emaitza:** `Profil (Profil ID, izena, mota)`
* **Emaitzaren egoera:** 1FN ✓ | 2FN ✓ | 3FN ✓

---

### 4. Gailuak
* **Hasierako egitura:** `Gailuak (Gailu ID, mota, izena)`
* **Egoera:** 1FN ✓ | 2FN ✓ | 3FN ✓

---

### 5. Edukiak
* **Hasierako egitura:** `Edukiak (Edukia ID, titulua, estrenatze_Date, adin-gutxienekoa)`
* **Egoera:** 1FN ✓ | 2FN ✓ | 3FN ✓

---

### 6. Bistaratzeak
* **Hasierako egitura:** `Bistaratzeak (Ikustaldi_Denbora, Profil ID, Gailu ID, Edukia ID)`

**Mendekotasun Taula:**
```text
┌───────────┐
│ Profil    │
├───────────┤
│ Gailu     │ ──► Ikustaldi_Denbora
├───────────┤
│ Edukia    │
└───────────┘
```

* **Emaitza:** `Bistaratzeak (Ikustaldi_Denbora)`
* **Emaitzaren egoera:** 1FN ✓ | 2FN ✓ | 3FN ✓

---

### 7. Genero
* **Hasierako egitura:** `Genero (Genero ID, izena, aita-Genero ID)`
* **Hasierako egoera:** 1FN ✓ | 2FN ✓ | 3FN ✓

**Mendekotasun Taula:**
```text
┌─────────────────┐
│ Genero ID       │ ──► izena
├─────────────────┤
│ aita-Genero ID  │
└─────────────────┘
```

* **Emaitza (Eskuineko karratua):** `Genero (Genero ID, izena, aita Genero ID)`

---

### 8. Eduki-Genero
* **Hasierako egitura:** `Eduki-Genero (genero nagusia, Edukia ID, genero ID)`

**Mendekotasun Taula:**
```text
┌───────────┐
│ Edukia    │ ──► Genero nagusia
├───────────┤
│ Genero    │
└───────────┘
```

* **Emaitza:** `Eduki Generoa (genero nagusia)`
* **Emaitzaren egoera:** 1FN ✓ | 2FN ✓ | 3FN ✓

---

### 9. Dokumentalak
* **Hasierako egitura:** `Dokumentalak (Gaia, Edukia ID, titulo, adin_gutxienekoa, estrenatze date)`
* **Egoera:** 1FN ✓ | 2FN ✓ | 3FN ✓

---

### 10. Serieak
* **Hasierako egitura:** `Serieak (Denboraldi_Kopurua, Goiko =)`
* **Egoera:** 1FN ✓ | 2FN ✓ | 3FN ✓

---

### 11. Pelikulak
* **Hasierako egitura:** `Pelikulak (iraupena, Goiko =)`
* **Egoera:** 1FN ✓ | 2FN ✓ | 3FN ✓

---

### 12. Denboraldiak
* **Hasierako egitura:** `Denboraldiak (Denboraldi ID, Sinopsia, estrenatze data, Edukia ID)`
```text
┌───────────────┐
│ Denboraldi ID │ ──► Sinopsia, Estrenatze Data
├───────────────┤
│ Edukia ID     │
└───────────────┘
```

* **Emaitza:** `Denboraldi (Sin Denboraldi ID, Sinopsia, estrenatze data)`
* **Emaitzaren egoera:** 1FN ✓ | 2FN ✓ | 3FN ✓


### 13. Episodioak
* **Hasierako egitura:** `Episodioak (Titula, episodio zenbakia, iraupena, denboraldi ID)`
* **Egoera:** 1FN ✓ | 2FN ✓ | 3FN ✓


### Normalizazio Mailak:
* **Lehen Forma Normala (1FN):** Atributu guztiek balio atomikoak (bakarrak) dituzte. Pelikula batek genero bat baino gehiago izateko, `Eduki_Generoak` taula tartekoa sortu da, zerrendak zutabe bakarrean ez gordetzeko.
* **Bigarren Forma Normala (2FN):** Gako konposatua duten tauletan (`Eduki_Generoak`, `Episodioak`), atributu ez-gako guztiak gako osoaren menpe daude eta ez gakoaren zati baten menpe bakarrik.
* **Hirugarren Forma Normala (3FN):** Gako ez diren atributuen arteko mendekotasunak ezabatu dira. Adibidez, plan baten prezioa `Planak` taulan bakarrik gordetzen da eta ez erabiltzaileen taulan.

---

## 6. Inplementazioa Microsoft Access-en

Eredu erlazionala prest dagoenean, Microsoft Access datu-baseen kudeatzailera igarotzeko urrats hauek jarraitu dira


* **Tauleen Diseinua eta Egitura:** Diseinu-ikuspegia erabiliz, proiektuko 13 taulak sortu dira Access-en, bakoitzari bere datu-mota egokiena esleituz (`Autozenbakia`, `Testu laburra`, `Zenbakia`, `Data/Ordua`, `Moneta`, `Bai/Ez`) eta eremuen deskribapen pertsonalizatuak gehituz.
* **Gako Nagusiak eta Konposatuak:** Taula bakoitzaren gako nagusia (`PK`) zehaztu da. Subtipoetan (`Pelikulak`, `Serieak`, `Dokumentalak`), entitate ahulean (`Episodioak`) eta N:M zein ternarioetan (`Eduki-Genero`, `Bistaratzea`) gako konposatuak eta kanpo-gakoak (`FK`) egoki konfiguratu dira.
* **Erreferentzia-Osotasuna eta Erlazioak:** Access-eko Erlazioen leihoan 13 tauleen arteko konexio guztiak ezarri dira, *Erreferentzia-osotasuna behartu* aukera hautatuz datu orfanoak edo inkonsekuenteak saihesteko.
* **Baliozkotze Arauak (Reglas de Validación):** Eremuen propietateetan murriztapen-arauak ezarri dira; esaterako, `adin_gutxienekoa` eremuan `>= 18` baldintza eta abisu-testu pertsonalizatua jarri dira adin-muga bermatzen dela ziurtatzeko.
* **Datu-Proben Txertaketa Ordenatua:** Datu-basearen funtzionamendua egiaztatzeko erregistroak txertatu dira exekuzio-ordena zorrotz bati jarraituz (entitate independenteetatik hasi eta tarteko zein entitate ahuletaraino), osotasun-akatsik ez gertatzea ziurtatuz.

---

### 6.1. Datu-Moten Ezarpena Access-en
Taulak sortzerakoan (`Diseinu-ikuspegia` / *Vista Diseño* erabiliz), zutabe bakoitzari dagokion datu-mota egokia ezarri zaio:

* **Identifikatzaileak / Gako Nagusiak (PK):** `Autozenbakia` (*Autonumeración*) erabili da zenbakiak automatikoki sortzeko (adib. `ErabiltzaileID`, `PlanID`, `EdukiaID`...).
* **Testuak:**
  * Testu laburrak (`posta`, `pasahitza`, `izena`, `titulua`): `Testu laburra` (*Texto corto* - gehienez 255 karaktere).
* **Zenbakiak:**
  * Zenbaki osoak (`iraupena`, `denboraldi_kopurua`, `adin_gutxienekoa`): `Zenbakia` (*Número* $\rightarrow$ *Entero largo*).
* **Egutegia eta Denbora:**
  * Datak (`sortze_data`, `Hasiera_data`, `estrenatze_data`, `Ikustaldi_denbora`): `Data/Ordua` (*Fecha/Hora*).
* **Dirua:**
  * Prezioak (`prezioa`): `Moneta` (*Moneda*).
* **Balio boolearrak:**
  * Genero nagusia den ala ez adierazteko (`Genero_nagusia`): `Bai/Ez` (*Sí/No*).

---

### 6.2. Gako Nagusiak (PK) eta Konposatuak
Access-en taulak sortzerakoan gako nagusiak zehaztu dira:
1. **Gako bakunak:** Zutabean eskuineko botoiaz klik egin eta `Gako nagusia` (*Clave principal*) hautatuz.
2. **Gako konposatuak (Entitate ahulean, N:M eta Ternariotan):**
   * **`Episodioak`:** `denboraldiID` eta `episodio_zenbakia` zutabe biak batera hautatu dira gako nagusi konposatua ezartzeko.
   * **`Eduki-Genero`:** `EdukiaID` eta `GeneroID` zutabe biak batera hautatu dira gako nagusi konposatua sortzeko.
   * **`Bistaratzea`:** `ProfilID`, `GailuID` eta `EdukiaID` hiru zutabeak batera hautatu dira gako nagusi ternarioa sortzeko.

---

### 6.3. Erlazioen Leihoa eta Erreferentzia-Osotasuna (*Relaciones e Integridad Referencial*)

Access-eko **`Datu-baseko tresnak` $\rightarrow$ `Erlazioak`** atalean tauletako gakoak elkarren artean lotu dira:

1. **Erlazioak sortzea:** Jatorrizko taulako Gako Nagusia (PK) klikatu eta saguaz arrastatu da helburuko taulako Kanpo-Gakoraino (FK).
2. **Erreferentzia-osotasuna bermatzea (*Exigir integridad referencial*):**
   * Aukera hau beti markatu da. Honi esker, ezingo da sortu existitzen ez den erabiltzaile baten profil bat, ezta existitzen ez den plan baten suskripzioa ere.
3. **Kaskadan ezabatzea eta eguneratzea (*Actualizar/Eliminar en cascada*):**
   * **`Serieak` $\rightarrow$ `Denboraldiak` $\rightarrow$ `Episodioak`** loturetan kaskadan ezabatzeko aukera aktibatu da: serie bat ezabatzen denean, bere denboraldi eta kapitulu guztiak automatikoki ezabatuko dira datu-basea garbi mantentzeko.

---

### 6.4. Datuak Sartzeko eta Ezabatzeko Prozedura

Access-en erreferentzia-osotasuna aktibatuta dagoenez, datuak manipulatzeko ordena zehatz bat jarraitu da:

#### Datuak Sartzeko Ordena (Txertaketa):
1. **Hasierako entitate independenteak:** Lehenik `Erabiltzaile`, `Gailuak` eta `Generoa` tauletan sartu dira datuak.
2. **Dependentzia zuzena dutenak:** Ondoren `Plana` eta `Profila` (`ErabiltzaileID` behar dute), eta `Edukiak` taulan.
3. **Herentzia eta serieak:** Jarraian `Pelikulak`, `Dokumentalak` eta `Serieak` bete dira (`EdukiaID` erabiliz), eta ondoren `Denboraldiak`.
4. **Entitate ahulek eta tarteko taulak:** Azkenik `Episodioak`, `Eduki-Genero` eta `Bistaratzea` tauletan sartu dira datuak.

#### Datuak Ezabatzeko Ordena:
* **Ume-tauletatik aita-tauletara:** Lehenik ume-tauletako erregistroak ezabatu behar dira (`Episodioak`, `Bistaratzea`, `Eduki-Genero`) aita-taulakoak (`Denboraldiak`, `Profila`, `Edukiak`) ezabatu ahal izateko.
