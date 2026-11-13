# Research: Stufenlose, leise Drehzahlregelung von 230-V-AC-Saunalüftern

> Issue: [#12](https://github.com/zisami/sauna-O-matic/issues/12) · Stand: 13.11.2026
>
> **Frage:** Wie lassen sich zwei vorhandene, hitzebeständige 230-V-AC-Lüfter stufenlos und leise in der Drehzahl regeln?
>
> **Kontext:** Steuerung über M5Stack StamPLC (ESP32-S3, RS485/Modbus, Hutschiene) im Werkstattverteiler, ca. 5 m entfernt. Lüftermodell unbekannt.

Kennzeichnung: Aussagen ohne Quellenverweis und mit *(Allgemeinwissen)* oder *(Schlussfolgerung)* markiert sind nicht durch eine Primärquelle belegt.

## Kurzantwort

- **Zuerst den Motortyp bestimmen.** Davon hängt die Regelmethode ab. Siehe [Anleitung](#anleitung-motortyp-bestimmen). Ein EC-Lüfter regelt sich selbst über 0–10 V, PWM oder Modbus. Ein AC-Lüfter braucht einen externen Regler [S2].
- **Typische Saunalüfter** in der Bauform 120 × 120 × 38 mm, 230 V, ca. 20 W [S11][S12] entsprechen äußerlich AC-Kompaktlüftern wie dem ebm-papst 4650 N. Dieser hat einen **Außenläufer-Spaltpolmotor** [S13][S14]. Ob die eigenen Lüfter so gebaut sind, ist *nicht verifiziert*.
- **Die meisten AC-Lüfter sind spannungsregelbar**, entweder per Transformator, TRIAC (Phasenanschnitt) oder Frequenzumrichter [S1].
- **Phasenanschnitt (TRIAC)** ist stufenlos, günstig und in Hutschienenform mit 0–10 V und Modbus erhältlich. Der Regler selbst ist leise, aber bei niedriger Drehzahl kann der Motor **brummen**. Abhilfe: die Mindestspannung anheben [S1].
- **Transformatorregler** liefern einen reinen Sinus und laufen damit am leisesten. Sie sind aber **gestuft** (meist 5 Stufen), und der Trafo kann selbst brummen [S1][S5].
- **Frequenzumrichter mit Sinusfilter** sind sehr leise, aber teuer und aufwendig. Für kleine 1~-Lüfter mit ca. 20 W ist das überdimensioniert [S1][S6] *(Schlussfolgerung)*.
- **Empfehlung für AC-Lüfter:** ein Hutschienen-TRIAC-Regler **Sentera MVS-1-15CDM** (1,5 A, 0–10 V oder **Modbus RTU**, Min/Max-Trimmer, Kickstart) für beide Lüfter gemeinsam. Das passt, wenn der Gesamtstrom ≤ 1,5 A ist; 2 × ca. 20 W ergeben ca. 0,2 A *(Schlussfolgerung)* [S3][S4].
- **ESP32-Anbindung, bevorzugt:** Modbus RTU über die eingebaute RS485-Schnittstelle des StamPLC [S7]. So ist keine analoge Leitung über 5 m nötig.
- **ESP32-Anbindung, alternativ:** M5Stack **Unit DAC2** (2 × 0–10 V, I²C, Grove) [S8]. Der StamPLC hat **keinen Analogausgang** [S7].
- **Mindestdrehzahl und Anlauf:** Am Regler eine Mindestspannung einstellen, damit der Lüfter sicher anläuft und nicht brummt. Kickstart (kurz volle Spannung beim Start) hilft beim Anlauf [S1][S3]. Den konkreten Wert durch Ausprobieren ermitteln.
- **Thermik beachten:** Bei niedriger Drehzahl kühlt sich der Motor schlechter. Saunalüfter sind teils nur bis +55…+80 °C Umgebungstemperatur spezifiziert [S1][S11][S12][S14].
- **Alternative:** Die Lüfter durch EC-Lüfter ersetzen. Dann entfällt der Leistungsregler, und die Ansteuerung erfolgt direkt per 0–10 V, PWM oder Modbus [S2]. Saunataugliche EC-Lüfter wurden *nicht recherchiert*.

## Matrix Motortyp × Regelmethode

Legende: ++ sehr gut · + gut · o eingeschränkt · – ungeeignet / nicht anwendbar

| Motortyp | Phasenanschnitt (TRIAC) | Trafo-Stufenregler | Elektronischer Spannungsregler mit Sinus / FU mit Sinusfilter | Direkte Ansteuerung 0–10 V / PWM |
|---|---|---|---|---|
| **Spaltpol** (kleine Kompaktlüfter) | **Eignung +**: spannungsregelbar *(Allgemeinwissen; vom Hersteller für den konkreten Lüfter zu bestätigen)*. **Geräusch o**: Brummen bei kleiner Spannung möglich [S1]. **Kosten** niedrig [S5] | **Eignung +**, aber gestuft. **Geräusch ++** am Motor; der Trafo kann brummen [S1][S5]. **Kosten** mittel | **Eignung o**: technisch möglich, für ca. 20 W unwirtschaftlich *(Schlussfolgerung)*. **Geräusch ++** [S1][S6]. **Kosten** hoch | **–** (keine Elektronik im Motor) |
| **Kondensator** (PSC) | **Eignung +**: Standardfall für Spannungsregler bei Außenläufern [S5]. **Geräusch o**: Brummen bei niedriger Drehzahl, mit Min-Spannung reduzierbar [S1]. **Kosten** niedrig | **Eignung ++**. **Geräusch ++**: „ohne elektromagnetische Motorgeräusche“ [S5]. Gestuft. **Kosten** mittel | **Eignung o/+**: Ziehl-Abegg 1~ Fcontrol mit allpoligem Sinusfilter [S6]. Ob er für den konkreten Lüfter passt, ist offen. **Geräusch ++**. **Kosten** hoch | **–** |
| **EC** (elektronisch kommutiert) | **–**: Phasenanschnitt am EC-Netzteil ist nicht vorgesehen *(Allgemeinwissen)* | **–** | **–** | **Eignung ++**: integrierter Regler mit 0–10 V, PWM oder Modbus [S2]. **Geräusch ++**. **Kosten**: Signalgeber ab ca. 39 € [S10] |

## Produkttabelle

Preise inkl. MwSt., abgerufen am 13.11.2026. „a. A.“ bedeutet „auf Anfrage“ oder „kein Preis gefunden“.

| Produkt | Typ / Montage | Eingang | Strom | Hinweise | Preis | Shop |
|---|---|---|---|---|---|---|
| **Sentera MVS-1-15CDM** | TRIAC-Phasenanschnitt, **Hutschiene** IP20 | 0–10 V / 0–20 mA, **Modbus RTU** | 1,5 A | Min/Max-Trimmer, Kickstart/Softstart, Fernstart, ungeregelter 230-V-Ausgang [S3][S4] | a. A. | [sentera.eu](https://www.sentera.eu/de/produktdetails/drehzahlsteller-0-10-v-steuereingang-din-schiene-15-a/119383) |
| Sentera MVS-1-30CDM / -60CDM / MVS-1100CDM | wie oben | wie oben | 3 / 6 / 10 A | wie oben [S4] | a. A. | [sentera.eu](https://www.sentera.eu/de/produkte/elektronische-drehzahlregler/drehzahlregelung-%C3%BCber-analogeingang/drehzahlregler-f%C3%BCr-einphasenmotoren-din-schienenmontage/837) |
| Sentera EVS-1-15-DM | TRIAC, Aufputz IP54 | 0–10 V, Modbus RTU | 1,5 A | Funktionen wie MVS [S9] | a. A. | [sentera.eu](https://www.sentera.eu/de/produktdetails/drehzahlregler-steuereingang-0-10-v-6-a/117560) |
| Ziehl-Abegg PKE-2.5 (303600) | Phasenanschnitt „Acontrol“, Aufputz | Poti / 0–10 V | 2,5 A | [S5] | a. A. | [ziehl-abegg.com](https://www.ziehl-abegg.com/produkte/elektronische-spannungsregelgeraete) |
| Ziehl-Abegg PXET6Q (303612) | Phasenanschnitt, IP54 | internes Poti / 0–10 V | 6 A | „stufenlose Drehzahlregelung von spannungsregelbaren 1~ Ventilatoren“ [S15] | a. A. | [ventos.de](https://www.ventos.de) |
| Ziehl-Abegg R-E-1.5G (302001) | Trafo 5-stufig | Schalter, teils 0–10 V | 1,5 A | „Regelung ohne elektromagnetische Motorgeräusche“ [S16] | a. A. | [ziehl-abegg.com](https://www.ziehl-abegg.com/produkte/transformatorische-steuergeraete) |
| Ziehl-Abegg 1~ Fcontrol | FU mit Sinusfilter, IP54 | 0–10 V | 4–10 A | Kein geschirmtes Kabel nötig [S6] | a. A. | [ziehl-abegg.com](https://www.ziehl-abegg.com/produkte/frequenzumrichter) |
| Helios ESA 3 (00239) | Phasenanschnitt, Aufputz IP40 | Drehknopf (manuell) | 2,5 A | Mindestspannung einstellbar; **kein** Steuereingang, nur als Referenz [S17] | **241,47 €** | [elektrikshop.at](https://elektrikshop.at/shop/Haustechnik/Ventilatoren/Elektronischer-Aufputz-Drehzahlsteller::721128.html) (AT) |
| M5Stack Unit DAC2 (U012-B) | I²C-DAC, Grove | I²C | – | 2 × 0–5/0–10 V, 15 bit, nicht galvanisch getrennt [S8] | a. A. | [m5stack.com](https://docs.m5stack.com/en/unit/Unit-DAC2) |
| Systemair MTP (EC-Regler) | 0–10-V-Signalgeber für EC | Poti | – | nur für EC-Lüfter [S10] | **38,90 €** | aktivluft.de (DE) |

**Saunalüfter am Markt (zur Einordnung):**

| Produkt | Daten | Regelbarkeit | Preis | Shop |
|---|---|---|---|---|
| EOS Lüftungsventilator 94 5774 | 230 V 1N~, 22 W, 160 m³/h, 120×120×38 mm, IP55, −10…+70 °C, kugelgelagert [S11] | nicht angegeben | a. A. | polarholz-sauna.de (DE) |
| Eliga Saunalüfter (Art. 85904) | 230 V, 23 W, 178 m³/h, 45 dB(A), IP55, −40…+80 °C, 120×120×38 mm [S12] | nicht angegeben | **31,90 €** | [hitl-shop.de](https://www.hitl-shop.de) (DE) |
| MMotors MM-S Ø100 | 17 W, 105 m³/h, 39 dB, bis 140 °C, IPX4 [S18] | nicht angegeben | **68,00 €** | mmotors.shop |
| *Referenz:* ebm-papst 4650 N | 230 V 50 Hz, 19 W, 2650 min⁻¹, 160 m³/h, 46 dB(A), −10…+55 °C, **Spaltpol-Außenläufer** [S13][S14] | Spannungsregelung *nicht verifiziert* | – | – |

## Anleitung: Motortyp bestimmen

**Sicherheit:** Vor jeder Prüfung allpolig spannungsfrei schalten. Arbeiten am 230-V-Netz nur durch eine Elektrofachkraft *(Allgemeinwissen)*.

1. **Typenschild ablesen.** Hersteller, Typ, Spannung, Leistung (W), Strom (A) und eventuell eine Kondensatorangabe (µF). Mit dem Typ das Datenblatt suchen. Dort steht der Motortyp, z. B. „shaded-pole“ oder „Spaltpol“ beim 4650 N [S14].
2. **Anschlüsse zählen.** Zwei Anschlüsse plus PE und kein Kondensator deuten auf einen Spaltpolmotor hin [S14]. Drei oder mehr Adern oder ein separater Kondensator (Zylinder mit µF-Angabe) bedeuten einen Kondensatormotor *(Allgemeinwissen)*.
3. **Elektronik prüfen.** Zusätzliche Steueradern (0–10 V, PWM, Tacho oder Modbus-Klemmen) oder „EC“ auf dem Typenschild bedeuten einen EC-Motor [S2].
4. **Bauform beachten.** Kompaktlüfter 120 × 120 × 38 mm mit zwei Flachsteckern und ca. 20 W sind typischerweise Spaltpol [S13][S14] *(Schlussfolgerung, im Einzelfall prüfen)*.
5. **Strom messen** (Zangenamperemeter): Das Ergebnis bestimmt die Reglergröße. Die Summe beider Lüfter muss unter dem Reglernennstrom liegen [S3].
6. **Regelbarkeit klären.** Im Datenblatt nach „spannungsregelbar“ oder „voltage controllable“ suchen. Fehlt die Angabe, beim Hersteller oder Händler nachfragen.

## ESP32-Anbindung (StamPLC)

| Weg | Hardware | Vorteile | Nachteile |
|---|---|---|---|
| **Modbus RTU** (empfohlen) | RS485 des StamPLC (PWR-485, SIT3088) [S7] → Modbus-Eingang MVS/EVS [S3] | digital, störfest über 5 m, Rücklesen möglich, keine Zusatzhardware | Registerbelegung aus dem Sentera-Handbuch nötig; der StamPLC ist ab Werk Modbus-*Slave* [S7], die eigene Firmware muss Master sein *(Schlussfolgerung)* |
| **0–10 V analog** | Unit DAC2 an Grove PORT.A/PORT.C (I²C) [S7][S8] | herstellerunabhängig, für jeden 0–10-V-Regler geeignet | nicht galvanisch getrennt [S8]; analoges Signal über 5 m; I²C-Adresse 0x59 kollidiert eventuell mit dem AW9523B im StamPLC (andere Bus-Instanz?) [S7][S8] |

## Offene Punkte

- **Lüftermodell, Typenschild und Strom** der eigenen zwei Lüfter sind unbekannt. Ohne diese Daten bleibt die Motortyp-Zuordnung eine Annahme.
- **Max. Umgebungstemperatur** der eigenen Lüfter im Vergleich zur tatsächlichen Ablufttemperatur im unteren Drittel. Das gilt besonders für den Betrieb bei niedriger Drehzahl.
- **Preise** für Sentera MVS/EVS und Ziehl-Abegg: kein öffentlicher Preis gefunden, Angebot bei einem Händler (AT/DE) einholen.
- **Sentera-Modbus-Registerbelegung** und Datenblatt-PDF konnten nicht geladen werden; mit dem Handbuch nachziehen.
- **Ob ebm-papst-ähnliche Spaltpol-Kompaktlüfter mit Phasenanschnitt sauber laufen** (Mindestspannung, Brummen), ist herstellerseitig nicht belegt. Am besten mit einem Testaufbau prüfen.
- **Eignung des Ziehl-Abegg 1~ Fcontrol** für kleine 1~-Lüfter wurde nicht verifiziert.
- **DAC2-I²C-Adresse** gegen die StamPLC-interne Belegung prüfen (nur relevant für die 0–10-V-Variante).
- **Nicht recherchiert:** Harvia- und Sentiotec-Lüfter, saunataugliche EC-Lüfter, Hutschienen-Dimmer von Eltako/Finder/Kemo (meist für Lampen; Motoreignung offen), Phasenanschnitt-SSR (Carlo Gavazzi, Crydom).

## Quellen

Alle abgerufen am 13.11.2026.

- [S1] Sentera: AC-Drehzahlregelung. https://fan-speed-control.eu/de/ac-drehzahlregelung
- [S2] Sentera: Technologie (EC vs. AC). https://fan-speed-control.eu/de/technologie
- [S3] Sentera: MVS-1-15CDM Produktseite. https://www.sentera.eu/de/produktdetails/drehzahlsteller-0-10-v-steuereingang-din-schiene-15-a/119383
- [S4] Sentera: Drehzahlregler für Einphasenmotoren, Hutschienenmontage (MVS-Serie). https://www.sentera.eu/de/produkte/elektronische-drehzahlregler/drehzahlregelung-%C3%BCber-analogeingang/drehzahlregler-f%C3%BCr-einphasenmotoren-din-schienenmontage/837
- [S5] Ziehl-Abegg: Elektronische Spannungsregelgeräte. https://www.ziehl-abegg.com/produkte/elektronische-spannungsregelgeraete
- [S6] Ziehl-Abegg: Frequenzumrichter. https://www.ziehl-abegg.com/produkte/frequenzumrichter
- [S7] M5Stack: StamPLC Dokumentation. https://docs.m5stack.com/en/core/StamPLC
- [S8] M5Stack: Unit DAC2 Dokumentation. https://docs.m5stack.com/en/unit/Unit-DAC2
- [S9] Sentera: EVS Drehzahlregler 0–10 V. https://www.sentera.eu/de/produktdetails/drehzahlregler-steuereingang-0-10-v-6-a/117560
- [S10] Systemair MTP/MTV (systemair.com, de-at). Preis von aktivluft.de, Produkt „EC-Regler MTP“.
- [S11] Polarholz Sauna: EOS Lüftungsventilator 94 5774. polarholz-sauna.de
- [S12] HITL-Shop: Eliga Lüftungsventilator für die Sauna, Art. 85904, mit Datenblatt. https://www.hitl-shop.de/artikelbilder/dokumente/datenblatt_85904_ventilator.pdf
- [S13] ebm-papst: 4650 N Produktseite. https://www.ebmpapst.com/us/en/products/compact-fans/axial-compact-fans/p/4650N.html
- [S14] ebm-papst: 4650 N Datenblatt („external rotor shaded-pole motor“, 2 Flachstecker), zitiert über die Suchmaschinen-Auszüge von alldatasheet.com und equipspares.com. Original: https://img.ebmpapst.com/products/datasheets/AC-axial-fan-4650N-ENU.pdf
- [S15] ventos.de: Ziehl-Abegg PXET6Q (Händlerangaben).
- [S16] Ziehl-Abegg: Transformatorische Steuergeräte. https://www.ziehl-abegg.com/produkte/transformatorische-steuergeraete
- [S17] elektrikshop.at: Helios ESA 3 Elektronischer Aufputz-Drehzahlsteller. https://elektrikshop.at/shop/Haustechnik/Ventilatoren/Elektronischer-Aufputz-Drehzahlsteller::721128.html
- [S18] mmotors.shop: MMotors MM-S Saunalüfter Ø100.
