# Leistungsstellung 11 kW: Schütze oder Halbleiterrelais

Ticket: [#10](https://github.com/zisami/sauna-O-matic/issues/10)

Hinweis: Preise der Halbleiterrelais waren nicht abrufbar (Händlerseiten blockieren automatischen Abruf). Alles ohne Beleg ist als *(Schluss)* oder *(offen)* markiert.

## Kurzantwort

- **Empfehlung:** Hybrid aus **3-poligem Halbleiterrelais (Nullspannungsschalter, 3 × 30 A)** zum Stellen der Leistung und einem **4-poligen Installationsschütz 25 A in Reihe** als Sicherheitsschütz. Das Halbleiterrelais bildet per Zeitproportional-Taktung (Software) die stufenlose Leistung nach. Das Schütz öffnet über die Sicherheitskette und trennt allpolig, auch bei durchlegierter Halbleiter.
- **Rückfallvariante:** nur Schütz, Zweipunktregelung mit langer Mindestzykluszeit. Kostet rund 15 bis 20 EUR, ist aber nicht stufenlos und schaltet hörbar.
- **Nicht empfohlen:** Phasenanschnitt (Netzrückwirkung, Oberschwingungen) und Stufenschaltung über getrennte Heizelementgruppen (im Ofen intern verdrahtet, Eingriff nötig).
- **StampPLC taugt nicht zum direkten Stellen:** Die 4 Relais (5 A) sind weder für 16 A noch für häufiges Takten gedacht. Das Halbleiterrelais wird über einen **MOSFET-Ausgang des IO-Moduls A176** angesteuert (DC bis 60 V/2,5 A). Das StampPLC-System hat keinen Analogausgang, ein proportionales Stellglied mit 0-10 V entfällt.

## Lastdaten (aus dem Ticket Harvia PO11)

- 6 Heizelemente, zusammen 10,95 kW, **3 × 3650 W**, also 15,9 A je Phase bei 230 V. Anschluss 3 × 16 A, Nullleiter wird mitgeführt. Quelle: research/harvia-po11.md.
- Reine ohmsche Last. Kein Motoranteil.

## Variante A: Halbleiterrelais (Carlo Gavazzi RGC3)

Quelle: Datenblätter RGC2/RGC3 (25.02.2026) und RGC2P/RGC3P (16.07.2026), Carlo Gavazzi.

| Punkt | Wert | Folge |
|---|---|---|
| Typ | RGC3A60D30KGE: 3-polig, Nullspannungsschalter, 30 A je Pol bei 40 °C, 42 bis 660 VAC, Steuerung 5 bis 32 VDC, Breite 70 mm, ohne Lüfter | Passt zu 15,9 A je Phase mit Reserve. Der 20-A-Typ (RGC3A60D20…) wäre bei 40 °C zu 80 % ausgelastet, im Verteiler wird die Derating-Kurve schnell relevant. |
| Steuereingang | 5 bis 32 VDC, Ansprechen bei 4,8 V, Abfall bei 1 V | MOSFET des A176 (0 bis 60 V DC) mit 12 oder 24 V genügt. |
| Ansprechzeit | 0,5 Netzperiode + 500 µs bei 24 VDC | Taktung im Sekundenbereich unkritisch. |
| Sperrstrom | 5 mA je Pol bei Nennspannung | Last ist im Aus-Zustand nicht galvanisch getrennt, daher Schütz nötig. Der Strom fließt über Last zum N, nicht zum PE (Schluss), FI-Verhalten ist unkritisch. |
| Mindeststrom | 250 mA (Typ 30 A: 400 mA bei Variante M) | Heizlast liegt weit darüber. |
| 4-Leiter-Betrieb | RGC3 schaltet 3 Außenleiter. Die Proportionalreihe kennt eine Klemme C1/C2 für Vierleiternetz. Der Nullleiter läuft am Halbleiterrelais vorbei oder über das Schütz. | Verdrahtung mit der Elektrofachkraft festlegen. |
| Monitor-Variante (…M) | erkennt Netzausfall, Lastausfall, Kurzschluss/Unterbrechung des Halbleiters, Übertemperatur. Alarmausgang als Relais (Wechsler, 2 A/250 V), nur für ohmsche Lasten | Nützlich als **Diagnose** (Halbleiter durchlegiert, Heizstab defekt). Braucht externe Hilfsspannung (24 VDC oder 90 bis 250 VAC). Der Alarm ist eine Meldung, keine Sicherheitsfunktion. |
| Verlustleistung | Das Datenblatt gibt Kurven (Watt über Strom je Pol). Zahlenwerte aus dem Textauszug nicht lesbar. | Grobe Annahme etwa 1 W/A je Pol, also **etwa 50 W im Verteiler** bei 16 A *(Schluss, mit Datenblatt-Kurve prüfen)*. Abstände und Lüftung im Verteiler berücksichtigen. |
| Lebensdauer | Datenblatt nennt Bonddraht-Technik, "große Zahl Schaltspiele", Zahl nicht im Auszug | Bei Taktung alle 10 bis 30 s unkritisch *(Schluss)*. |
| Ausfallverhalten | Halbleiterrelais fallen typisch durchlegiert aus *(Schluss, allgemeine Erfahrung, im Datenblatt nicht belegt)* | Reihenschütz ist Pflicht für Aus-Zustand und Not-Aus. |
| Preis | nicht abrufbar *(offen)* | Händlerpreise manuell prüfen (Farnell, RS, Mouser, TME). |

Proportionalreihe RGC3P (Phasenanschnitt `E`, Vollwellenpakete `C1/C4/C16`) braucht 0 bis 10 V, 4 bis 20 mA oder Potentiometer als Eingang. Das StampPLC-System liefert keinen Analogausgang. Nur nutzbar mit zusätzlichem Analogausgang (Fremdbaugruppe) *(offen, nicht recherchiert)*. Der Nutzen gegenüber Software-Taktung des einfachen RGC3A ist gering.

## Variante B: Schütz

- Finder-Serie 22.34: Installationsschütz 4 Schließer, 25 A, Spule 230 V AC/DC, ab Reihenbaubreite, Schraubanschlüsse, Schließerkontaktöffnung ≥ 3 mm. Typ 22.34.0.230.4320 und .4340 (mit Hand-Schalter) sind in Shops gelistet. Preise Händler: ca. 14,99 bis 19,89 EUR (idealo, Stand 13.11.2026, Einzelstück).
- 25 A AC-1 ist ausreichend für 15,9 A ohmsch. Elektrische Lebensdauer und Schaltspiele pro Stunde stehen in den abgerufenen Quellen nicht *(offen)*.
- Zweipunktregelung mit Hysterese, Mindestzykluszeit **mehrere Minuten** (Schluss), um Verschleiß und Geräusch klein zu halten. Die Steine (bis 120 kg) puffern, daher regelungstechnisch brauchbar, aber Temperaturwelligkeit größer.
- Spule 230 V AC kann vom StampPLC-Relais (5 A/250 V) geschaltet werden, aber **nur in Reihe mit der Sicherheitskette** (siehe Ticket Sicherheitskonzept). 24-V-Spulen sind eine Alternative *(offen, Modelle nicht geprüft)*.

## Variante C: Stufenschaltung über Elementgruppen

Die Elemente (3 × 2150 W plus 3 × 1500 W) sind im Ofen intern verdrahtet. Das Handbuch zeigt nur beim PO165 einen Anschluss in zwei Gruppen. Beim PO11 wäre ein Eingriff in den Ofen nötig *(Schluss)*, das berührt Prüfung und Garantie. **Ausgeschlossen.**

## Netzrückwirkung und Flicker

- Die Produktnorm EN/IEC 61000-3-3 (Flicker) fordert für Geräte ≤ 16 A je Phase einen Kurzzeit-Flickerwert **Pst < 1,0**. Die Heizlast liegt knapp darunter (15,9 A). Für Geräte > 16 A gilt stattdessen IEC 61000-3-11. Quellen: Sekundärquellen (AGC Lighting, Spilma), nicht die Norm selbst.
- Das gilt rechtlich für in Verkehr gebrachte Geräte, nicht für den Eigenbau. Als Maßstab für verträgliches Verhalten am Netz taugt es trotzdem. Zuständig für die tatsächliche Verträglichkeit ist der Netzbetreiber *(offen: TAEV/Netzbetreiber-Vorgaben nicht recherchiert)*.
- Flicker hängt von **Stufengröße** (hier 16 A je Phase, gleichzeitig auf drei Phasen) und **Schaltrate** (Änderungen je Minute) ab. Eine Takt-Periode im Sekundenbereich vervielfacht die Zahl der Schalthandlungen gegenüber einem Schütz mit Minuten-Takt *(Schluss)*.
- **Folge für die Spezifikation:** Zeitproportional-Taktung mit **Periode 10 bis 30 s** statt 1 s. Die thermische Trägheit der Kabine und der Steine macht das regelungstechnisch unproblematisch *(Schluss)*. Alle drei Pole gleichzeitig (3-poliges Halbleiterrelais), damit die Last symmetrisch bleibt und der Nullleiter stromlos.
- Phasenanschnitt vermeiden: erzeugt Oberschwingungen und Funkstörungen.

## Sicherheitskette und Rolle der beiden Stellglieder

- **Schütz** liegt in Reihe hinter dem Hauptschalter und trennt alle vier Leiter. Seine Spule läuft über die hart verdrahtete Sicherheitskette (STB, Not-Aus, Türkontakt, Freigabe-Relais der Steuerung). Die Software darf die Kette nur **öffnen**, nie überbrücken.
- **Halbleiterrelais** wird nur im geschlossenen Schütz bedient. Bleibt der MOSFET des A176 hängen oder die Software ab, bleibt die Heizung bis zum Ablauf der Zeitbegrenzung bzw. bis das Schütz öffnet an. Gegenmaßnahme: Watchdog, der das Freigabe-Relais zurückfallen lässt *(Konzept offen, gehört ins Ticket Sicherheitskonzept)*. Ob das A176 selbst ein Ausgangs-Timeout hat, ist nicht dokumentiert *(offen)*.
- Normbezug (aus dem Ticket Normlage): Heizzeit ≤ 6 h, kein automatischer Neustart, Türkontakt bei Fernbetrieb. Diese Bedingungen werden am Schütz umgesetzt.

## Vorschlag Stückliste (Leistungsteil)

| Position | Typ | Preis |
|---|---|---|
| Halbleiterrelais 3-polig 30 A | Carlo Gavazzi RGC3A60D30KGE (optional …M mit Monitoring) | *(offen)* |
| Sicherheitsschütz 4 Schließer 25 A | Finder 22.34.0.230.4320 oder gleichwertig | ca. 15 bis 20 EUR |
| MOSFET-Ausgang zur Ansteuerung | IO-Modul A176 (hat auch Analogeingänge für Ticket Sensorik) | USD 19,95 (Quelle: Ticket Plattform) |
| Hilfsspannung 24 VDC | je nach Steuerkonzept (Netzteil, Ticket Stromlaufplan) | *(offen)* |

## Offene Punkte

1. Preise Halbleiterrelais (RGC3A60D30KGE, Variante …M).
2. Verlustleistung und Derating aus den Datenblatt-Kurven ablesen (Zahlen aus dem Textauszug nicht lesbar).
3. Elektrische Lebensdauer des Schütz bei ohmscher Last und AC-1.
4. A176: Verhalten der MOSFET-Ausgänge bei Kommunikationsverlust (Timeout?), Schaltfrequenz über I2C.
5. Verdrahtung des Nullleiters mit Halbleiterrelais (Vierleiter), mit der Elektrofachkraft festlegen.
6. Flickerverhalten am konkreten Anschluss: Netzbetreiber fragen, Messung nach Aufbau ist Sache der Fachkraft.
7. Modbus- oder Netzwerk-fähige Leistungssteller als Alternative zu A176 plus RGC3A: nicht recherchiert.

## Quellen

- Carlo Gavazzi, RGC2/RGC3, 3-phase solid state relays with integrated heatsink, Datasheet 25/02/2026.
- Carlo Gavazzi, RGC2P/RGC3P, 3-phase proportional switching controllers, Datasheet 16/07/2026.
- Finder, Typ 22.34 Installationsschütze (findernet.com), Händlerlisten (idealo.de, idealo.at, Stand 13.11.2026).
- Spilma, IEC 61000-3-2 and 3-3; AGC Lighting, Pst value for flicker (Sekundärquellen für Pst < 1,0).
- Interne Tickets: research/harvia-po11.md, research/plattform-stamplc-tab5.md, research/normlage.md.
