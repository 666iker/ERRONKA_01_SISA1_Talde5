# Datu-Basearen Diseinuaren Memoria: Streaming Plataforma (Netflix Estiloa)

---

## 1. Sarrera eta Helburua

Dokumentu honen helburua Netflix estiloko streaming plataforma baten datu-basearen diseinua azaltzea da. Prozesua beharren analisitik hasi eta eredu kontzeptual nahiz erlazional normalizatura arte dokumentatzen da, azkenik Microsoft Access bidezko inplementazioarekin amaituz.

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

![Eredu Kontzeptuala](./irudiak/eredu_kontzeptuala.png)

### 3.1. Diseinu Erabaki Nagusiak eta Haien Azalpena

* **`SUSKRIBZIOA` Erronbo Moduan:** 
  `SUSKRIBZIOA` erlazio moduan jarri da erabiltzailearen eta planaren artean. Ez da berezko entitate independente bat; erabiltzaile batek plan bat kontratatzean sortzen den lotura da. `hasiera_data` eta `amaiera_data` atributuak erlazio honen berezko datuak dira.

* **Herentzia Hierarkia (`ISA`) `EDUKIAK` Entitatean:** 
  `EDUKIAK` entitate orokorretik (supermota) hiru azpimota sortu dira: `PELIKULAK`, `SERIEAK` eta `DOKUMENTALAK`. Datu arruntai (`titulua`, `estreinatze_data`, `adina_gutxienekoa`) behin bakarrik gordetzeko `EDUKIAK` taulan, eta balio nulurik (NULL) ez izateko.

* **`EPISODIOAK` Entitate Ahula:** 
  `EPISODIOAK` entitate ahul gisa definitu da `Denboraldiak` entitatearekiko, kapitulu bat ezin delako bere kabuz existitu zein denboralditakoa den jakin gabe. Bere gako nagusia konposatua da (`denboraldi_id` + `episodio_zenbakia`).

* **Generoak `EDUKIAK` Entitateari Lotzea:** 
  `GENEROAK` erlazioa `EDUKIAK` kutxa nagusira konektatu da. Honela, pelikulez gain, serieek eta dokumentalek ere generoa izatea lortzen da lerro bakar batekin, herentzia baliatuz.

* **Generoen Errekurtsibitatea (`SUBGENEROA`):** 
  `GENEROAK` entitateari bucle bat (erlazio errekurtsiboa) jarri zaio, genero nagusien eta subgeneroen arteko hierarkiak sortzeko (adibidez: *Akzioa* $\rightarrow$ *Artes Marzialak*).

* **`BISTARATZEA` Erlazio Hirukoitza (Ternaria):** 
  Rombo bakar batek hiru entitate lotzen ditu: `PROFILA`, `GAILUAK` eta `EDUKIAK`. Ikustaldi bat erregistratzeko hiru faktoreak batera jakin behar dira: **Nork** ikusi duen (Profila), **Zer** ikusi duen (Edukia) eta **Nondik** ikusi duen (Gailua).

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

---

## 4. Eredu Erlazionala

Eredu kontzeptuala taula fisikoetara pasatu ondoren, lortutako eredu erlazionalaren eskema:

![Eredu Erlazionala](./irudiak/eredu_erlazionala.png)

---

## 5. Normalizazioa

Datu-basearen taulak optimizatzeko eta erredundantziak ezabatzeko egindako normalizazio prozesuaren egiaztapena:

![Normalizazioa](./irudiak/normalizazioa.png)

### Normalizazio Mailak:
* **Lehen Forma Normala (1FN):** Atributu guztiek balio atomikoak (bakarrak) dituzte. Pelikula batek genero bat baino gehiago izateko, `Eduki_Generoak` taula tartekoa sortu da, zerrendak zutabe bakarrean ez gordetzeko.
* **Bigarren Forma Normala (2FN):** Gako konposatua duten tauletan (`Eduki_Generoak`, `Episodioak`), atributu ez-gako guztiak gako osoaren menpe daude eta ez gakoaren zati baten menpe bakarrik.
* **Hirugarren Forma Normala (3FN):** Gako ez diren atributuen arteko mendekotasunak ezabatu dira. Adibidez, plan baten prezioa `Planak` taulan bakarrik gordetzen da eta ez erabiltzaileen taulan.

---

## 6. Inplementazioa Microsoft Access-en

Eredu erlazionala prest dagoenean, Microsoft Access datu-baseen kudeatzailera igarotzeko urrats hauek jarraitu dira:

### 6.1. Datu-Moten Ezarpena Access-en
Taulak sortzerakoan (`Diseinu-ikuspegia` / *Vista Diseño* erabiliz), zutabe bakoitzari dagokion datu-mota egokia ezarri zaio:

* **Identifikatzaileak / Gako Nagusiak (PK):** `Autozenbakia` (*Autonumeración*) erabili da zenbakiak automatikoki sortzeko (adib. `erabiltzaile_id`, `plan_id`, `edukia_id`...).
* **Testuak:**
  * Testu laburrak (`posta`, `pasahitza`, `izena`, `titulua`): `Testu laburra` (*Texto corto* - gehienez 255 karaktere).
* **Zenbakiak:**
  * Zenbaki osoak (`iraupena`, `pantailak`, `denboraldi_kopurua`): `Zenbakia` (*Número* $\rightarrow$ *Entero largo*).
* **Egutegia eta Denbora:**
  * Datak (`sortze_data`, `hasiera_data`, `estreinatze_data`): `Data/Ordua` (*Fecha/Hora*).
* **Dirua:**
  * Prezioak (`prezioa`): `Moneta` (*Moneda*).
* **Balio boolearrak:**
  * Genero nagusia den ala ez adierazteko (`nagusia`): `Bai/Ez` (*Sí/No*).

---

### 6.2. Gako Nagusiak (PK) eta Konposatuak
Access-en taulak sortzerakoan gako nagusiak zehaztu dira:
1. **Gako bakunak:** Zutabean eskuineko botoiaz klik egin eta `Gako nagusia` (*Clave principal*) hautatuz.
2. **Gako konposatuak (Entitate ahulean eta Tarteko tauletan):**
   * **`Episodioak`:** `denboraldi_id` eta `episodio_zenbakia` zutabe biak batera hautatu dira gako nagusi konposatua ezartzeko.
   * **`Eduki_Generoak`:** `edukia_id` eta `genero_id` zutabe biak batera hautatu dira gako nagusi konposatua sortzeko.

---

### 6.3. Erlazioen Leihoa eta Erreferentzia-Osotasuna (*Relaciones e Integridad Referencial*)

Access-eko **`Datu-baseko tresnak``Erlazioak`** (*Herramientas de base de datos Relaciones*) atalean tauletako gakoak elkarren artean lotu dira:

1. **Erlazioak sortzea:** Jatorrizko taulako Gako Nagusia (PK) klikatu eta saguaz arrastatu da helburuko taulako Kanpo-Gakoraino (FK).
2. **Erreferentzia-osotasuna bermatzea (*Exigir integridad referencial*):**
   * Aukera hau **beharrezkoa da beti markatzea**. Honi esker, ezingo da sortu adibidez lehendik existitzen ez den erabiltzaile baten profil bat, ezta existitzen ez den plan baten suskripzioa ere.
3. **Kaskadan ezabatzea eta eguneratzea (*Actualizar/Eliminar en cascada*):**
   * **`Serieak`  `Denboraldiak`  `Episodioak`** loturetan kaskadan ezabatzeko aukera aktibatu da: serie bat ezabatzen denean, bere denboraldi eta kapitulu guztiak automatikoki ezabatuko dira datu-basea garbi mantentzeko.
