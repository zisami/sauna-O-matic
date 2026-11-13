# Automatischer Aufguss und Duftdosierung: Marktlösungen und Eigenbau

Ticket: #13 (Research). Stand der Quellen: Herstellerseiten, Bedienungsanleitungen und Händlerangebote, abgerufen im November 2026.
Kennzeichnung: *(Schluss)* = eigene Folgerung, *(offen)* = in den Quellen nicht zu finden.

## Kurzantwort

- **Kein Marktprodukt passt für den PO11 mit Fremdsteuerung.** Die geprüften Geräte sind an eigene Öfen oder Steuerungen gebunden, haben höchstens einen Duft und brauchen Festwasseranschluss (1,5 bis 7 bar). Der Wassertank der Anlage ist damit nicht vereinbar.
- **Empfehlung: Eigenbau mit 24-V-Komponenten.** Wassertank außerhalb der Kabine, Membranpumpe (und Magnetventil), je Duft eine Schlauch-Dosierpumpe, Ofenbrause aus Edelstahl, Leitung mit Gefälle zum Ofen. Richtpreis 150 bis 400 EUR *(Schluss)*, mehrere Düfte möglich.
- **Wichtigste Regeln aus den Herstellerunterlagen:** Duft nur dosieren, wenn Wasserfluss nachgewiesen ist. Kein Aufguss auf kalten Ofen. Wassermenge je Aufguss klein halten (Harvia: Schöpfkelle höchstens 0,2 l). Nur Saunadüfte verwenden.
- **Rückfall:** WDT/EOS AromaTec II Basic (ein Duft, Tasterkontakt) mit Druckerhöhungspumpe aus dem Tank. Preis nur auf Anfrage.

## 1. Marktlösungen

| Produkt | Duft | Anbindung | Wasser | Preis | Eignung für PO11 mit eigener Steuerung |
|---|---|---|---|---|---|
| Harvia Autodose SASL1 | ein Duft, Modus "Reinwasser" oder "Duft" | Taster, laut Seite "Remote use: No" | Wasserleitung bauseits | 632,00 EUR (Händler) | Nein (Fremdsteuerung nicht vorgesehen, Anschluss an Harvia-Steuerung *(offen)*) |
| EOS AquaDisp (Art. 94.6904) | kein Duft | nur EOS-Steuergeräte, Auslösung über potenzialfreien Kontakt (3-s-Impuls an E3/E4) | Festwasser, Magnetventil | auf Anfrage | Nein (nur für EOS-Öfen mit Verdampferrinne freigegeben) |
| EOS/WDT AromaTec II Basic (Art. 94.6798) | ein Duft | Zyklusprogramm oder Taster (Steckdose für Taster) | 1,5 bis 6 bar | auf Anfrage | Bedingt, siehe Abschnitt 2 |
| Klafs Vitality Boost | integriert | Klafs-Steuerung, Sprachbefehl | integrierter Tank | auf Anfrage | Nein, nur im Klafs-Ofen MAJUS |
| "Sauna-Aufgussautomatik" (EisSommer) | ein Duft, max. 12 ml | Zyklus | Wasserleitung bis 7 bar | 140,00 EUR | Bedingt, Händler wirkt unseriös (Verleih und Fahrräder im Sortiment) *(Schluss)* |

**AquaDisp (Bedienungsanleitung):**
- Dosierung 200, 300 oder 400 ml je Aufguss (5, 8 oder 10 s), Intervall 15, 30 oder 45 min.
- Dosierung startet erst, wenn der Ofen die Solltemperatur das erste Mal erreicht hat.
- Betrieb nur mit EOS-Öfen und EOS-Steuergeräten, "nicht bestimmungsgemäß" sonst. Magnetventil bis 80 °C, Wasser mit Gefälle zum Ofen, keine Knicke und kein Durchhang.

**AromaTec II Basic (Bedienungsanleitung):**
- Wasser bis 1200 ml und Duft bis 24 ml (2 %) je Aufguss, Zykluszeit 1 bis 90 min, 230 V, 50 W, Wasserdruck 1,5 bis 6 bar, Medium 5 bis 30 °C.
- Duftpumpe ist eine Schlauchpumpe, Ofenbrause Edelstahl Ø 18 × 200 mm, 3/8 Zoll. Anschlussset mit 10 m PTFE-Schlauch 6 × 1 mm.
- Sicherheit: Duft wird erst freigegeben, wenn der Druck 1,2 bar übersteigt, und der Druckabbau muss binnen 3 s erfolgen. Sonst "Störung Wasserdruck", Duft aus. Rückschlag-/Dosierventil mit 0,69 bar Öffnungsdruck.
- Hersteller verlangt ausdrücklich, die Anlage mit der Saunasteuerung zu verriegeln (Aufguss nicht auf kalten Ofen), sonst Brandgefahr durch Duftansammlung.
- Duftkonzentrate sind leicht entzündlich und reizend. Dosierschlauch bei jedem Auffüllen prüfen, Verschleißteil.
- Montageort 5 bis 35 °C. Aufguss-Leitung muss selbst leerlaufen (fallend, ohne Wassersack).

## 2. Eigenbau (empfohlen)

**Aufbau (Schluss, abgeleitet aus den Herstellerlösungen):**

```
Tank (ca. 10 l, Technikraum) -> Membranpumpe 24 V -> Magnetventil (NC, optional)
   -> Strömungs-/Druckschalter -> T-Stück mit Dosierventil (<- Duftpumpen 1..n)
   -> PTFE-Schlauch 6 x 1 mm mit Gefälle -> Wanddurchführung -> Ofenbrause
```

**Komponenten und Richtpreise (Marktangebote November 2026, grobe Spannen):**
- Membranpumpe 12/24 V, selbstansaugend, trockenlaufgeschützt: ca. 10 bis 60 EUR.
- Schlauch-Dosierpumpe 24 V je Duft (Kamoer-Klasse, 100 bis 160 ml/min): ca. 30 bis 150 EUR, je nach Händler. Größere Einheiten kosten mehr.
- Magnetventil, Füllstandsschalter, Strömungsschalter, Rückschlagventil, Schlauch, Brause, Wanddurchführung: ca. 80 bis 150 EUR *(Schluss, keine Einzelquellen)*.
- Edelstahl-Ofenbrause und Wanddurchführung gibt es als EOS-/WDT-Zubehör (Art. 94.5191, 15214), Preise *(offen)*.

**Warum Schlauchpumpe für den Duft:** Sie fördert kleine Mengen genau, auch bei Luftblasen in der Saugleitung, und kommt mit dem Konzentrat nur über den Schlauch in Berührung (WDT-Anleitung). Die Kamoer KCM ist für Medien bis 60 °C angegeben und läuft optimal bei 24 V.

**Dosiermengen:** Harvia nennt für die Handkelle höchstens 0,2 l je Wurf, bei größeren Mengen kocht Wasser nicht vollständig ab und spritzt. Orientierung der Herstellerlösungen: 200 bis 400 ml Wasser (AquaDisp), Duft 1 bis 2 % *(AromaTec, EisSommer 5 %)*. Für den Eigenbau ein Aufguss in mehreren Schüben von je ca. 100 bis 200 ml *(Schluss)*.

**Wasserqualität (Harvia-Handbuch):** Kalzium unter 100 mg/l, Eisen unter 0,2 mg/l, kein gechlortes Wasser, kein Salzwasser. Nur für Saunawasser zugelassene Düfte. Tankwasser regelmäßig wechseln und entkalken *(Schluss)*.

**Steuerung:** Fünf 24-V-Gleichstromausgänge (Wasserpumpe, Ventil, bis zu drei Düfte) und zwei Eingänge (Tank leer, Wasserfluss). Das StampPLC hat nur vier Relais (5 A), die schon durch Lüfter, Licht und weitere Funktionen gebraucht werden. Es braucht ein Modbus-Relais- oder -MOSFET-Modul am RS485-Bus *(Schluss, nicht recherchiert)*.

**Zeitlicher Ablauf je Aufguss (Vorschlag):**
1. Freigabe prüfen: Saunagang aktiv, Ofen über Mindesttemperatur (z. B. 60 °C *(Schluss)*), Tank nicht leer, Mindestabstand zum letzten Aufguss, Tür zu, Kontakt der Sicherheitskette ok.
2. Wasserpumpe an, Strömungsschalter muss binnen weniger Sekunden ansprechen.
3. Erst danach Duftpumpe für die eingestellte Menge, währenddessen Wasserfluss weiter überwachen.
4. Duft aus, Wasser läuft nach, Leitung läuft über Gefälle leer.
5. Sperrzeit bis zum nächsten Aufguss.

## 3. Sicherheit und Betrieb

- **Kein Aufguss auf kalten Ofen:** Herstellerforderung bei AromaTec, bei AquaDisp durch die Temperaturfreigabe gelöst. Software-Freigabe plus ein **zweiter, unabhängiger Kontakt** (z. B. Bimetall-Thermostat am Ofen in Reihe zur Versorgung von Ventil und Pumpen) *(Schluss)*.
- **Überflutung:** Begrenzung auf Tankinhalt, Höchstmenge je Aufguss und je Stunde in der Software, Strömungsüberwachung. Im Fehlerfall alle Pumpen und das Ventil stromlos.
- **Duftkonzentrat:** nur bei bestätigtem Wasserfluss dosieren, Dosierleitung mit Rückschlagventil (ca. 0,7 bar), Behälter außerhalb der Kabine bei 5 bis 35 °C und nicht im Verteilerkasten *(Schluss)*.
- **Nachtropfen:** Leitung mit gleichmäßigem Gefälle zum Ofen, keine Wassersäcke. Restwasser läuft auf die Steine.
- **Verbrühungsgefahr:** Aufguss nur bei geschlossener Tür und mit Voranzeige in der Kabine, kein Fernstart-Aufguss ohne anwesende Person *(Schluss, Entscheidung im Grilling #23)*.
- **Verkalkung:** weiches oder enthärtetes Wasser, Brause regelmäßig entkalken *(Schluss)*.
- Die Aufguss-Technik ist **keine Sicherheitsfunktion**. Die Sicherheitskette bleibt unabhängig davon.

## Offene Punkte

- Preise: Harvia Autodose bei österreichischen Händlern, AquaDisp, AromaTec II Basic, Ofenbrause und Wanddurchführung.
- Ob der Harvia Autodose ohne Harvia-Steuerung nutzbar ist (Handbuch war nicht abrufbar).
- Wasserdruck im Tankbetrieb: 1,5 bar für den Rückfall AromaTec erfordern Druckerhöhungspumpe mit Druckschalter und Ausgleichsgefäß.
- Magnetventil, Strömungsschalter und Füllstandsschalter: konkrete Typen und Preise.
- Duftstoffe: welche Konzentrate (Flammpunkt, Schlauchverträglichkeit) und Menge je Duft.
- Ausfallverhalten der Ausgangsmodule bei Kommunikationsverlust (siehe auch Leistungsstellung).

## Quellen

- EOS: AquaDisp, Montage- und Gebrauchsanweisung (Druck-Nr. 29345017) und Produktseite; AromaTec II Basic (WDT), Montage- und Bedienungsanleitung (Index 01, 2017) und Produktseite.
- Harvia: Autodose SASL1 Produktseite; Legend-Handbuch (Aufguss, Wasserqualität, Warnungen).
- Klafs: Vitality Boost Produktseite (privat).
- Händlerseiten: Megasauna (Harvia Autodose, 632,00 EUR), Saunatechniker (EOS, Preis auf Anfrage), EisSommer (Aufgussautomatik, 140,00 EUR).
- Kamoer KCM (Funduino-Seite, Marktplatzangebote).
- halstrup-walcher: Fachartikel zur EOS-Aufguss-Mühle (Hinweis auf Alternativtechnik mit Schaufelrad).
