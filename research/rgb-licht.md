# RGB-Licht in der Saunakabine

> Recherche zu Issue #16 – Stand 13.11.2026
> Frage: Welche RGB-Lichtlösungen sind in einer Saunakabine dauerhaft hitzetauglich, und wie werden sie von der Hauptschalteinheit (M5Stack StamPLC), der Bedieneinheit (Tab5) und Home Assistant angesteuert?

Rahmen: private Sauna in Österreich, Kabine 2,2 × 3,1 × 2,6 m (Holz). Die StamPLC sitzt im Verteilerkasten in der Werkstatt, rund 5 m von der Kabine entfernt. Weißes Saunalicht ist vorhanden, der Typ ist unbekannt.

## Kurzantwort

- **Am besten passt ein 24-V-RGBW-Silikonstrip** mit Umgebungstemperatur bis 125 °C, z. B. Cariitti Sauna Ultra Ambient RGBW: −40…+125 °C, IP67, Kabel bis 200 °C, 132 €/m [1]. Netzteil und Dimmer kommen **außerhalb** der Kabine unter. Cariitti-Netzteile sind nur bis Ta 45 °C spezifiziert [4].
- **Sauna-Glasfaser-Sets gibt es nur mit hitzefesten Glasfasern (180 °C)** [5][6]. Der Projektor darf laut Cariitti auch in der Kabine sitzen, unter der Bank in max. 80 cm Höhe [5][6][7]. Die Fasern sind 1,4–4 m lang und dürfen nicht gekürzt werden [5]. Ein Projektor im 5 m entfernten Verteilerkasten ist damit **nicht** möglich, er muss direkt an oder in die Kabine.
- **RGBW-Sternenhimmel kostet ab ca. 970 €**: Cariitti-Set mit 11 Fasern und BLE-Projektor [5]. Der weiße Starry-Sky-Satz VPAC-1527-CE75 kostet 874 € [6]. Der Cariitti-Projektor RGBW-DMX allein kostet 532 € [8].
- **Die Steuerung der Herstellersysteme ist proprietär oder busbasiert.** Cariitti setzt primär auf Casambi/BLE [3][9], alternativ DALI DT6 [4] oder DMX [8]. Sentiotec-RGBW-Spots laufen an einem 4×350-mA-Konstantstrom-**DMX**-Treiber [10]. EOS-Strips brauchen ein eigenes Netzteil und einen eigenen Controller [11].
- **Für die StamPLC + ESPHome empfiehlt sich Modbus RTU.** Ein 4-Kanal-LED-Dimmer für die DIN-Schiene am RS485-Port der StamPLC (z. B. Wiren Board WBPRO-LEDDIM, 4×5 A, bis 48 V, offene Registertabelle) [12][15] lässt sich über ESPHome `modbus_controller` als `rgbw`-Licht einbinden [16][17]. Damit sind Home Assistant und Tab5 automatisch abgedeckt.
- **Alternative ohne Bus:** PWM direkt von der StamPLC über ESPHome `ledc` mit externer MOSFET-Endstufe. Der ESP32-S3 hat 8 LEDC-Kanäle [18]. Die Relais der StamPLC sind für PWM ungeeignet [15], und freie GPIOs sind knapp (siehe offene Punkte).
- **DMX512 und DALI sind ungünstig:** ESPHome hat keine native DMX- oder DALI-Komponente [19]. DMX bräuchte außerdem den einzigen RS485-Port der StamPLC exklusiv, was mit Modbus kollidiert [15].
- **Der Spannungsfall über 5 m ist bei 24 V unkritisch.** Bei 4 m RGBW-Strip unter Volllast (2 A) sind es ca. 0,47 V (≈ 2 %) mit 0,75 mm² und ca. 0,24 V (≈ 1 %) mit 1,5 mm². Bei 12 V wäre es das Doppelte an Strom und damit etwa das Vierfache an relativem Spannungsfall (Rechnung unten).
- **Ob das vorhandene weiße Licht dimmbar ist, hängt vom Typ ab.** 230-V-Projektoren wie der Cariitti VPAC werden „über die Versorgungsspannung“ gedimmt, also per Phasenabschnitt [7]. 24-V-Strips werden per PWM gedimmt [1][14]. EOS-Strips sind laut Händler „spannungsbasiert dimmbar“ [11]. Für 230 V gibt es einen Modbus-Phasendimmer auf der DIN-Schiene (WB-MDM3, Abschnitt/Anschnitt, 180 W LED pro Kanal, 98 €) [13].
- **Montageort:** Strips eignen sich hinter Rückenlehnen, unter Bänken oder an der Decke, an der Decke aber nur mit einem Produkt für 125 °C [1]. EOS-LEDs schalten bei 110 °C einzeln ab [11]. Spots wie der Sentiotec RGBW sind IP65 und laut Hersteller auch direkt über dem Ofen montierbar [10].
- **Kostenrahmen Strip-Variante** (4 m, ohne Montage): ca. 530 € Strip [1] plus ca. 115 € Modbus-Dimmer [12] plus 24-V-Netzteil. Das Netzteil ist noch nicht recherchiert, Cariitti verlangt für sein DALI-Netzteil 172 € [4].

## Variantenvergleich

| Variante | Hitzetauglichkeit | Optik | Ansteuerung | Kosten (Stand 13.11.2026) |
|---|---|---|---|---|
| **A – Cariitti Sauna Ultra Ambient RGBW** (24 V, RGB + 2700 K) | Ta −40…+125 °C, Silikon bis 180 °C und vergilbungsfrei, Kabel bis 200 °C, IP67 [1] | Linienlicht, RGBW, Ra 90; Profil SIDE 12×6 mm oder TOP 14×14 mm; kürzbar alle 50 bzw. 62 mm [1] | 4 Kanäle PWM, Konstantspannung 24 V; Cariitti-Zubehör: Casambi-PWM-Dimmer [3] oder DALI-Netzteil [4]; jeder 24-V-PWM-Dimmer passt | 132 €/m [1] |
| **B – Generischer Sauna-Strip** (z. B. Lichtdekor 9132, nur weiß verifiziert) | bis 125 °C, IP67 (Shop-Angabe; Bildtexte nennen 105 °C, widersprüchlich) [14] | COB, 3000 K, 10 W/m, Ra > 94 [14] | 24 V, dimmbar [14]; RGB-Varianten gibt es, sind aber nicht verifiziert | ab 189,94 € inkl. 19 % MwSt. (6 m, inkl. Trafo/Controller nach Wahl) [14] |
| **C – EOS LED Strip RGB** | Abschaltung je LED bei 110 °C; Deckenmontage erlaubt; nicht für Dampfbad [11] | Segmente 165/330/495 mm, bis 12 in Reihe [11] | 24 V, 3,6–4 W pro Segment; braucht EOS-Netzteil und -Controller [11] | Preis auf Anfrage [11] |
| **D – Sentiotec RGBW Spot** (18 W eckig / 5 W rund) | Alu und Hartglas, IP65, auch über dem Ofen montierbar, 2 m hitzefestes Kabel [10] | Punktlicht, RGB + 3000 K [10] | Konstantstrom 4×350 mA über **DMX**-Treiber [10] | nicht ermittelt |
| **E – Glasfaser-Sternenhimmel RGBW** (Cariitti RGBW-BLE-G211) | Glasfasern bis 180 °C; Projektor IP65 ohne Lüfter, in der Kabine unter der Bank bei ≤ 80 cm zulässig [5] | Sternenpunkte (ø 2 mm) plus eine ø-4-mm-Faser mit Linse Richtung Ofen; Sets mit 11/17/23/29 Fasern [5] | Casambi/BLE (Taster + App) [5]; DMX-Projektor separat erhältlich [8] | 970 € (11 Fasern) [5]; RGBW-DMX-Projektor allein 532 € [8] |
| **F – Glasfaser-Sternenhimmel weiß** (Cariitti VPAC-1527-CE75, Referenz) | 75 Glasfasern ø 1 mm bis 180 °C; laut Cariitti das einzige Starry-Sky-Modell für Saunahitze [6] | 2700 K, für 1–4 m² Decke [6] | 230 V, gedimmt über die Versorgungsspannung [6][7] | 874 € [6] |

Anmerkungen:

- Cariitti-Preise stammen aus dem finnischen Shop cariitti.fi. Ob sie für AT gelten und welche MwSt. enthalten ist, ist nicht geprüft.
- Bei den Glasfaser-Sets ist die Faserlänge der begrenzende Faktor. Beim 11-Faser-Set sind es 4×1,4 m, 4×1,7 m, 2×2,0 m und 1×4,0 m, und kürzen ist nicht erlaubt [5]. Der Projektor gehört daher hinter die Kabinenwand oder unter die Bank, nicht in den Verteilerkasten.
- Der Projektor RGBW-DMX hat laut Datenblatt 230 V, 19 W, IP65/IP20, RGB + W 7600 K und ein elektronisches Vorschaltgerät [8]. Für welche Teile die Angaben IP65 und IP20 jeweils gelten und welche Montagegrenzen in der Kabine gelten, ist nicht angegeben.

## Ansteuerungs-Optionen

Ausgangslage StamPLC [15]:

- ESP32-S3 (Stamp-S3A)
- 4 Relais, für PWM ungeeignet
- 8 Opto-Eingänge
- 1× RS485 (SIT3088) und 1× CAN
- PORT.A (G1/G2), PORT.C (G4/G5) und GPIO.EXT (u. a. G40/G41)
- Versorgung 6–36 V DC, Betriebstemperatur 0–40 °C
- ESPHome/Home-Assistant-Integration dokumentiert

| Option | ESPHome-Unterstützung | Ressource an der StamPLC | Bewertung |
|---|---|---|---|
| **Modbus-RTU-LED-Dimmer** (z. B. WBPRO-LEDDIM: 4×5 A, bis 48 V DC, PWM 100 Hz–24 kHz, RGB/RGBW/CCT, DIN 2TE, 9–28 V) [12] | `modbus_controller` + Output → `rgbw`-Licht [16][17] | RS485 (Bus, mehrere Teilnehmer möglich) | **Empfohlen.** Läuft im Verteilerkasten, 24 V für Strip und Dimmer gemeinsam, kein Funk. Preis 115,77 €, aktuell „Not available“ [12] |
| **Modbus-Phasendimmer 230 V** für vorhandenes weißes Licht (WB-MDM3: 3 Kanäle, Anschnitt/Abschnitt, 180 W LED pro Kanal, DIN 3TE, −40…+80 °C) [13] | wie oben | RS485 (gleicher Bus) | Sinnvoll, falls das weiße Licht ein 230-V-Phasendimmer-Typ ist. 98,18 €, lagernd [13] |
| **Modbus-PWM-Signalmodul** (Waveshare 4CH, Logikpegel 3,3/5 V, ≤ 30 mA, 1 Hz–200 kHz) [20] | wie oben | RS485 | Nur Steuersignal, braucht zusätzliche MOSFET-Endstufen. ca. 22 USD [20] |
| **Direkt-PWM** über ESPHome `ledc` + MOSFET-Board | `ledc` (8 Kanäle, 14 bit; empfohlen z. B. 1220/2441/4882 Hz) [18] → `rgbw` [16] | 4 freie GPIOs (PORT.A/PORT.C bzw. EXT) | Günstig, aber die GPIOs könnten anderweitig belegt sein (z. B. PORT.A als I²C). Eigenbau-Endstufe nötig |
| **DMX512** (DIN-Decoder oder Sentiotec-Treiber [10], Cariitti RGBW-DMX [8]) | keine native Komponente [19] | RS485 exklusiv | Kollidiert mit Modbus am einzigen RS485-Port. Nur sinnvoll, wenn Spots/Projektor zwingend DMX brauchen |
| **DALI DT6/DT8** (z. B. Cariitti DALI 100 W 4CH, DT6, Ta −20…+45 °C) [4] | keine native Komponente [19] | externes DALI-Gateway nötig | Aufwendig, kein Mehrwert gegenüber Modbus |
| **Casambi/BLE** (Cariitti CBU-PWM4: 12–24 V, 4 Kanäle, 6 A gesamt, 144 W bei 24 V, 135 €) [3] | keine | — | Herstellerweg per App; Home-Assistant/Tab5-Einbindung ungeklärt |

### Spannungsfall über ca. 5 m (24 V vs. 12 V)

Annahmen:

- 4 m RGBW-Strip mit 3 W pro Farbe und Meter [1], also 12 W pro Kanal.
- Bei 24 V sind das 0,5 A pro Kanal. Worst Case sind alle 4 Kanäle voll: 2 A auf dem gemeinsamen Pluspol.
- Konservativ gerechnet: 2 A auf Hin- und Rückleiter.
- Kupfer mit $\rho = 0{,}0178\ \Omega\cdot\text{mm}^2/\text{m}$.

$$\Delta U = \frac{2 \cdot L \cdot \rho \cdot I}{A}$$

| Spannung | Strom | 0,75 mm² | 1,5 mm² |
|---|---|---|---|
| 24 V | 2 A | 0,47 V (≈ 2,0 %) | 0,24 V (≈ 1,0 %) |
| 12 V | 4 A | 0,95 V (≈ 7,9 %) | 0,47 V (≈ 4,0 %) |

Fazit: Bei **24 V** reicht eine Leitung mit 5 × 0,75–1,5 mm² zur Kabine. 12 V ist über diese Distanz unnötig verlustbehaftet. Der Spannungsfall *im* Strip selbst (maximale Einspeiselänge) ist nicht berücksichtigt (siehe offene Punkte).

## Offene Punkte

1. **Typ des vorhandenen weißen Saunalichts.** Davon hängt die Dimmart ab:
   - 230 V Phasenabschnitt → WB-MDM3
   - 12/24 V Konstantspannung → PWM-Kanal
   - Konstantstrom 350/700 mA → CC-Treiber mit Dimmeingang

   Vor Ort Typenschild bzw. Treiber prüfen.
2. **Freie GPIOs und RS485-Belegung der StamPLC.** Wird RS485 schon für andere Geräte gebraucht? Modbus mit mehreren Teilnehmern ist unproblematisch, DMX nicht.
3. **WBPRO-LEDDIM ist „Not available“** [12]. Lieferbare Alternative mit 4-Kanal-Modbus-RTU-CV-Dimmer für die DIN-Schiene in AT/DE ist noch nicht verifiziert.
4. **Maximale Einspeiselänge bzw. Verbindungsart des Cariitti-Ultra-Ambient-Strips:** Die Installationsanleitung ist nicht ausgewertet.
5. **Cariitti RGBW-DMX:** Unklar sind die Montagegrenzen in der Kabine und die DMX-Kanalbelegung [8]. Offen ist auch, ob ein Cariitti-Projektor mit PWM- oder 0–10-V-Eingang existiert.
6. **Sentiotec RGBW Spot:** Unklar, ob er statt am DMX-Treiber an einem generischen 4-Kanal-CC-PWM-Treiber betrieben werden darf [10]. Preis nicht ermittelt.
7. **Harvia RGBWS2020CV** („LED-option for sauna room“): Datenblatt und Preis nicht auslesbar (JS-Seite, Shop leitet um) [21].
8. **EOS LED Strip:** Preis, Controller-Schnittstelle und ob das auf der EOS-Herstellerseite steht (Cookie-Wall), sind offen [11].
9. **Günstige Glasfaser-Sets** aus DE (z. B. Lichtdekor „Sternenhimmel SAUNA“ bzw. „Glasfaser Sauna-Sternenhimmel RGBW“, ab ca. 198–340 € [2][14]): Fasermaterial (Glas vs. PMMA) und Temperaturfreigabe sind nicht geprüft.
10. **Montage von Netzteil und Dimmer in der Nähe der Kabine.** Cariitti-Netzteile sind bis Ta 45 °C spezifiziert [4], die StamPLC bis 40 °C [15]. Liegt der Verteilerkasten in der Werkstatt, passt das. Bei Montage direkt an der Kabinenaußenwand ist die Temperatur zu prüfen.

## Quellen

Alle Abrufe am 13.11.2026. Inhalte von Webseiten wurden nur als Daten ausgewertet.

1. Cariitti – Sauna Ultra Ambient RGBW (Produktseite und Shop-JSON): https://cariitti.fi/products/sauna-ultra-ambient-rgbw
2. Lichtdekor – Glasfaser Sauna-Sternenhimmel RGBW LED 20 Glasfasern (Preis „ab 340,45 €“ laut Querverweis auf Seite [14], Produktseite nicht ausgewertet): https://www.lichtdekor.de/innenleuchten/glasfaser-licht/sternenhimmel-mit-glasfaser-20/
3. Cariitti – Nelikanavainen BLE PWM-himmennin CBU-PWM4: https://cariitti.fi/products/nelikanavainen-ble-pwm-himmennin
4. Cariitti – Led tasajännitelähde DALI 100W 4CH: https://cariitti.fi/products/led-tasajannitelaiet-dali-100w-4ch
5. Cariitti – Sauna Lighting Color Set RGBW-BLE-G211: https://cariitti.fi/products/rgbw-ble-g211-laudevalokuitusarja
6. Cariitti – Sauna Starry Sky VPAC-1527-CE75: https://cariitti.fi/products/vpac-1527-ce75-saunavalaistuskuitusarja
7. Cariitti – VPAC led-projektori: https://cariitti.fi/products/vpac-led-projektori
8. Cariitti – RGBW-DMX (Projektor): https://cariitti.fi/products/rgbw-dmx
9. Cariitti – RGBW-BLE Projector with colour change (Suchtreffer-Auszug: „most popular … wireless BLE lighting control, used with the Casambi App“): https://cariitti.eu/products/rgbw-ble-projektori
10. Sentiotec – RGBW Spot (LED-SP-RGBW-18 / -5): https://www.sentiotec.com/de/kategorien/sentiotec-light-and-media/rgbw_spot
11. Polarholz (Händler) – EOS LED Strip: https://polarholz-sauna.de/produkt/eos-led-strip/
12. Wiren Board – WBPRO-LEDDIM: https://wirenboard.com/en/product/WBPRO-LEDDIM/
13. Wiren Board – WB-MDM3: https://wirenboard.com/en/product/WB-MDM3/
14. Lichtdekor – Sauna LED-Streifen hitzebeständig bis 125 °C, IP67, 6 m (Art. 9132-6): https://www.lichtdekor.de/led-streifen/sauna-led-streifen/led-streifen-fuer-sauna-warmweiss-6m/
15. M5Stack – StamPLC Dokumentation: https://docs.m5stack.com/en/core/StamPLC
16. ESPHome – RGBW Light: https://esphome.io/components/light/rgbw/
17. ESPHome – Modbus Controller Output: https://esphome.io/components/output/modbus_controller/
18. ESPHome – ESP32 LEDC Output: https://esphome.io/components/output/ledc/
19. ESPHome – Komponentenübersicht (Version 2026.10.2; keine DMX512- oder DALI-Komponente gelistet): https://esphome.io/components/
20. Waveshare – Modbus RTU PWM Output 4CH: https://www.waveshare.com/modbus-rtu-pwm-output-4ch.htm
21. Harvia – LED-option for sauna room RGBWS2020CV: https://www.harvia.com/en/products/RGBWS2020CV/ledoption-for-sauna-room
