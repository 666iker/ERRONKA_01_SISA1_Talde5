# SP - Sarearen Topologia eta IP Helbideratze Plana

## 1. Sarearen Mapa Fisiko eta Logikoaren Deskribapena

Dokumentu honek enpresako sarearen azpiegitura fisiko eta logikoaren definizioa eta konfigurazioa biltzen ditu. Sarearen helburua komunikazio azkarra, segurua eta fidagarria bermatzea da gailu guztien artean.

### Topologia Fisikoa
Sarearen topologia fisikoak gailuen arteko konexio materiala eta kableatua zehazten ditu:
* **Internet eta Bideratzailea (Router):** Kanpoko konexioa (ISP) bideratzaile nagusira iristen da. Bideratzaileak barne-sarearen atebide (Default Gateway) gisa jokatzen du.
* **Komutadore Nagusia (Switch):** Bideratzailea komutadore nagusira (2960 seriekoa) konektatuta dago Cat6 Ethernet kable baten bidez. Gailu kableatu guztiak komutadore honetara bideratzen dira.
* **Ekipamendu Kableatua:** Lan-tokiak (PC Zuzendaritza, PC1), Zerbitzaria (Server0), Inprimagailua (Printer0) eta Wi-Fi Ikuspuntua (AP/WRT300N) kable bidez konektatuta daude komutadorearen portuetara, egonkortasuna eta abiadura bermatzeko.
* **Haririk Gabeko Konexioak (Wi-Fi):** Gailu mugikorrak, ordenagailu eramangarriak eta bisitarien gailuak haririk gabe konektatzen dira Wi-Fi ikuspuntura.

### Topologia Logikoa
Sarearen segmentazio logikoa **192.168.1.0/24** IPv4 helbideratze-espazioaren bidez egituratu da. Maskara estandarra **255.255.255.0** da gailu guztientzat, tokiko komunikazio zuzena ahalbidetuz eta bideratzailearen bidez kanpo-irteera bermatuz.

---

## 2. IP Helbideratze Plana (Plan de Direccionamiento IP)

Beheko taulan sareko gailu guztiei esleitutako IP helbide estatikoak, azpisare-maskarak eta atebideak zehazten dira:

| Gailua (Dispositivo) | Mota / Funtzioa | IP Helbide Estatikoa | Azpisare-Maskara | Atebidea (Gateway) |
| :--- | :--- | :--- | :--- | :--- |
| **ISR4331 Bideratzailea** | Router / Atebidea | `192.168.1.1` | `255.255.255.0` | N/A |
| **2960 Komutadorea** | Switch Nagusia | `192.168.1.2` | `255.255.255.0` | `192.168.1.1` |
| **Zerbitzaria (Server0)** | Fitxategi