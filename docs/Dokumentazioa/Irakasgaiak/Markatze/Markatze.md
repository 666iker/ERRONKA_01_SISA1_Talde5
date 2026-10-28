# Markatze Lengoaiak (HTML5 eta CSS3) — Dokumentazio Teknikoa (`Erronka1 html`)

Dokumentu honek `Erronka1 html` karpetako **HTML eta CSS garapen osoa** azaltzen du, `Errubrika_SISA1_Zuzenduta.xlsx` fitxategiko **MARKATZE LENGOAIAK** moduluko **IE1** eta **IE2** ebaluazio-irizpideetan (**AURRERATUA - 10**) oinarrituta.

---

## 0. Errubrikaren Irizpideak: Markatze Lengoaiak

| Ikasketa Emaitza (IE) | Ebaluazio Irizpidea | Aurreratua (10) Mailaren Eskakizuna | Non eta Nola Bete da Proiektuan? |
| :--- | :--- | :--- | :--- |
| **IE 1** *(Pisua: 0.09)*<br>Markatzeko lengoaien ezaugarriak ezagutzen ditu, eta kode zatiak aztertu eta interpretatzen ditu. | **1.1. HTML Egitura:** Orria hiru atal nagusitan banatzen du: goiburua, gorputza eta oina (`header`, `main`/`body`, `footer`). | Egitura oso argia eta koherentea du; atal bakoitzak bere funtzioa betetzen du eta semantika zehatz aplikatzen da (`header`/`nav`/`main`/`footer`). | 5 HTML fitxategietan txantiloi semantiko bera erabili da: `<header>`, `<nav>`, `<main>`, `<section>` eta `<footer>`. |
| **IE 1** *(Pisua: 0.09)* | **1.2. CSS Antolaketa:** CSSan etiketen, IDen eta klaseen bidez estiloak antolatzen eta aplikatzen ditu modu egokian. | CSS antolatua eta garbia da; `class` eta `ID` izenak argiak eta funtzionalak dira, eta estiloak egiturarekin bat datoz. | `css/style.css` fitxategi bakarrean zentralizatuta, euskarazko klase eta ID izen deskriptiboekin. |
| **IE 2** *(Pisua: 0.14)*<br>Markatzeko lengoaiak erabiltzen ditu informazioa web-aren bitartez transmititzeko, dokumentuen egitura aztertuz. | **2.1. Web Egitura eta Nabigazio-Menua:** Web orriaren egitura eta nabigazio-menua diseinatzen eta martxan jartzen du, atalak elkartuz. | Nabigazioa intuitiboa eta eraginkorra da; diseinua garbia da. | Goiburu finkoa (`sticky`), 5 orrien arteko lotura gurutzatua, `.aktiboa` klasea orri aktiboa markatzeko eta orri-oineko bigarren menua. |

---

## 1. IE 1 — 1. Irizpidea: HTML Orrien Hiru Atal Nagusiak eta Egitura Semantikoa (`header`, `main`, `footer`)

Webguneko 5 orriek (`index.html`, `sareak.html`, `digitalizazioa.html`, `datubasea.html` eta `kontaktua.html`) egitura semantiko berbera eta koherentea erabiltzen dute `<body>` barruan:

### 1.1. Goiburua (`<head>` — Metadatuak eta CSS lotura)
Orri guztietako lehen 8 lerroetan (adib. `index.html` L1-L8) dokumentuaren konfigurazioa definitzen da:

```html
<!DOCTYPE html>
<html lang="eu">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>1. Erronka - Klaseko Proiektu Teknikoa (SISA1)</title>
    <link rel="stylesheet" href="css/style.css">
</head>
```
* **`<!DOCTYPE html>` eta `<html lang="eu">`:** HTML5 estandarra eta euskara (`eu`) hizkuntza nagusi gisa ezartzen ditu.
* **`<meta charset="UTF-8">`:** Karaktere bereziak (`ñ`, azentuak, ikur matematikoak eta emojiak) ondo bistaratzea bermatzen du.
* **`<meta name="viewport" ...>`:** Diseinua pantaila txikietara (mugikor eta tabletetara) egokitzeko ezinbesteko etiketa.
* **`<link rel="stylesheet" href="css/style.css">`:** HTML dokumentua kanpoko CSS estilo-orriarekin lotzen du.

---

### 1.2. Lehen Atal Nagusia: `<header>` eta `<nav>` (Webgunearen Goiburua)
Orri guztietako goiburua (adib. `index.html` L11-L28) `<header>` etiketa semantikoaren barruan dago:

```html
<header>
    <div class="container nav-bar">
        <div class="logo">
            <a href="index.html">
                <img src="img/logo.svg" alt="1. Erronka Logotipoa">
            </a>
        </div>
        <nav>
            <ul class="menu-zerrenda">
                <li><a href="index.html" class="aktiboa">Hasiera</a></li>
                <li><a href="sareak.html">Sareak (OSI 1-2)</a></li>
                <li><a href="digitalizazioa.html">Digitalizazioa</a></li>
                <li><a href="datubasea.html">Datu-Basea</a></li>
                <li><a href="kontaktua.html">Azpiegitura eta Taldea</a></li>
            </ul>
        </nav>
    </div>
</header>
```
* **`<header>`:** Orriaren goiko barra beltza definitzen du.
* **`.logo`:** `img/logo.svg` irudia `<a href="index.html">` esteka baten barruan sartzen du, edozein orritatik logotipoan klik egitean hasierara itzultzeko.
* **`<nav>`:** Nabigazio-menua biltzen duen etiketa semantikoa da, `<ul class="menu-zerrenda">` zerrendaren bidez 5 orriak elkartuz.

---

### 1.3. Bigarren Atal Nagusia: `<main>` eta `<section>` (Gorputz Nagusia)
Eduki guztia `<main class="container">` barruan kokatzen da (adib. `index.html` L30-L141), eta gai bakoitza `<section>` etiketekin banatzen da:
* **`<main>`:** Dokumentuaren eduki nagusi eta bakarra biltzen du (goiburua eta oina kanpoan utziz).
* **`<section>`:** Gai bakoitza bloke semantiko independentetan banatzen du (aurkezpena, azalpen teorikoak, taulak, formularioa).
* **Izenburu hierarkikoak (`<h1>`, `<h2>`, `<h3>`):**
  * `<h1>`: Orri bakoitzeko izenburu nagusia (behin bakarrik orri bakoitzean).
  * `<h2>`: `<section>` bakoitzaren izenburua, azpian marra gorri batekin (`border-bottom: 2px solid #e50914`).
  * `<h3>`: Azpiatalen izenburuak (adib. datu-baseko 7 taulak bereizteko).

---

### 1.4. Hirugarren Atal Nagusia: `<footer>` (Orri-oina)
Orri guztien amaieran (adib. `index.html` L143-L172) `<footer>` etiketa semantikoa erabiltzen da:

```html
<footer>
    <div class="container footer-edukia">
        <div class="footer-blokea">
            <h4>1. Erronka — SISA1</h4>
            <p>Klaseko Proiektu Teknikoa (5. Taldea)</p>
            <p><strong>Ikasturtea:</strong> 2026-2027</p>
        </div>
        <div class="footer-blokea">
            <h4>Ikastetxea eta Kokapena</h4>
            <p><strong>Zentroa:</strong> CIFP Uni Eibar-Ermua LHII</p>
            <p><strong>Udalerria:</strong> Eibar, 20600 (Gipuzkoa)</p>
        </div>
        <div class="footer-blokea">
            <h4>Webguneko Atalak</h4>
            <ul>
                <li><a href="index.html">Hasiera</a></li>
                <li><a href="sareak.html">Sareak (OSI 1-2 eta CRC)</a></li>
                <li><a href="digitalizazioa.html">Digitalizazioa (IT/OT, THD, Cloud)</a></li>
                <li><a href="datubasea.html">Datu-Basea (ER eta Taulak)</a></li>
                <li><a href="kontaktua.html">Azpiegitura eta Taldea</a></li>
            </ul>
        </div>
    </div>
    <div class="container footer-egilea">
        <p>&copy; 2026 1. Erronka - Egileak: 5. Taldea</p>
    </div>
</footer>
```

---

## 2. IE 1 — 2. Irizpidea: CSS Antolaketa Etiketen, Klaseen eta IDen bidez (`css/style.css`)

`css/style.css` fitxategiak (476 lerro) hiru hautatzaile mota uztartzen ditu modu garbi eta antolatuan:

### 2.1. Etiketa-hautatzaileak (Oinarrizko HTML elementuak)
* **`*` (L1-L5):** Reset unibertsala (`box-sizing: border-box; margin: 0; padding: 0;`) nabigatzaileen berezko marjinak ezabatzeko.
* **`body` (L7-L12):** Atzeko plano iluna (`#141414`), testu zuria (`#ffffff`), `Arial, Helvetica, sans-serif` letra-tipoa eta `line-height: 1.6` irakurgarritasuna hobetzeko.
* **`header` (L21-L28):** Atzeko plano beltza (`#000000`), beheko ertz gorria (`3px solid #e50914`) eta **`position: sticky; top: 0; z-index: 100;`** menua beti goian finkatuta mantentzeko.
* **`h1`, `h2`, `h3`, `p`, `a` (L79-L110):** Tipografia-tamainak eta kolore korporatiboak.
* **`table`, `th`, `td`, `tr` (L298-L326):** `border-collapse: collapse`, goiburu beltzak letra gorriekin (`th`), errenkada bikoitiak tonu desberdinean (`tr:nth-child(even)`) eta saguaren efektua (`tr:hover`).
* **`footer` (L399-L405):** Orri-oinaren kolore iluna (`#0c0c0c`) eta goiko ertz gorria.

### 2.2. Klase-hautatzaileak (`.class` — Euskarazko izen funtzional eta deskriptiboak)
Klase guztiek funtzio argi bat dute eta hainbat orritan berrerabiltzen dira:

| Klasea (`.class`) | Lerroak `style.css`-n | CSS Propietate Nagusiak | Funtzioa Webgunean |
| :--- | :---: | :--- | :--- |
| **`.container`** | L14-L19 | `width: 92%; max-width: 1120px; margin: 0 auto;` | Edukia pantailaren erdian lerrokatzen du. |
| **`.nav-bar`** | L30-L36 | `display: flex; justify-content: space-between; align-items: center;` | Logotipoa ezkerrean eta menua eskuinean kokatzen ditu. |
| **`.menu-zerrenda`** | L43-L77 | `list-style: none; display: flex; gap: 8px;` | Menuko `<li>` elementuak errenkada horizontalean jartzen ditu. |
| **`.aktiboa`** | L73-L77 | `background-color: #e50914; color: #ffffff;` | Unean irekita dagoen orriaren botoia gorriz nabarmentzen du. |
| **`.hero-kutxa`** | L112-L122 | `background-color: #1f1f1f; border-radius: 8px; display: flex;` | `index.html` eta `kontaktua.html` orrietako aurkezpen-kutxa. |
| **`.rol-sarea` / `.rol-txartela`** | L141-L172 | `display: flex; flex-wrap: wrap; gap: 20px; width: 240px;` | Hasierako orriko 4 arlo teknikoen txartelak eta ikonoak. |
| **`.botoi-gorria` / `.botoi-grisa`** | L174-L207 | `display: inline-block; padding: 10px 18px; border-radius: 4px;` | Estekak (`<a>`) eta formularioko botoiak (`<button>`) botoi gisa apaintzen ditu. |
| **`.diagrama-kutxa`** | L238-L254 | `background-color: #1a1a1a; text-align: center; max-width: 100%;` | SVG eskemak eta diagramak erdian eta modu erantzunkorrean erakusten ditu. |
| **`.kode-kutxa`** | L256-L268 | `font-family: monospace; color: #46d369; white-space: pre;` | CRC-6 zatiketa bitarra eta 3FN eskema terminal baten moduan erakusten ditu. |
| **`.zerrenda-arrunta` / `.zerrenda-zenbatua`** | L270-L290 | `background-color: #1f1f1f; border-left: 4px solid #e50914;` | `<ul>` eta `<ol>` zerrendetako puntu bakoitza kutxa estilizatu bihurtzen du. |
| **`.taula-edukiontzia`** | L292-L296 | `overflow-x: auto; margin-top: 15px;` | Pantaila txikietan taulei korritze horizontala gehitzen die. |
| **`.gako-pk` / `.gako-fk` / `.etiketa-prezioa`** | L328-L356 | `display: inline-block; padding: 2px 6px; border-radius: 3px;` | Taulen barruko etiketa koloredunak (`PK` laranja, `FK` urdina, `Aktibo` gorria). |
| **`.formulario-kutxa` / `.eremu-multzoa`** | L358-L397 | `max-width: 650px; width: 100%; border-color: #e50914 (:focus)` | Harremanetarako formularioaren kutxa eta input eremuak. |
| **`.footer-edukia` / `.footer-blokea`** | L407-L457 | `display: flex; flex-wrap: wrap; flex: 1; min-width: 210px;` | Orri-oina 3 zutabe malgutan banatzen du. |

### 2.3. IDen Erabilera (`#id` — Elementu bakarrak eta aingurak)
IDak elementu bakar eta errepikaezinetarako soilik erabili dira HTML arau semantikoak errespetatuz:
* **`id="formularioa"` (`kontaktua.html` L104):** Goiko `<a href="#formularioa" class="botoi-gorria">` botoiari klik egitean orriaren beheko formulariora zuzenean jaisteko aingura.
* **`id="izena"`, `id="email"`, `id="atala"`, `id="mezua"` (`kontaktua.html` L111-L133):** Formularioko sarrera-eremu bakoitza bere `<label for="...">` etiketarekin lotzeko, irisgarritasuna bermatuz.

### 2.4. Diseinu Erantzunkorra (`@media (max-width: 768px)`)
`css/style.css` (L459-L476) amaieran Media Query bat gehitu da: mugikorretan (`<= 768px`) `.nav-bar` eta `.hero-kutxa` zutabe bertikalean (`flex-direction: column`) jartzen dira automatikoki.

---

## 3. IE 2 — 1. Irizpidea: Nabigazio-Menua eta Atalen Arteko Lotura

Webguneak **lau nabigazio-mekanismo osagarri** ditu atal guztiak modu intuitiboan elkartzeko:

1. **Goiko Menu Nagusi Finkoa (`<nav>`):**
   * 5 orrietan leku berean agertzen da. Orri bakoitzean `class="aktiboa"` klasea aldatzen da erabiltzaileak beti jakin dezan zein ataletan dagoen:
     * `index.html` (L20): `<a href="index.html" class="aktiboa">Hasiera</a>`
     * `sareak.html` (L21): `<a href="sareak.html" class="aktiboa">Sareak (OSI 1-2)</a>`
     * `digitalizazioa.html` (L22): `<a href="digitalizazioa.html" class="aktiboa">Digitalizazioa</a>`
     * `datubasea.html` (L23): `<a href="datubasea.html" class="aktiboa">Datu-Basea</a>`
     * `kontaktua.html` (L24): `<a href="kontaktua.html" class="aktiboa">Azpiegitura eta Taldea</a>`
2. **Hero Botoiak eta Aurkibide Zenbatua (`index.html` L36-L37 eta L121-L138):**
   * Hasierako orritik atal garrantzitsuenetara zuzenean joateko botoi gorri/grisak eta zerrenda zenbatuko estekak.
3. **Barne-aingura (`#formularioa` — `kontaktua.html` L36):**
   * Orri bereko atal batera jauzi egiteko esteka (`href="#formularioa"`).
4. **Orri-oineko Nabigazio Zerrenda (`<footer>`):**
   * Orriaren amaierara iristean gora igo beharrik gabe edozein ataletara joateko bigarren esteka-zerrenda.

---

## 4. Bost HTML Fitxategien Azterketa Xehea (Erabilitako Elementuak)

### 4.1. `index.html` (Hasierako Orria)
* **Erabilitako HTML elementu nagusiak:**
  * `<section class="hero-kutxa">` (L32-L39): Aurkezpen-testua (`<h1>`, `<p>`, `<strong>`) eta bi `<a class="botoi-gorria / botoi-grisa">` esteka.
  * `<div class="rol-sarea">` (L45-L69): 4 `.rol-txartela` kutxa, bakoitza `<div class="rol-ikonoa" style="background-color: ...;">` ikonoarekin, `<h3>` izenburuarekin eta `<p>` azalpenarekin.
  * `<ul class="zerrenda-arrunta">` (L76-L80) eta `<table>` (L83-L113): Windows Server azpiegituraren laburpena (`<code>` etiketarekin bide eta atakak markatuz).
  * `<ol class="zerrenda-zenbatua">` (L121-L138): Webguneko 4 atalen aurkibide estekatua (`<sup>` goi-indizearekin $x^6$ polinomioa idazteko).

### 4.2. `sareak.html` (Sareak: OSI 1-2, CRC-6-GSM eta Modulazioa)
* **Erabilitako HTML elementu nagusiak:**
  * `<table>` (L50-L109): `Kuper-67` pasahitzaren ASCII bihurketa-taula, `'K'` letraren balio bitarra `<span class="etiketa-prezioa">01001011</span>` bidez nabarmenduz.
  * `<sup>` (L113-L117): Polinomio sortzailearen berretzaileak ($x^6 + x^5 + x^3 + x^2 + x + 1$) adierazteko.
  * `<div class="kode-kutxa">` (L120-L142): Aurrez formateatutako testu-blokea (`white-space: pre`), XOR zatiketa bitarraren urrats guztiak zutabeetan lerrokatuta erakusteko.
  * `<div class="diagrama-kutxa">` + `<img src="img/modulazioa-manchester.svg">` (L149-L151): SVG grafiko bektoriala txertatzen du, eta azpian `<table>` bat `&darr;` eta `&uarr;` HTML entitateekin.

### 4.3. `digitalizazioa.html` (Digitalizazioa: IT/OT, THD eta Cloud)
* **Erabilitako HTML elementu nagusiak:**
  * `<ul class="zerrenda-arrunta">` (L41-L44) eta konparazio-taula `<table>` (L47-L72) IT eta OT inguruneak bereizteko.
  * `<ol class="zerrenda-zenbatua">` (L80-L97): THD teknologiak zenbakituta azaltzeko.
  * `<h3>`, `<table>` eta `<ul>` (L104-L139): Cloud zerbitzu-ereduak (`IaaS`, `PaaS`, `SaaS`) eta hedapen-ereduak.

### 4.4. `datubasea.html` (Datu-Basea: ER Diagrama eta 7 Taulak)
* **Erabilitako HTML elementu nagusiak:**
  * `<div class="diagrama-kutxa">` (L49-L52): ER diagramaren irudia eta oin-oharra.
  * `<div class="kode-kutxa">` (L59-L66): 3FN eredu erlazionalaren 7 taulen eskema.
  * **7 `<table>` taula normalizatu (L73-L408):** Taula bakoitzaren `<thead>` goiburuan `<span class="gako-pk">PK</span>` eta `<span class="gako-fk">FK</span>` etiketak erabiltzen dira gako nagusiak eta arrotzak bisualki identifikatzeko, eta `&euro;` (€) entitatea prezioetarako.

### 4.5. `kontaktua.html` (Azpiegitura, Planifikazioa eta Formularioa)
* **Erabilitako HTML elementu nagusiak:**
  * **Zerrenda habiaratuak (`<ol>` barruan `<ul>`, L44-L67):** Windows Server-eko IIS, RAID bolumenak (`C:`, `E:`, `F:`) eta segurtasun-politikak egituratzeko.
  * **HTML5 Formulario osoa (`<form action="#" method="post">`, L109-L138):**
    * `<label for="...">` + `<input type="text" id="izena" required>`: Testu-eremu derrigorrezkoa.
    * `<input type="email" id="email" required>`: Posta elektronikoaren formatua automatikoki balidatzen duen eremua.
    * `<select id="atala">` + 5 `<option value="...">`: Goitibeherako hautatzailea.
    * `<textarea id="mezua" rows="4" required>`: Lerro anitzeko iruzkin-koadroa.
    * `<button type="submit" class="botoi-gorria">` eta `<button type="reset" class="botoi-grisa">`: Bidaltzeko eta garbitzeko botoiak.
