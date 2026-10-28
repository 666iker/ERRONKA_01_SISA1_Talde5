# 1. Erronka - Webgunearen Dokumentazioa (Markatze Lengoaiak)

**Taldea:** SISA1 - 5. Taldea (Axular Muñoz, Jokin Martinez, Iker Monkayola eta Alex Sierra)

Dokumentu honetan 1. Erronkarako sortu dugun webgune estatikoa nola egin dugun azaltzen da. Gure helburua klasean landutako atal guztiak (Sareak, Digitalizazioa, Datu-Basea eta Windows Server) webgune bakar batean biltzea izan da, gero Windows Server-eko IIS zerbitzarian argitaratzeko. Datu-basearen gaia streaming plataforma bat denez, webguneari ere estilo iluna (beltza eta gorria, Netflix modukoa) ematea erabaki dugu.

---

## 1. Karpetaren egitura

Proiektua ondo antolatuta edukitzeko, fitxategiak honela banatu ditugu `Erronka1 html` karpetan:

* **5 HTML fitxategi nagusiak:**
  * `index.html`: Hasierako orria eta proiektuaren laburpena.
  * `sareak.html`: OSI 1 eta 2 mailak, pasahitza ASCIIra pasatzea, CRC-6 kalkulua eta modulazioak.
  * `digitalizazioa.html`: IT/OT, THD teknologiak eta Cloud computing.
  * `datubasea.html`: Streaming plataformaren ER diagrama eta normalizatutako 7 taulak.
  * `kontaktua.html`: Windows Server azpiegitura, taldearen planifikazioa eta kontaktuko formularioa.
* **`css` karpeta:** Barruan `style.css` fitxategia dago. Orri guztiek CSS fitxategi berbera erabiltzen dute estiloa mantentzeko.
* **`img` karpeta:** Webguneko irudiak gordetzeko (`logo.svg` eta `modulazioa-manchester.svg`).

---

## 2. HTML orrien egitura: Header, Main eta Footer (IE1)

Webgune osoak itxura berdina izateko, txantiloi moduko bat prestatu dugu lehenengo eta gero orri bakoitzeko edukia aldatu dugu. HTML fitxategi guztiak hiru zati nagusitan banatuta daude: goiburua (`header`), gorputza (`main`) eta oina (`footer`).

### Goiburua (`<head>` eta `<header>`)
`<head>` zatian oinarrizko konfigurazioa jarri dugu: `charset="UTF-8"` euskarazko karaktereekin eta ikurrekin arazorik ez izateko, `viewport` mugikorrean ondo ikusteko eta `<link rel="stylesheet" href="css/style.css">` estilo-orria kargatzeko.

`<body>` barruan, lehenengo gauza `<header>` etiketa da. Hemen bi elementu jarri ditugu `.nav-bar` klasearen barruan:
1. Ezkerrean taldearen logotipoa (`img/logo.svg`), `<a>` etiketa batekin `index.html` orrira lotuta.
2. Eskuinean `<nav>` etiketa, nabigazio-menuarekin.

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

### Gorputz nagusia (`<main>` eta `<section>`)
Orri bakoitzaren informazio guztia `<main class="container">` barruan doa. Edukia ez nahasteko, gai bakoitza `<section>` etiketekin banatu dugu. Atal bakoitzaren barruan izenburuak (`<h1>` orriaren titulurako, `<h2>` atal bakoitzerako eta `<h3>` azpiataletarako) eta paragrafoak (`<p>`) erabili ditugu.

### Orri-oina (`<footer>`)
Orriaren behealdean `<footer>` jarri dugu. Hiru zutabetan banatuta dago (`.footer-blokea` klasearekin):
* Lehenengoan erronkaren datuak eta ikasturtea (2026-2027).
* Bigarrenean ikastetxearen informazioa (CIFP Uni Eibar-Ermua LHII).
* Hirugarrenean webguneko orrietara joateko esteka zerrenda bat.
Azpian `.footer-egilea` jarri dugu taldearen copyright-arekin.

---

## 3. Nabigazio-menua eta orrien arteko lotura (IE2)

Orri batetik bestera erraz ibiltzeko, nabigazio-menua `<nav>` barruan egin dugu `<ul>` zerrenda batekin (`.menu-zerrenda`).

* **Orri aktiboa markatzea:** Erabiltzaileak une oro jakin dezan zein orritan dagoen, orri bakoitzeko HTMLan `class="aktiboa"` jarri diogu dagokion estekari. Adibidez, `sareak.html` fitxategian *Sareak* botoiak dauka klase hori, eta CSS bidez atzeko plano gorria (`#e50914`) jartzen zaio.
* **Menu finkoa:** CSSan `header` etiketari `position: sticky; top: 0;` jarri diogu. Horrela, orrian behera egin arren (batez ere `datubasea.html` bezalako orri luzeetan), menua beti goian geratzen da bistan.
* **Beste esteka batzuk:** Menuko estekez gain, `index.html` orrian botoi gorri eta grisak jarri ditugu atal nagusietara joateko (`.botoi-gorria` eta `.botoi-grisa`), eta `kontaktua.html` orrian barne-esteka bat (`href="#formularioa"`) jarri dugu botoia sakatzean zuzenean beheko formulariora jaisteko.

---

## 4. Orri bakoitzean erabilitako HTML elementuak

* **`index.html`:** Hasierako aurkezpen-kutxa (`.hero-kutxa`), 4 arlo teknikoak azaltzeko txartelak (`.rol-sarea` eta `.rol-txartela`), Windows Server-eko laburpen-taula (`<table>`) eta orri guztien aurkibidea `<ol>` zerrenda zenbatu batekin.
* **`sareak.html`:** Pasahitza (`Kuper-67`) ASCIIra pasatzeko taula, polinomioa idazteko `<sup>` etiketa (berretzaileak jartzeko, adibidez `x<sup>6</sup>`), eta CRC-6 zatiketa bitarra erakusteko `<div class="kode-kutxa">` blokea. Azken honi CSSan `white-space: pre` jarri diogu espazioak eta zutabeak ez mugitzeko. Horrez gain, Manchester eta ASK/FSK/PSK modulazioen irudia (`<img>`) eta Wi-Fi probaren pausoak daude.
* **`digitalizazioa.html`:** IT eta OT desberdintzeko zerrendak (`<ul class="zerrenda-arrunta">`) eta konparazio-taula. THD teknologiak `<ol class="zerrenda-zenbatua">` zerrenda batekin egin ditugu eta Cloud atala (`IaaS`, `PaaS`, `SaaS`) beste taula batekin.
* **`datubasea.html`:** ER diagramaren irudia, taulen eskema `.kode-kutxa` batean, eta 7 taulak (`<table>`, `<thead>`, `<tbody>`, `<tr>`, `<th>`, `<td>`). Taulen goiburuetan gako nagusiak eta kanpo-gakoak bereizteko `<span class="gako-pk">PK</span>` eta `<span class="gako-fk">FK</span>` etiketak sortu ditugu.
* **`kontaktua.html`:** Windows Server-eko azalpena zerrenda habiaratuekin (`<ol>` baten barruan `<ul>` zerrendak sartuta RAID bolumenak eta segurtasun-politikak azaltzeko), taldekideen lan-banaketaren taula eta harremanetarako `<form>` formularioa (`input type="text"`, `input type="email"`, `select`, `textarea` eta `submit`/`reset` botoiekin).

---

## 5. CSS estiloen antolaketa: Etiketak, Klaseak eta IDak (`css/style.css`) (IE1)

HTML kodea garbi mantentzeko, estilo guztiak `css/style.css` fitxategian sartu ditugu. Hiru modutara antolatu ditugu hautatzaileak:

### 1. Etiketa bidezko estiloak
Oinarrizko etiketei zuzenean eman diegu estiloa webgune osoan berdin ikusteko:
* `*`: `margin: 0; padding: 0; box-sizing: border-box;` nabigatzaileak berez jartzen dituen tarteak kentzeko.
* `body`: Atzeko plano iluna (`#141414`), letra zuria (`#ffffff`) eta Arial letra-tipoa.
* `h1`, `h2`, `h3`, `p`, `a`: Tituluen tamainak eta koloreak (`h2` tituluei azpiko marra gorria jarri diegu).
* `table`, `th`, `td`: Taulen ertzak batzeko (`border-collapse: collapse`), goiburu beltza letra gorriekin (`th`) eta errenkada bikoitiei beste gris tonu bat emateko (`tr:nth-child(even)`).

### 2. Klaseak (`.class`)
Gehien erabili ditugun klaseak euskarazko izenekin sortu ditugu, zer egiten duten erraz ulertzeko:
* `.container`: Webguneko edukia erdian zentratzeko (`max-width: 1120px; margin: 0 auto;`).
* `.nav-bar` eta `.menu-zerrenda`: `display: flex` erabiliz menuko elementuak lerroan jartzeko.
* `.hero-kutxa`, `.rol-sarea` eta `.rol-txartela`: Aurkezpeneko kutxak eta txartelak maketatzeko.
* `.botoi-gorria` eta `.botoi-grisa`: Estekei eta formularioko botoiei botoi itxura emateko.
* `.zerrenda-arrunta` eta `.zerrenda-zenbatua`: Zerrendetako `<li>` bakoitzari kaxa ilun bat eta ezkerreko marra gorria (`border-left: 4px solid #e50914`) jartzeko.
* `.kode-kutxa`: Kontsola itxurako kaxa beltza letra berdeekin (`#46d369`), CRC kalkulua eta datu-basearen eskema erakusteko.
* `.taula-edukiontzia`: `overflow-x: auto` dauka, mugikorrean taulak pantailatik ez ateratzeko.
* `.gako-pk`, `.gako-fk` eta `.etiketa-prezioa`: Datu-baseko tauletan `PK` (laranja) eta `FK` (urdina) txarteltxoak margotzeko.
* `.formulario-kutxa` eta `.eremu-multzoa`: Kontaktuko formularioa eta bere input-ak txukuntzeko.

### 3. IDak (`#id`)
IDak behin bakarrik agertzen diren elementuetan erabili ditugu:
* `#formularioa`: `kontaktua.html` orrian formularioaren atalari jarri diogu, goiko botoitik zuzenean bertara salto egiteko.
* `#izena`, `#email`, `#atala`, `#mezua`: Formularioko input bakoitza bere `<label for="...">` etiketarekin lotzeko.

### 4. Mugikorrerako egokitzapena (`@media`)
CSS fitxategiaren bukaeran `@media (max-width: 768px)` araua gehitu dugu. Pantaila txikietan (adibidez Sareak atalean mugikorretik Wi-Fi proba egitean), `.nav-bar` eta `.hero-kutxa` zutabean jartzen dira (`flex-direction: column`) dena ondo irakurtzeko.

