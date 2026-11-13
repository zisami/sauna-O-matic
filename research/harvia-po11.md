# Research: Harvia Legend PO11 – Anschluss, Heizelemente, Fühler und STB

> Issue: [#8](https://github.com/zisami/sauna-O-matic/issues/8) · Stand: 13.11.2026
>
> **Frage:** Was gibt der Harvia Legend PO11 für eine externe Steuerung vor?
>
> **Kennzeichnung:** *(Schluss)* = aus den Quellen abgeleitet, nicht wörtlich belegt. *(offen)* = nicht ermittelt.

## Kurzantwort

- **Heizelemente:** 6 Stück, 3 × ZSP-240 mit 2150 W/230 V und 3 × ZSS-110 mit 1500 W/230 V. Zusammen 3 × 3650 W = **10,95 kW** [S1, Kap. 4]. Die Zuordnung zu den Phasen (je ein Element jeder Sorte pro Phase) ist *(Schluss)*; der Anschluss erfolgt an U, V, W und N.
- **Anschluss:** 400 V 3N~, **3 × 16 A**, Zuleitung und Verbindung zum Ofen **5 × 2,5 mm²**. Der Ofen hängt halbfest an einer spritzwassergeschützten Klemmdose, höchstens 500 mm über dem Boden. Verbindungskabel H07RN-F, **kein PVC** [S1, Kap. 3.3].
- **Hitzeklassen:** Leitungen oberhalb von 1000 mm Höhe in der Kabine müssen belastet mindestens 170 °C aushalten (z. B. SSJ). Geräte über 1000 mm Höhe müssen für 125 °C zugelassen sein (T125) [S1, Kap. 3.3].
- **Fühlerkabel:** 4 × 0,25 mm² (SSJ), vom Ofen-Handbuch gefordert [S1]. Der Harvia-Fühler WX232 hat ca. 4 m Leitung [S2].
- **Fühler WX232:** NTC-Thermistor **22 kΩ bei 25 °C** plus **rücksetzbarer Überhitzungsschutz** im selben Gehäuse. Es gibt 4 Adern: Rot/Gelb für den NTC, Blau/Weiß für den Überhitzungsschutz [S2, Kap. 1.1 und Fehlercodes E1–E3]. Die Kennlinie (B-Wert) steht nicht im Handbuch *(offen)*.
- **Wichtig für die Sicherheitskette:** Bei Harvia ist der Überhitzungsschutz ein **Messkreis**, den die Elektronik auswertet (Fehler E3 bei Unterbrechung), kein dokumentierter Lastkontakt [S2]. Die Belastbarkeit des Kontakts (Strom/Spannung) ist nicht dokumentiert *(offen)*. Er darf erst dann direkt die Schützspulen schalten, wenn der Wert geklärt ist. Alternativen: Kontakt nur über ein Sicherheitsrelais mit Kleinstrom auswerten, oder eigener STB mit dokumentierter Schaltleistung.
- **Fühlerposition (freistehender Ofen):** Fühler an der Wand über dem Ofen auf der senkrechten Mittellinie, **100 mm unter der Decke** (Option 1), oder an der Decke über dem Ofen, 100–200 mm von der senkrechten Mittellinie der Ofenseite (Option 2) [S1, Abb. 6, Tabelle PO11: 100–150 / 100–200 / 100; S2, Kap. 3.3.1]. Die Bedeutung der Maße A–C stammt aus der Zeichnung und ist nicht im Fließtext erklärt. Ein optionaler zweiter Fühler (WX325) kommt an die Wand möglichst weit vom Ofen, 500–700 mm unter der Decke [S2, Kap. 3.3.2].
- **Zuluft-Abstand zum Fühler:** mindestens **1000 mm** bei Mehrrichtungs-Auslass, **500 mm** bei Auslass, der vom Fühler wegzeigt [S1, S2]. Die Zuluft über dem Ofen muss das einhalten, sonst misst der Fühler zu kalt und der Ofen kann überheizen. Bei mechanischer Entlüftung soll die Zuluft über dem Ofen liegen, bei Schwerkraftentlüftung unter oder neben dem Ofen [S1, Kap. 2.2].
- **Kabinenvolumen:** Der PO11 ist für **9–20 m³** und Raumhöhe ≥ 1900 mm ausgelegt [S1, Tabelle 2]. Die ältere Fassung von 2009 nennt 9–24 m³ und 2000 mm [S3]. Unisolierte Flächen (Glas) zählen **1,2 m³ je m²** dazu [S1, Kap. 2.3]. Deine Kabine hat 2,2 × 3,1 × 2,6 m = **17,7 m³** (Schluss aus deinen Maßen). Ab etwa **1,9 m² Glasfläche** (Fenster plus Glastür) wird die Obergrenze von 20 m³ überschritten. Das ist zu messen.
- **Steine:** ca. 120 kg: ca. 40 kg kleinere (Ø 5–10 cm) und ca. 80 kg größere (Ø 10–15 cm). Gewicht Ofen 30 kg [S1].
- **Zulässige Steuergeräte laut Ofen-Handbuch:** Harvia C150, C150VKK, C260 (laut Handbuch bei C150VKK/C260 nur Gemeinschaftsnutzung), Griffin [S1, Kap. 1.3]. CS170/CX170 sind dort nicht genannt, obwohl dein Händler sie als kompatibel führt *(Widerspruch, offen)*.
- **Referenzwerte eines Harvia-Steuergeräts (CS170):** Temperaturbereich 40–110 °C, Betriebszeit 1–6 h bei Familiensaunen (bis 12 h bei Gemeinschaftssaunen einstellbar), Abtrocknungsintervall 45 min bei 40 °C, Stromausfall-Speicher standardmäßig aus [S2, Kap. 1.2, 2.2]. Das Bedienfeld darf in der Kabine nur bis 1 m Höhe eingelassen werden, die Leistungseinheit gehört außerhalb der Kabine an einen trockenen Ort über 0 °C [S2, Kap. 3.1, 3.2].
- **Hinweis zum FI:** Im Ofen-Handbuch steht im Abschnitt zum Isolationswiderstand: „Schalten Sie den Netzstrom des Elektrosaunaofens nicht über den Fehlerstromschalter ein!“ Das bezieht sich auf die ersten Aufheizungen, solange Feuchte in den Heizwiderständen steckt [S1, Kap. 3.3.2]. Das Schaltbild des CS170 zeigt dagegen einen FI im Zulauf [S2, Abb. 4]. Die Elektrofachkraft muss das für die Inbetriebnahme klären *(offen)*.
- **Preise:** Für WX232, SP232 und den Ofen konnte ich keinen Preis ermitteln *(offen)*. Das Ersatzteil **SP232** („Temperaturfühler, max. 150 °C Begrenzer, weiß“) passt an ein SIHF-4×0,25-Kabel (T 180 °C), das separat zu bestellen ist [S4].
- **Abdeckschutz (SafetySwitch) für Fernstart:** *Nicht belegt.* Das heruntergeladene Dokument war nicht auswertbar. Kompatibilität mit dem PO11 und Preis bleiben offen *(offen)*.

## Elektrische Daten PO11 (Auszug)

| Größe | Wert | Quelle |
|---|---|---|
| Leistung | 11,0 kW | S1, S3 |
| Netz | 400 V 3N~, 50/60 Hz | S1, S3 |
| Absicherung | 3 × 16 A | S1, S3 |
| Kabel Netz und Ofen | 5 × 2,5 mm² | S1 |
| Fühlerkabel | 4 × 0,25 mm² | S1 |
| Heizelemente | 3 × ZSP-240 (2150 W/230 V), 3 × ZSS-110 (1500 W/230 V) | S1 |
| Strom je Phase *(Schluss)* | 3650 W / 230 V ≈ 15,9 A | berechnet |
| Kabinenvolumen | 9–20 m³, Höhe ≥ 1900 mm | S1 |
| Steine | ca. 120 kg | S1 |
| Schutzart | IPX4 | S3 |

## Wirkung auf unsere Entscheidungen

- **Leistungsstellung (#10):** 3 Phasen mit je ca. 15,9 A. Die Aufteilung 2150 W/1500 W je Phase ist im Ofen intern verdrahtet *(Schluss)* und lässt sich ohne Öffnen des Ofens nicht aufteilen. Ein Anschluss in zwei Gruppen kennt das Handbuch nur beim PO165 [S1, Abb. 5].
- **Sicherheitskonzept:** Den Harvia-STB als Primärglied zu nutzen ist nur möglich, wenn seine Kontaktdaten stimmen. Sonst braucht es einen eigenen STB mit dokumentierter Schaltleistung.
- **Sensorik:** Der NTC 22 kΩ lässt sich über einen Modbus-Wandler weiterverwenden, sobald die Kennlinie geklärt ist. Zuluft-Abstand zum Fühler beachten.
- **Normlage (#9):** Maximale Betriebszeit und Fernstart sind dort zu prüfen. Das Harvia-Gerät begrenzt standardmäßig auf 1–6 h.

## Offene Punkte

1. Kontaktdaten des Überhitzungsschutzes im WX232/SP232 (Strom, Spannung, Auslösetemperatur).
2. Kennlinie (B-Wert) des NTC 22 kΩ.
3. Abdeckschutz (SafetySwitch): Kompatibilität und Preis.
4. Preise von WX232/SP232, Heizelementen und Ofen.
5. Glasfläche der Kabine messen (Volumen-Obergrenze).
6. Verhalten FI bei der ersten Inbetriebnahme mit der Elektrofachkraft klären.
7. Herstellerangabe zu CS170/CX170 als Steuergerät des PO11 klären (Handbuch nennt sie nicht).

## Quellen

- **[S1]** Harvia: Montage- und Gebrauchsanleitung PO11, PO165, Dokument 14072023/Y05-0146, Kap. 1.1, 1.3, 2.2, 2.3, 3.1–3.4, 4 (Ersatzteile), Abb. 5 und 6. Abgerufen am 13.11.2026.
- **[S2]** Harvia: Montageanleitung Xafir CS170, Dokument 08122016/Y05-0304, Kap. 1.1–1.3, 2.1–2.3, 3.1–3.4, Abb. 4. Gelesen über eine Händlerkopie: [saunaundmehr.de](https://www.saunaundmehr.de/SuM/manuals/Montageanleitung-Xafir-CS170.pdf) (kein Herstellerserver).
- **[S3]** Harvia: Produktseite Legend Pro PO11 11 kW schwarz, HPO110400M: [harvia.com](https://www.harvia.com/de/produkts/HPO110400M/legend-pro-po11-11-kw-schwarz). Dazu die ältere Anleitung 06012009 (PO11/PO165).
- **[S4]** Harvia: Ersatzteil SP232: [harvia.com](https://www.harvia.com/en/products/SP232/temperature-sensor-max-150c-limiter-white).
