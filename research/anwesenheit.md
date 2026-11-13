# Anwesenheitserkennung in der Saunakabine

Recherche zu Issue #15. Stand: 13.11.2026. Preise inkl. MwSt. laut Shop an diesem Tag, sofern nicht anders angegeben; USD-Preise sind Herstellerpreise ohne Versand/Zoll.

Rahmen: Private Sauna in Österreich, Kabine 2,2 × 3,1 × 2,6 m aus Holz, ein Fenster und eine Glastür zum Vorraum (3,1 × 3,3 × 2,6 m), 100–110 °C (an der Decke mehr), hohe Feuchte nach dem Aufguss. Steuerung: M5Stack StampPLC (ESP32-S3) und Tab5 (ESP32-P4, mit Kamera) im Vorraum, Home Assistant + MQTT. Ziele: **Komfort** (Licht/Aufguss nur bei Anwesenheit) und **Sicherheit** (Alarm, wenn eine Person zu lange regungslos ist).

## Kurzantwort

- **mmWave-Radar ist das einzige Sensorprinzip, das ruhende Personen erkennt und in ESPHome nativ unterstützt wird** (LD2410/B/C, LD2450, LD2412, LD2420, Seeed MR24HPC1, MR60BHA2, MR60FDA2 u. a.) [1][2][3][4][5].
- **Kein recherchiertes Modul ist für Kabinentemperatur spezifiziert**: Seeed MR24HPC1 −20…+85 °C [6], Infineon BGT60TR13C −20…+70 °C [7], Aqara FP2 −10…+40 °C [8], MLX90640-Breakout −40…+85 °C [9]. Elektronik muss außerhalb der heißen Zone sitzen (Vorraum, oder sehr tief/hinter Holzverkleidung – Temperatur dort messen).
- **Selbst der Not-Aus-Taster der Saunahersteller ist nur bis +70 °C Umgebung ausgelegt** und wird deshalb tief (max. 100 cm) neben der Tür montiert [12][13][14] – ein Anhaltspunkt für eine mögliche Sensorposition.
- **Empfehlung Komfort**: LD2410C (günstig, ruhend/bewegt, Gate-Energien) oder LD2450 (bis 3 Ziele mit Position, Zonen) per UART an einen ESP32 mit ESPHome; Einbau im Vorraum mit Blick durch Glastür/Fenster oder tief in der Kabine hinter Holz. Hi-Link wirbt mit Durchdringung von Gehäusen [10][11], Seeed rät dagegen von Erkennung durch Glas und dünne Holzplatten ab [5] – **Durchdringung von (beschichtetem) Glas und Holz muss praktisch getestet werden**.
- **Sicherheit (Bewusstlosigkeit) ist mit Hobby-Radar nicht verlässlich lösbar**: Atem-/Herzfrequenz (MR60BHA2) funktioniert laut Hersteller nur bis 1,5 m und nur im Schlafszenario, ungenau im Sitzen [5]; Aqara FP2 ist ausdrücklich „kein medizinisches Gerät“ [8].
- **Sicherheit daher mehrstufig**: (1) Not-Ruf-/Not-Aus-Taster im Sicherheitskreis (Ofen aus + Signal) [12][13], (2) maximale Saunagang-Dauer als Timer, (3) Radar-Regel „Anwesenheit ohne Bewegung > X min“ nur als **Voralarm** an Handy/Vorraum, nie als alleinige Sicherheitsfunktion.
- **Seeed MR24HPC1** liefert eine „Bewegungsintensität“ (0 = keine Person, 1 = ruhend, >1 = Bewegung) und hat ein Badezimmer-Szenario sowie eine Bewegungsgrenze gegen Glas-/Türstörungen [3]; erkennt aber nur **eine** Person zuverlässig [15].
- **Thermopile/Wärmebild (Grid-EYE, MLX90640) ist ungeeignet**: In der Kabine ist die Luft (100 °C) heißer als der Körper (~37 °C), der Kontrast kehrt sich um und Grid-EYE misst Objekte nur bis 80 bzw. 100 °C [16]; die Sensoren selbst halten max. 85 °C [9].
- **PIR ist ungeeignet**: erkennt nur Bewegung (keine ruhenden Personen) und arbeitet ebenfalls mit Wärmekontrast – keine Primärquelle für Saunaeignung gefunden.
- **Kamera (Tab5) wird nicht empfohlen**: Nacktheit/Privatsphäre, Beschlag an Glas – keine Primärquelle, Bewertung.
- **Hersteller (EOS, sentiotec/Harvia) bieten nur manuelle Not-Ruf-/Not-Aus-Taster**, keine automatische Personenüberwachung [12][13]; Aevia Labs entwickelt einen nicht-bildgebenden Sauna-Sicherheitssensor, aber nur als Pilot ohne Daten/Preis [17].
- **Kosten**: LD2410C 7,55 € (roboter-bausatz.de, derzeit nicht lieferbar) [18], LD2410 ab 5,33 € + 2,73 € Versand (eBay.at, China) [19]; MR24HPC1 6,90 USD [6]; MR60BHA2-Kit 26,90 USD netto [20]; Not-Aus A-EMS 112,00 € (Schachermayer AT) [14]; gewerbliche Notruf-Meldeanlage 480,00 € (saunabau.at) [21].

## Vergleich der Sensorprinzipien

| Prinzip | Erkennt ruhende Person? | Komfort | Sicherheit | Montage | Temperatur | Preis |
|---|---|---|---|---|---|---|
| **mmWave 24 GHz** (LD2410C, LD2450, MR24HPC1) | Ja (Mikrobewegung/Atmung) [1][2][3] | Gut | Nur Voralarm („keine Bewegung seit X min“) | Vorraum durch Glas/Fenster oder tief hinter Holz; Glas-/Holzdurchgang testen [5][10] | Module bis ca. 85 °C (MR24HPC1) [6]; LD2410 nicht aus Primärquelle verifiziert | 3–8 € |
| **mmWave 60 GHz Vitalzeichen** (MR60BHA2, BGT60TR13C) | Ja, plus Atem-/Herzfrequenz [4][5] | Gut | Theoretisch ja, praktisch nur ≤ 1,5 m, nur Schlafszenario, nicht durch Glas/Holz [5] | 1 m über Liegefläche, 45° geneigt [5] – in Kabine nicht hitzeverträglich | BGT60TR13C −20…+70 °C [7] | ca. 27 USD (Kit) [20] |
| **Fertiger mmWave-Sensor** (Aqara FP2) | Ja, bis 5 Personen, Zonen [8] | Gut (über HomeKit Controller in HA) [8] | Sturzerkennung nur bei Deckenmontage, „kein medizinisches Gerät“ [8] | Nur Vorraum (−10…+40 °C, 0–95 % rF nicht kondensierend) [8] | −10…+40 °C [8] | EUR-Preis nicht ermittelt |
| **Thermopile-Array** (Grid-EYE 8×8, MLX90640 32×24) | Theoretisch ja (Wärmekontrast) | Schlecht | Schlecht | Sichtverbindung nötig, IR geht nicht durch Glas (Bewertung) | Objekt bis 80/100 °C [16]; Sensor bis 85 °C [9] | Grid-EYE k. A.; MLX90640 74,95 USD [9] |
| **PIR** | Nein | Schlecht (nur Bewegung) | Ungeeignet | Sichtverbindung, kein Glas | Nicht recherchiert | < 5 € (nicht verifiziert) |
| **Kamera** (Tab5) | Ja (mit Bildauswertung) | Möglich | Möglich, aber Datenschutz | Durch Glastür, Beschlag | Tab5 im Vorraum | vorhanden |
| **Not-Ruf-/Not-Aus-Taster** | Manuell | – | Standardlösung der Hersteller, Ofen über Sicherheitskreis aus [12][13] | ≤ 100 cm Höhe neben Tür [13] | −25…+70 °C (A-EMS) [14] | 112 € (A-EMS) [14] |
| **Türkontakt + Timer** | Indirekt (Tür zu = jemand drin) | Mittel | Ergänzend (max. Saunagang-Dauer) | Reed-Kontakt an Tür | Nicht recherchiert | gering |

## Produkttabelle

| Produkt | Prinzip / Frequenz | Reichweite | Schnittstelle / ESPHome | Betriebstemperatur | Preis (Shop, 13.11.2026) |
|---|---|---|---|---|---|
| Hi-Link HLK-LD2410B/C | FMCW 24 GHz, ±60° | 5 m (Hersteller) [10]; Shop: 6 m [18] | UART 256000 Bd + GPIO-Ausgang; ESPHome `ld2410`: `has_target`, `has_moving_target`, `has_still_target`, Distanz/Energie je Gate, Engineering Mode zur Kalibrierung, Timeout Standard 5 s [1] | Nicht aus Primärquelle verifiziert (Datenblatt nur auf Google Drive) | 7,55 € roboter-bausatz.de (nicht lieferbar) [18]; LD2410 5,33–6,71 € + 2,73 € Versand eBay.at [19]; 2,90 USD Hi-Link [10] |
| Hi-Link HLK-LD2450 | 24 GHz, Zielverfolgung | 6 m, ±60° Azimut, ±35° Elevation [11] | UART 256000 Bd; ESPHome `ld2450`: bis 3 Ziele mit X/Y/Geschwindigkeit, 3 Zonen, FW ≥ V2.02.23090617 [2] | Nicht verifiziert | 4,46 USD Hi-Link [11] |
| Seeed MR24HPC1 | 24 GHz | Bewegung 5 m, ruhend 4 m, Schlaf 3–3,5 m [6] | UART 115200 Bd; ESPHome `seeed_mr24hpc1`: `has_target`, `movement_signs`, Szenen (u. a. Badezimmer), `custom_unman_time` (Std. 30 s), Bewegungsgrenze gegen Glas-/Türstörungen [3] | −20…+85 °C [6] | 6,90 USD (nicht lieferbar) [6] |
| Seeed MR60BHA2 Kit (mit XIAO ESP32C6, ESPHome vorinstalliert) | 60 GHz, Atem/Herz | Präsenz 6 m, Vitalwerte 1,5 m [5] | ESPHome `seeed_mr60bha2`: `has_target`, `breath_rate`, `heart_rate`, `distance`, `num_targets` [4]; 5 V/1 A [20] | Nicht angegeben | 26,90 USD netto (Versand aus DE) [20] |
| Infineon BGT60TR13C | 58–63,5 GHz, 1 Tx/3 Rx, FoV 90° | 0,2–15 m [7] | Nur Chip/Dev-Kit, keine ESPHome-Komponente | −20…+70 °C [7] | k. A. |
| Aqara Presence Sensor FP2 | 60–64 GHz | bis 5 Personen, Zonen [8] | WLAN 2,4 GHz; HA über HomeKit Controller [8] | −10…+40 °C, IPX5 [8] | EUR-Preis nicht ermittelt |
| Panasonic Grid-EYE AMG8833/8834 | Thermopile 8×8 | FoV 60°/90°/35,6° [16] | I²C, 1/10 Hz [16] | Objekt 0–80 °C (AMG8833) bzw. −20…100 °C (AMG8834) [16] | k. A. |
| Melexis MLX90640 (Adafruit 4407) | Thermopile 32×24 | 55°×35° (110°-Variante 4469) [9] | I²C, keine native ESPHome-Komponente | −40…+85 °C [9] | 74,95 USD (nicht lieferbar) [9] |
| sentiotec/Harvia Not-Aus A-EMS | Taster, 1 Öffner + 1 Schließer | – | Öffner in Sicherheitskreis (STB), Schließer für Hupe/Anzeige [13][14] | −25…+70 °C, IPX4 [14] | 112,00 € Schachermayer AT [14] |
| EOS Not-Ruf/Not-Aus-Taster | Taster, zwei getrennte Kreise | – | Ofen über Sicherheitskreis aus, optional Signal in besetzten Raum [12] | Nicht ermittelt | Nicht ermittelt |
| saunabau.at Notruf-Meldeanlage (gewerblich) | Taster + Hupe + orange Leuchte | – | Optional GSM/Festnetz-Wählgerät, optional Übertemperaturfühler [21] | Nicht ermittelt | 480,00 € (statt 550,00 €) saunabau.at [21] |

## Bewertung für diese Sauna

1. **Komfort**: Ein LD2410C oder LD2450 im Vorraum, ausgerichtet durch Glastür/Fenster auf die Liegen, ist die günstigste Variante. Die LD2450-Zonen erlauben, den Vorraum selbst auszublenden [2]. Funktioniert der Glasdurchgang nicht (z. B. wegen Low-E-Beschichtung), Sensor tief in der Kabine hinter einer Holzblende montieren; dort vorher mit Datenlogger die Temperatur messen.
2. **Sicherheit**: Not-Aus-Taster mit Öffner im Sicherheitskreis des Ofens und Schließer auf einen StampPLC-Eingang (Alarm in HA). Zusätzlich maximale Saunagang-Dauer bei geschlossener Tür (Türkontakt). Radar-Logik „Person anwesend, aber `has_moving_target` seit X min aus“ als Voralarm (Push, Licht im Vorraum blinkt); keine automatische Abschaltung allein darauf stützen.
3. **Anbindung**: ESPHome-Komponenten für LD2410/LD2450/MR24HPC1 nutzen UART [1][2][3]. Ob am StampPLC ein freier UART neben RS485 verfügbar ist, ist nicht geprüft – alternativ ein eigener kleiner ESP32 im Vorraum mit MQTT/HA-API.

## Offene Punkte

- **Dämpfung durch Glas und Holz**: Keine Primärquelle mit Messwerten für 24/60 GHz durch Sauna-Glastüren (ggf. beschichtet) oder Holzpaneele gefunden. Hi-Link: „good shell penetration“ [10]; Seeed: Erkennung durch Glas und dünne Holzplatten vermeiden [5]. → Praxistest mit LD2410C im Engineering Mode (Gate-Energien vergleichen) [1].
- **Wasserdampf/Wasserfilm**: Seeed nennt Wasserfluss und Wasserfilm als Störquelle [5]; Wirkung von Aufgussdampf/Beschlag nicht belegt. → Test nach Aufguss.
- **Betriebstemperatur LD2410/LD2450** nicht aus Primärquelle verifiziert (Datenblatt nur über Google Drive verlinkt).
- **Temperatur an möglichen Montageorten** in der Kabine (bodennah, hinter Verkleidung) unbekannt → messen.
- **Normen**: EOS verweist auf EN 60335-1, EN 60335-2-53 und VDE 0100 Teil 703 [12]; konkrete Anforderungen für private Saunen in Österreich (ÖVE/ÖNORM) nicht recherchiert.
- **Aqara FP2 EUR-Preis** nicht ermittelt (Shopseite lädt Preise per JavaScript).
- **Ruhende Person vs. Bewusstlosigkeit**: Wie lange ein ruhender, wacher Saunagast „ohne Bewegung“ gemeldet wird, hängt von Firmware/Gate-Schwellen ab → empirisch Schwellwert bestimmen.
- **Mehrere Personen**: MR24HPC1 nur für eine Person [15]; LD2450 bis 3 Ziele [2] – Verhalten bei 2–3 Personen auf Liegen testen.

## Quellen

1. ESPHome – LD2410 Sensor: https://esphome.io/components/sensor/ld2410/
2. ESPHome – LD2450 Sensor: https://esphome.io/components/sensor/ld2450/
3. ESPHome – Seeed Studio MR24HPC1: https://esphome.io/components/seeed_mr24hpc1/
4. ESPHome – Seeed Studio MR60BHA2: https://esphome.io/components/seeed_mr60bha2/
5. Seeed Wiki – Getting started with MR60BHA2 mmWave Kit: https://wiki.seeedstudio.com/getting_started_with_mr60bha2_mmwave_kit/
6. Seeed Studio – MR24HPC1 Produktseite (SKU 101991030): https://www.seeedstudio.com/24GHz-mmWave-Sensor-Human-Static-Presence-Module-Lite-p-5524.html
7. Infineon – BGT60TR13C: https://www.infineon.com/part/BGT60TR13C
8. Aqara – Presence Sensor FP2 Specs: https://www.aqara.com/eu/product/presence-sensor-fp2/specs/
9. Adafruit – MLX90640 24x32 IR Thermal Camera Breakout (4407): https://www.adafruit.com/product/4407
10. Hi-Link – HLK-LD2410B: https://www.hlktech.net/index.php?id=1094
11. Hi-Link – HLK-LD2450: https://www.hlktech.net/index.php?id=1157
12. EOS – Not-Ruf / Not-Aus Taster (Produktseite und Montageanleitung): https://www.eos-sauna.com/produkte/saunazubehoer/not-ruf-not-aus-taster/ , https://www.eos-sauna.com/fileadmin/user_upload/EOS/pdf/gbaw/deutsch/saunazubehoer/Not-Aus-Taster_-_MA_de-en.pdf
13. sentiotec – Notaus-Schalter A-EMS (Produktseite und Anleitung): https://www.sentiotec.com/de/kategorien/sentiotec-sauna/zubehor/not-aus , https://www.sentiotec.com/sentiotec/sentiotec-sauna/ofen/notaus/not-aus_a-ems_10-24_deen.pdf
14. Schachermayer Webshop AT – SENTIOTEC Not-Aus Schalter A-EMS (112,00 €): https://webshop.schachermayer.com/cat/de-AT/product/sentiotec-not-aus-schalter-a-ems/113545217 ; Temperaturbereich/IPX4: https://www.heizungonlineshop.com/product/sentiotec-notausschalter-a-ems/
15. Seeed Wiki – MR24HPC1 (FAQ: mehrere Personen): https://wiki.seeedstudio.com/Radar_MR24HPC1/
16. Panasonic Industry – Grid-EYE Infrared Array Sensor: https://industry.panasonic.eu/products/components/sensors/ir-thermophile-array-sensor-grid-eye
17. Aevia Labs: https://aevialabs.com/
18. roboter-bausatz.de – Bewegungssensor LD2410C 24G (7,55 €): https://www.roboter-bausatz.de/p/bewegungssensor-ld2410c-24g
19. eBay.at – Suchergebnis HLK-LD2410 (5,33–6,71 € + 2,73 € Versand, Suchmaschinen-Snippet, nicht direkt geprüft)
20. Seeed Studio – MR60BHA2 mmWave Kit: https://www.seeedstudio.com/MR60BHA2-60GHz-mmWave-Sensor-Breathing-and-Heartbeat-Module-p-5945.html
21. saunabau.at – Notruf-Meldeanlage für gewerbliche Saunakabinen: https://saunabau.at/produkt/notruf-meldeanlage-fuer-gewerbliche-saunakabinen/
