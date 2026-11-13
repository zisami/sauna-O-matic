# Sensorik: Modbus-RTU-Fühler für Temperatur, Feuchte und CO₂

Ticket: #11 (Research). Stand der Quellen: Herstellerseiten und Datenblätter, abgerufen für diese Recherche.
Kennzeichnung: *(Schluss)* = eigene Folgerung, *(offen)* = in den Quellen nicht zu finden.

## Kurzantwort

| Größe | Empfehlung | Schnittstelle zur Steuerung | Preis |
|---|---|---|---|
| Kabinentemperatur | Pt1000-Kabelfühler mit Silikonleitung (bis 180 °C) plus Hutschienen-Messumformer im Verteiler | 0–10 V auf Eingang A176, oder Modbus-Analogmodul | ca. 41 EUR je Kanal (Fühler plus Umformer) |
| Kabinenfeuchte | Optional. Nur beheizte Sonde der Klasse E+E HTS801 (Sonde bis 180 °C) | RS485, Modbus RTU | *(offen)*, Anfrage nötig |
| CO₂ | NDIR-Kanalfühler (E+E EE850) im Abluftweg außerhalb der Hitzezone | RS485, Modbus RTU | *(offen)*, Anfrage nötig |

- Reine Halbleiter-Feuchtefühler (Sensirion SHT4x) gehören nicht in die heiße, feuchte Kabine.
- Der Harvia-Fühler (NTC 22 kΩ) wird für die Regelung nicht weiterverwendet. Gründe: Kennlinie nicht dokumentiert, und für einen Widerstandseingang fehlt in der Steuerung die Hardware.
- Die Temperaturkette ist die einzige, die sicher und günstig umsetzbar ist. Feuchte und CO₂ hängen an Preisangaben, die der Hersteller nur auf Anfrage nennt.

## 1. Temperatur

**Anforderung:** Messung an Decke und optional in Bankhöhe, Dauerbetrieb bis ca. 140 °C Fühlerspitze.

**Aufbau:**
- Pt1000-Kabelfühler Ø 6 mm in der Kabine, Zuleitung mit 4-adriger Silikonleitung (−60…+180 °C, ca. 2,00 EUR/m netto).
- Hutschienen-Messumformer Pt1000/Pt100 auf 0–10 V oder 4–20 mA im Verteiler (Sensorshop24, Art. MU-PT1000: 35,00 EUR netto, 41,65 EUR brutto).
- Fühler Ø 6 mm: ca. 6,40 EUR. Die maximale Fühlertemperatur dieses Artikels ist vor der Bestellung zu prüfen *(offen)*.

**Anbindung an die Steuerung:**
- Die StampPLC hat keinen Analogeingang. Das IO-Modul A176 hat zwei Analogeingänge (0–10 V / 0–20 mA), das reicht für zwei Temperaturen.
- Für mehr Kanäle: Waveshare Modbus RTU Analog Input 8CH. Acht Eingänge, 12 Bit, Variante 0–10 V (B) oder 0–20 mA, Standard 9600 Baud, Funktion 04, Register 0x0000–0x0007, Adresse und Baudrate einstellbar, Jumper je Kanal. Preis *(offen)*.
- Alternative mit direktem Pt-Eingang: Seneca Z-4RTD2 (4-Kanal-RTD-Wandler mit Modbus RTU laut Produktlinie). Die abgerufene Seite enthielt keine technischen Daten, Preis *(offen)*.

**Genauigkeit:** Pt1000 plus Umformer liegt typischerweise bei wenigen Zehntel Kelvin *(Schluss, keine Herstellerangabe in den Quellen)*. Für eine Saunaregelung ausreichend.

**Sicherheit:** Die Messung dient nur der Regelung. Die Übertemperaturabschaltung bleibt die hartverdrahtete Kette (STB), siehe Normlage und Leistungsstellung.

## 2. Relative Feuchte

**Randbedingungen:**
- Kabinenluft typischerweise 70–110 °C mit niedriger relativer Feuchte, nach dem Aufguss kurzzeitig hohe Feuchte mit Kondensation *(Schluss)*.
- Duftstoffe (ätherische Öle) belasten Sensorelemente chemisch *(Schluss)*.

**Beheizte Sonde (E+E):**
- EE33 ist abgekündigt, Nachfolger HTS801.
- HTS801: Sonde −80…+180 °C, 0–100 %RH, ±1,3 %RH und ±0,2 °C (Werte der EE33), RS485 mit Modbus RTU, wahlweise Ethernet-PoE mit Modbus TCP. Austauschbare "rapidX"-Sonden, Heizmodi gegen Kondensation und für chemische Regeneration (ARC, bei chemischer Belastung etwa alle 24 h empfohlen). Variante HTS801-M4T7 mit beheiztem Sensorelement und beheiztem Sondenkörper für hohen Taupunkt.
- EE33-Daten als Anhalt: t90 ≤ 15 s, Genauigkeit ±(1,5 + 0,015·Messwert) %RH bei 40…180 °C, Elektronik nur −40…+60 °C (daher Sonde in der Kabine, Elektronik im Verteiler oder auf der kalten Seite), Sondenkabel 2/5/10 m.
- Preis nicht veröffentlicht *(offen)*; bei Geräten dieser Klasse eher im dreistelligen Eurobereich *(Schluss)*.

**Ungeeignet für die Kabine:**
- Sensirion SHT4x: Betriebsbereich laut Datenblatt −40…125 °C und 0–100 %RH, kondensationsfähig. Die Applikationsnotiz empfiehlt aber 5–60 °C und 20–80 %RH. Bei längerer Feuchte über ca. 90 %RH tritt ein reversibler positiver Offset auf (Creep), mildern lässt er sich mit dem Chip-Heizer (Beispiel 200 mW für 1 s je Minute). Der Hersteller schließt Sicherheits- und Not-Aus-Anwendungen aus.
- Galltec "DI – Indoor": Temperaturbereich 0…+50, −30…+70 oder 0…+100 °C, Betrieb der Elektronik bis 60 °C, ±2,5 %RH. Nur für Vorraum oder Verteilerraum.

**Empfehlung:**
- Phase 1 ohne Feuchtefühler in der Kabine. Temperatur, CO₂ und Dosiermenge reichen für Regelung und Aufgussablauf *(Schluss)*.
- Phase 2 optional: HTS801 (Sonde in der Kabine) für Rückmeldung beim Aufguss und Komfortanzeige. Vor Kauf Preis und Eignung für Duftstoffe beim Hersteller klären.
- SHT45 mit Heizer als Preisfallback nur im Abluftkanal außerhalb der Hitze oder im Vorraum.

## 3. CO₂

**Prinzip:** NDIR mit Zweiwellenlängen-Kompensation (Alterung). Ein CO₂-Fühler gehört nicht in die Kabine, die Elektronik verträgt die Hitze nicht.

**E+E-Familie:**
- EE850: Kanalfühler, CO₂ plus Temperatur, RS485 mit Modbus RTU.
- CDS201: Raumfühler, CO₂/T/RH, analog oder RS485.
- EE820: für raue Umgebung.
- Sonden EE872 (beheizte Varianten gegen Kondensation) und EE895.
- Messbereich, zulässige Betriebstemperatur und Preis stehen nicht in den abgerufenen Auszügen *(offen)*.

**Einbauort:**
- Abluftkanal außerhalb der Kabinenwand, im unteren Drittel der Kabine abgesaugt. Die Lufttemperatur dort ist niedriger als an der Decke, kann aber über der Elektronikgrenze liegen *(Schluss)*.
- Erst Kanaltemperatur im Betrieb messen oder abschätzen. Wenn zu heiß: Probenahme per kleinem Schlauch aus der Kabine zu einem Sensor im Verteiler oder Vorraum *(Schluss, nicht recherchiert)*.
- Ein Fühler im Vorraum misst nicht die Kabinenluft und taugt nicht für die Regelung.

## 4. Busaufbau (RS485)

- StampPLC hat RS485 (SIT3088) und ist Modbus-Master.
- Alle Fühler als Slaves auf einem Strang, Linientopologie, Abschlusswiderstand 120 Ω an beiden Enden, geschirmtes verdrilltes Paar. Bei ca. 5–10 m Leitung unkritisch.
- Gemeinsame Parameter wählen (Baudrate, Parität). Waveshare ab Werk 9600 Baud, E+E nennt 9600 mit 8E1 für das eigene Protokoll. Parität für Modbus RTU bei E+E prüfen *(offen)*.
- Eine einzige Quelle (Modbus-over-Serial-Line-Spezifikation) wurde nur überflogen, die Grenzwerte (Knotenzahl, Länge) sind für diese Anlage nicht kritisch.

## 5. Harvia-Kennlinie wiederverwenden?

Nein. Der WX232 misst über einen NTC 22 kΩ plus Überhitzungsschutz in einem Messkreis. Die Kennlinie ist in den Quellen nicht dokumentiert *(offen)*, und ein Widerstandseingang fehlt (StampPLC) bzw. würde auf dem 0–10-V-Eingang des A176 eine Eigenbau-Beschaltung brauchen. Pt1000 mit Umformer ist einfacher und dokumentiert.

## Offene Punkte

- Preise: HTS801, E+E CO₂-Fühler, Waveshare 8CH, Seneca Z-4RTD2.
- Maximale Fühlertemperatur des Pt1000-Kabelfühlers Ø 6 mm (Art. prüfen).
- Messbereich und Betriebstemperatur EE850.
- Kanaltemperatur der Abluft im Betrieb.
- Parität/Protokoll E+E im Modbus-Betrieb.
- Duftstofftauglichkeit der Feuchtesonde.

## Quellen

- E+E Elektronik: EE33 Datenblatt und Produktseite, HTS801, CO₂-Sonden und -Transmitter (EE850, CDS201, EE820, EE872, EE895).
- Sensirion: SHT4x Datenblatt (Version 7.3, Juni 2026) und Hinweis zum Feuchte-Creep.
- Galltec + Mela: Produktseite DI – Indoor.
- Sensorshop24: Pt1000/Pt100-Hutschienenumformer, Silikonleitung, Kabelfühler.
- Waveshare: Wiki Modbus RTU Analog Input 8CH.
- Seneca: Produktseite Z-4RTD2 (ohne technische Daten).
