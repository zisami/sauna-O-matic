# Automatisches Wacheln: mechanische und luftbasierte Lösungen

Ticket: #14 (Research). Stand der Quellen: Herstellerseiten und Anleitungen, abgerufen im November 2026.
Kennzeichnung: *(Schluss)* = eigene Folgerung, *(offen)* = in den Quellen nicht zu finden.
Die Quellenlage ist dünn: Es gibt nur ein Marktprodukt mit automatischem Wedeln, und das ist im Ofen eines anderen Herstellers eingebaut. Der Rest ist Konzept auf Basis der Herstellerangaben.

## Kurzantwort

- **Marktlösung:** Nur Klafs Vitality Boost im Ofen SANARIUM MAJUS (6 bis 12 kW, Wedelsystem mit zwei Zonen und einstellbaren Lamellen, fünf Programme inklusive "trockenes Wedeln"). Nachrüstung nur in Klafs-Saunen. Für den Harvia PO11 nicht verwendbar. Preis auf Anfrage. Ein eigenständiges Wedelgerät zum Nachrüsten fand sich in den Quellen nicht.
- **Empfehlung: luftbasiertes Wedeln im Eigenbau.** Hitzebeständiger Ventilator außerhalb der Kabine saugt heiße Luft aus der Kabine, bläst sie über einen Kanal mit Lamellenauslass auf Körperhöhe zurück, Drehzahl stufenlos. Richtpreis 300 bis 800 EUR *(Schluss)*, Ventilatortyp und Preis *(offen)*.
- **Mechanisches Wedeln (Tuch oder Schwenkarm) nicht empfohlen:** Verletzungs- und Brandrisiko im Ofenbereich, Verschleiß, Geräusch, Antrieb und Hitzefestigkeit aufwendig. Nur als Option, wenn der Wedeleffekt "Tuch" ausdrücklich gewünscht ist.
- **Wirkung nach Klafs:** Bewegte Luft verdrängt die isolierende Luftschicht auf der Haut, der Wärmeeintrag steigt. Das deckt sich mit dem klassischen Wedeln.

## 1. Marktlösungen

**Klafs Vitality Boost (SANARIUM MAJUS):**
- Fünf Programme: trockenes Wedeln, sanfter Aufguss mit wenig Wasser, klassischer Aufguss, klassischer Aufguss mit nachfolgendem intensivem Wedeln, intensiver "alpiner" Aufguss.
- Wedelsystem: warme Luft aus dem Ofen, zwei Zonen mit einstellbaren Lamellen, Wärmewolke auf den ganzen Körper.
- Auslösung per Steuerung, Taster in der Kabine oder Sprachbefehl ("HEY KLAFS"). Integrierter, herausnehmbarer Aufgusstank, Verteilschale.
- Nachrüstung nur "bei einer bereits eingebauten Klafs-Sauna" durch Tausch des Ofens. Technische Schnittstelle nicht dokumentiert *(offen)*. Preis auf Anfrage.

**EOS Aufguss-Mühle (halstrup-walcher-Fachartikel):** Schaufelrad, das aus einem Vorratsbehälter Wasser auf die Steine gibt. Keine Wedelfunktion, aber ein nützlicher Hinweis zur Antriebstechnik: Antrieb bis 80 °C, geräuscharm, mit Rutschkupplung gegen Handeingriffe.

**Lüftungsventilator EOS (Beispiel für Umluft):** 22 W, 230 V, 160 m³/h, 120 × 120 × 38 mm, IP55, 45 dB, für Räume bis 16 m³, Umgebungstemperatur -10 bis +70 °C. Die Umgebungstemperatur ist für die Kabine zu niedrig (Luft unter der Decke etwa 110 °C, obere Bank 80 bis 90 °C laut EOS-Anleitung). Der Ventilator gehört daher an die Außenseite.

## 2. Luftbasiert (empfohlen)

**Prinzip *(Schluss)*:** Kreislauf aus der Kabine: Ansaugung an der heißen Zone, Ventilator außen, Kanal mit Auslass in Körperhöhe über der oberen Bank. Das Wedeln ist reine Umwälzung und tauscht keine Luft mit außen aus.

```
Kabine (heiße Zone) -> Ansaugöffnung -> Kanal (Wanddurchführung) -> Ventilator (außen)
   -> Kanal -> Lamellen-Auslass in Körperhöhe (Zonen links/rechts, ggf. mit Klappe)
```

**Komponenten:**
- Ventilator: hitzebeständig für Fördermedium bis ca. 110 bis 120 °C *(Schluss)*. Typ und Preis *(offen)*, Hochtemperatur-Rohr- oder Radialventilatoren gibt es für diese Klasse, ein konkretes Produkt wurde nicht geprüft.
- Drehzahl stufenlos wie bei den Abluftlüftern: Hutschienen-Phasenanschnittregler mit Modbus (z. B. Sentera MVS-1-15CDM, siehe Lüfterregelung), vorausgesetzt der Ventilator ist ein Spaltpol- oder Kondensatormotor.
- Kanal und Auslass aus Edelstahl oder Aluminium, nicht brennbar, mit Abstand zu Ofen und Holz.
- Zweite Zone: zwei Auslässe mit Klappe, Stellantrieb 24 V *(optional, Schluss)*.

**Programme (Anlehnung an Klafs):** Trockenes Wedeln (nur Luft), sanft (kurz, geringe Drehzahl), intensiv (nach dem Aufguss, mehrere Läufe). Dauer und Drehzahl als Parameter im Aufguss-Schema.

**Geräusch:** Der Ventilator ist außen, hörbar ist nur der Luftstrom am Auslass. Mit Drehzahlregelung und Schalldämpfer-Kanalstück beherrschbar *(Schluss)*. Referenz: 45 dB beim EOS-Ventilator im Freilauf.

**Preisrahmen *(Schluss)*:** Ventilator 100 bis 400 EUR, Phasenanschnittregler ca. 100 bis 200 EUR (Modbus, siehe Lüfterregelung), Kanal, Auslass und Durchführung 100 bis 200 EUR. Gesamt etwa 300 bis 800 EUR.

## 3. Mechanisch (nicht empfohlen)

- Schwenkarm oder Rolle mit Tuch, Antrieb außerhalb der Hitzezone, Welle durch die Wand.
- Antrieb: Getriebemotor 24 V mit Rutschkupplung (Beispiel der EOS-Mühle). Welle und Lager hitzefest, Tuch aus hitzebeständigem Gewebe *(offen)*.
- Nachteile: Quetsch- und Schlagstellen an Personen, Tuch in Ofennähe (Brandgefahr), Geräusch durch Getriebe, Verschleiß, hoher Konstruktionsaufwand. Aufwand 400 bis 1500 EUR *(Schluss)*.

## 4. Sicherheit und Ansteuerung

- Wedeln ist **keine Sicherheitsfunktion**. Ein Ausfall darf die Sicherheitskette nicht berühren.
- Bewegte Heißluft wirkt stärker auf die Haut. Luftgeschwindigkeit und Auslassposition begrenzen, im Prototyp mit Handanemometer und Temperaturfühler prüfen *(offen, keine Grenzwerte in den Quellen)*.
- Freigabe: nur im aktiven Saunagang, bei geschlossener Tür und Kabinentemperatur im Betriebsbereich, nicht bei Störung der Temperaturmessung.
- Kein Wedeln ohne Voranzeige in der Kabine. Gleiche Regel wie beim Aufguss *(Schluss, Entscheidung im Grilling #24)*.
- Ventilator und Kanal so bauen, dass die Kabinenluft nicht über die Lüftungsöffnungen ungewollt zurückgesaugt oder kurzgeschlossen wird *(Schluss)*.
- Ansteuerung wie die Abluftlüfter über Modbus-Regler am RS485-Bus. Ein Relais (5 A) am StampPLC kann den Ventilator zusätzlich allpolig trennen.

## Offene Punkte

- Preis und technische Schnittstelle von Klafs Vitality Boost.
- Konkreter Hochtemperaturventilator (Bauform, Temperaturklasse, Motortyp, Preis).
- Zulässige Luftgeschwindigkeit am Körper und Auslasstemperatur.
- Schutzabstände von Kanal und Ventilator zum Ofen (Harvia-Handbuch, Abschnitt Sicherheitsabstände).
- Brandschutzanforderungen der Wanddurchführung.
- Ob ein Nachrüst-Wedelsystem (Tuch) für Privatsaunen angeboten wird.

## Quellen

- Klafs: Vitality Boost Produktseite (privat und Professional).
- EOS: Lüftungsventilator Produktseite (22 W, 160 m³/h); AquaDisp-Anleitung (Temperaturen in der Kabine).
- halstrup-walcher: Fachartikel zum Antrieb der EOS-Aufguss-Mühle.
