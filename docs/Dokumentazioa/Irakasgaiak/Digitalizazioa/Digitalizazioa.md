2. Digitalizazioa: Kontzeptuak, IT/OT Konbergentzia, TGD/THD, Cloud Computing eta GitHub Kudeaketa

Produkzio-sektoreetan aplikatutako digitalizazioak gaur egungo teknologia- eta azpiegitura-proiektuen oinarria osatzen du. Atal honetan, proiektu honetan erabilitako kontzeptu tekniko nagusiak eta hodeiko baliabideen bidez garatutako planifikazioaren kudeaketa biltzen dira.

---

## 2.1. Digitalizazioaren Kontzeptu Nagusiak

### 1. Digitalizazioa eta Transformazio Digitala
Digitalizazioa prozesu analogikoak edo fisikoak formatu digitalera igarotzeko eta optimizatzeko ekintza da. Sektore produktiboan, digitalizazioak prozesuen eraginkortasuna hobetzea, erroreak murriztea, datuen bidezko erabakiak hartzea eta erantzun-gaitasuna azkartzea du helburu.

### 2. IT eta OT Teknologien Konbergentzia (IT/OT Convergence)
Gaur egungo azpiegitura teknologikoetan ezinbestekoa da Informazioaren Teknologien (**IT**) eta Operazioen Teknologien (**OT**) arteko integrazioa:

* **IT (Information Technology):** Enpresa-logika, datu-baseak, softwarea, cloud inguruneak eta informazioaren biltegiratze zein analisia kudeatzen ditu. Lehentasun nagusiak datuen konfidentzialtasuna, osotasuna eta eskuragarritasuna dira.
* **OT (Operational Technology):** Hardware fisikoa, kableatzeak, sentsoreak, switch-ak, PLC-ak eta industriako gailuen zuzeneko kontrola kudeatzen ditu. Lehentasuna denbora errealean funtzionatzea eta azpiegituraren jarraitutasuna zein segurtasuna (*safety*) bermatzea da.

| Ezaugarria | IT (Informazioaren Teknologiak) | OT (Operazioen Teknologiak) |
| :--- | :--- | :--- |
| **Funtzioa** | Datuak kudeatzea, prozesatzea eta biltegiratzea | Gailu fisikoak, sentsoreak eta kableatzea kontrolatzea |
| **Gailu Nagusiak** | Zerbitzariak, IIS, Datu-baseak (Access), Cloud | Switch-ak, Patch Panel-ak, UTP kableak, AP-ak |
| **Integrazioa Proiektuan** | Webgune estatikoa, streaming DB eta Windows Server | Cabling fisikoa, OSI 1/2 mailak eta Wi-Fi sarrera-puntua |

**Integrazioaren garrantzia:** Proiektu honetan, kableatze fisikoa (OT) eta Windows Server / IIS zerbitzaria zein datu-basea (IT) lotzen dira, azpiegitura osoak gailu mugikorren bidez komunikatzeko ahalmena izateko.

![IT eta OT Konbergentzia](/Dokumentazioa/img/it-ot.jpg)

*1. irudia: IT eta OT teknologien arteko konbergentzia eta integrazioa.*
### 3. TGD-ak (Teknologia Garatu Berriak / Transformazio Digitaleko Gakoak) eta THD
* **TGD-ak (Teknologia Garatu eta Disruptiboak):** Sektore produktibo berrietan integratzen diren teknologia aurreratuak dira (IoT, Cloud Computing, Zibersegurtasuna eta Datu Masiboen Analisiak).
* **THD (Total Harmonic Distortion - Distortsio Harmoniko Osoa):** Sare elektriko zein komunikazio-seinaleetan agertzen den zarata edo desbideratze neurgailua da. THD maila altu batek komunikazio fisikoan (OSI 1 mailan) interferentziak sortzen ditu, datu-transmisioaren kalitatea kaltetuz. Proiektuko kableatze eta modulazio probetan seinalearen kalitatea bermatu behar da distortsio hau prebenitzeko.

### 4. Cloud Computing (Hodeiko Konputazioa)
Hodeiko konputazioari esker, baliabide informatikoak (zerbitzariak, biltegiratzea, datu-baseak eta softwarea) internet bidez atzitu eta kudeatu daitezke, tokiko azpiegituren mende egon gabe.

* **Ereduak:**
  * **IaaS (Infrastructure as a Service):** Zerbitzari birtualak eta biltegiratzea (adib. Windows Server hodeiko ingurunean).
  * **PaaS (Platform as a Service):** Garapen eta publizazio plataformak (adib. GitHub Pages).
  * **SaaS (Software as a Service):** Hodeiko aplikazioak eta kudeaketa tresnak (adib. GitHub Projects).
* **Proiektuko Abantailak:** Bertsio-kontrola, datuen erredundantzia, talde-lan sinkronizatua eta urruneko atzipen babestua.
