# Plattform: Fähigkeiten von StampPLC und Tab5

Recherche zu Issue #7. Stand: 13.11.2026. Alle Angaben sind aus Primärquellen (M5Stack-Doku/-Shop, ESPHome-Doku) belegt, siehe Quellenliste. Schlussfolgerungen ohne direkte Quelle sind als **(Annahme)** markiert.

> Hinweis zur Schreibweise: Das Produkt heißt bei M5Stack offiziell **StamPLC** (SKU K141); im Projekt wird „StampPLC“ verwendet.

## Kurzantwort

- **StamPLC-Grundgerät:** 4 Relaisausgänge (AC 250 V / 5 A, DC 28 V / 5 A), 8 optoisolierte Digitaleingänge (DC 5–36 V, ein gemeinsamer COM), **keine** Analogein- oder -ausgänge. [Q1]
- **Relaisdetails** (Kontaktart Schließer/Wechsler, Typ, Lebensdauer) stehen für die 4 Basisrelais **nicht** in der Doku. Für die Schütz-Ansteuerung der Sauna sind das Koppelrelais, nicht die Lastschalter. [Q1]
- **Versorgung StamPLC:** DC 6–36 V, 1 A über DC5521-Buchse; Betrieb ca. 48 mA @ 12 V. Montage auf Hutschiene, 72 × 80 × 33,4 mm, Betriebstemperatur 0–40 °C. [Q1]
- **Bus StamPLC:** RS485 (SIT3088, Stecker HT3.96-4P) und CAN (SIT1044, XT30). Galvanische Trennung und Abschlusswiderstand sind **nicht dokumentiert**; die Versorgungspins beider Stecker liegen direkt auf VIN. [Q1]
- **Analog:** Nur über das **IO-Modul for StamPLC** (A176): 2 isolierte Analog**eingänge** 0–10 V / 0–20 mA, 2 MOSFET-Ausgänge, 1 Wechsler-Relais. Ein 0–10-V-**Ausgang** existiert im StamPLC-Ökosystem nicht. [Q3]
- **Ethernet für StamPLC:** **PoE-Modul for StamPLC** (A165, W5500 über SPI, RJ45, PoE 802.3af oder 12 V/5 V), Preis USD 18,50. In ESPHome wird es als `ethernet: type: W5500` unterstützt; ein bekannter Reset-/INT-Leitungskonflikt mit dem Basisgerät muss umgangen werden. [Q2][Q12]
- **WLAN im Metallverteiler:** Der StamPLC hat nur die Onboard-Antenne des Stamp-S3A, ohne Anschluss für eine externe Antenne **(Annahme aus Q1, kein Antennenport dokumentiert)**. Im Metallschrank deshalb Ethernet (PoE-Modul) nutzen.
- **Tab5:** ESP32-P4 + ESP32-C6 (Wi-Fi 6 2,4 GHz, Thread, Zigbee), 5″-Touch 1280 × 720, RS485 mit schaltbarem 120-Ω-Abschluss, USB-A/USB-C, **kein Ethernet**. Interne 3D-Antenne plus 2 × MMCX für externe Antennen. [Q5][Q6]
- **Tab5 im Dauerbetrieb:** Versorgung über USB-C oder über den RS485-Stecker (6–24 V); Akku NP-F550 optional. Wandmontage nur über ein 1/4″-20-Stativgewinde, eine Wandhalterung bietet M5Stack nicht an. Betriebstemperatur 0–40 °C, also nicht in die Saunakabine. [Q5][Q6]
- **ESPHome:** Der Tab5 wird offiziell unterstützt: ESP32-P4 nur mit ESP-IDF, WLAN über `esp32_hosted`, Display über `mipi_dsi` (Modell `M5STACK-TAB5`), LVGL 9. [Q7][Q8][Q9][Q10]
- **ESPHome auf dem StamPLC:** Läuft (ESP32-S3), aber der I/O-Expander AW9523 hat **keine** native ESPHome-Komponente. Nötig sind externe Community-Komponenten (Beormund/esphome-m5stamplc). [Q11][Q13]
- **Werks-Firmware StamPLC:** arbeitet als Modbus-RTU-Slave auf RS485 (Coils 0–3 = Relais, Input-Register 0–7 = Eingänge). Das ist nützlich zum Testen, aber für Sensoren als Master braucht es eigene Firmware. [Q1]
- **Preise** (M5Stack-Shop, USD, 13.11.2026): StamPLC 42,90; PoE-Modul 18,50; IO-Modul 19,95; AC/DC-Modul 12,95; Tab5-Kit mit Akku 60,00. EUR-Preise bei Berrybase waren nicht automatisch abrufbar (offener Punkt). [Q1–Q4][Q6]

## Details

### I/O-Tabelle StamPLC (K141)

| Funktion | Ausführung | Kennwerte | Quelle |
|---|---|---|---|
| MCU | Stamp-S3A (ESP32-S3FN8) | 240 MHz Dual-Core, 8 MB Flash, Wi-Fi 2,4 GHz | Q1 |
| Relaisausgänge | 4 Stück, über I2C-Expander AW9523B (0x59) | AC 5 A @ 250 V, DC 5 A @ 28 V; Kontaktart/Lebensdauer nicht angegeben | Q1 |
| Digitaleingänge | 8 Stück, Optokoppler EL3H4, über AW9523B | DC 5–36 V, ein gemeinsamer COM (Low- oder High-Level-Beschaltung) | Q1 |
| Analogeingänge | – | nicht vorhanden | Q1 |
| Analogausgänge | – | nicht vorhanden | Q1 |
| RS485 | PWR-485, HT3.96-4P, SIT3088EEUA | Isolation/Abschluss nicht dokumentiert; Versorgungspin = VIN | Q1 |
| CAN | PWR-CAN, XT30(2+2)PW-M, SIT1044QTK | Isolation/Abschluss nicht dokumentiert; Versorgungspin = VIN | Q1 |
| Versorgung | DC5521 (Mitte +) | DC 6–36 V @ 1 A; Standby 15 mA @ 12 V, Betrieb 48 mA @ 12 V | Q1 |
| Erweiterung | 2 × HY2.0-4P (Port A: G2/G1, Port C: G5/G4), StamPLC-Bus 2 × 8 | 5 V @ ca. 700 mA belastbar | Q1 |
| Onboard | 1,14″-LCD (ST7789V2), 3 Tasten, Buzzer, microSD, LM75 (0x48), INA226 (0x40), RTC RX8130CE (0x32), PI4IOE5V6408 (0x43) | – | Q1 |
| Mechanik | Hutschiene | 72 × 80 × 33,4 mm, 138,9 g, 0–40 °C | Q1 |

**StamPLC-Bus (2 × 8 Header)** [Q1]

| Pin | Signal | Pin | Signal |
|---|---|---|---|
| 1 | VIN | 2 | GND |
| 3 | GND | 4 | GND |
| 5 | GND | 6 | 5V_OUT |
| 7 | SCL (G15) | 8 | SDA (G13) |
| 9 | PHY_RST (G3) | 10 | INT (G14) |
| 11 | MISO (G9) | 12 | SCK (G7) |
| 13 | MOSI (G8) | 14 | CS (G11) |
| 15 | G40 | 16 | G41 |

**Werks-Firmware als Modbus-RTU-Slave (PWR-485)** [Q1]

| Adressbereich | Inhalt |
|---|---|
| Coils 0–3 | Relais 1–4 |
| Input-Register 0–7 | Digitaleingänge 1–8 |
| Input-Register 8–9 | Temperatur (Float) |
| Input-Register 10–11 | Busspannung |
| Input-Register 12–13 | Shunt-Strom |

### Erweiterungen für den StamPLC

| Modul | SKU | Funktion | Kennwerte | Preis (M5Stack-Shop, 13.11.2026) | Quelle |
|---|---|---|---|---|---|
| PoE Module for StamPLC | A165 | Ethernet 10/100 (W5500, SPI), RJ45 | PoE IEEE 802.3af (37–57 V, max. 13 W) oder DC 12 V / 5 V; Hutschiene | USD 18,50 | Q2 |
| IO Module for StamPLC | A176 | 2 × AI 0–10 V / 0–20 mA, 2 × MOSFET-Ausgang, 1 × Relais | galvanisch getrennt (CA-IS3020S, B0505M); STM32G030; I2C 0x20–0x2F per DIP-Schalter, also stapelbar; MOSFET DC 0–60 V / 2,5 A; Relais SRD-5VDC-SL-C (Wechsler) AC 250 V / DC 28 V 5 A | USD 19,95 | Q3 |
| AC/DC Power Supply Module for StamPLC | A160 | Netzteil 100–240 V AC auf 12 V / 1 A, plus 1 Leistungsrelais | isoliert (WCAL12-S12CA); Relais HF3FF/005-1HTF, Schließer, AC 240 V / 10 A, ≥ 100.000 Zyklen (ohmsche Last); Ansteuerung über PI4IOE5V6408 (0x44) | USD 12,95 | Q4 |

Hinweise:
- **PoE-Modul:** Die RST-Leitungen von LCD, internem PI4IOE und PoE-Modul sind verbunden. Deshalb im Ethernet-Treiber RST auf „none“ (−1) setzen. INT ist mit dem PI4IOE-INT geteilt, deshalb den PI4IOE-Interrupt maskieren (Register 0x11 = 0xFF). [Q2]
- **AC/DC-Modul:** Nicht gleichzeitig eine andere Versorgung als 12 V auf StamPLC-Seite anschließen. [Q4]
- **Analogausgang:** Ein 0–10-V-Ausgang ist im StamPLC-Zubehör nicht vorhanden. Bei Bedarf wäre ein externer Modbus-Analogausgang am RS485 nötig **(Annahme)**.

### Ethernet und WLAN im Metallverteiler

| Gerät | Ethernet | WLAN | Externe Antenne | Quelle |
|---|---|---|---|---|
| StamPLC | nur mit PoE-Modul A165 (W5500) | ESP32-S3, 2,4 GHz | kein Port dokumentiert | Q1, Q2 |
| Tab5 | nein (kein PHY verbaut) | ESP32-C6 über SDIO, Wi-Fi 6 2,4 GHz | 2 × MMCX, per PI4IOE umschaltbar | Q5, Q6 |

ESPHome mit W5500: `ethernet: type: W5500` mit `clk_pin`, `mosi_pin`, `miso_pin`, `cs_pin`. `interrupt_pin` oder `polling_interval` sind möglich; `reset_pin` ist optional, was den Reset-Konflikt umgeht. Ein gemeinsamer SPI-Bus wird über `spi_id` angegeben. Wenn WLAN und Ethernet gleichzeitig laufen, braucht es die Priorisierung unter `network:`. [Q12]
Für den StamPLC wären das CLK G7, MOSI G8, MISO G9, CS G11 und INT G14 (laut Bus-Pinout aus Q1) **(Annahme, noch nicht getestet)**.

### Tab5 (C145 ohne Akku / K145 Kit mit NP-F550)

| Merkmal | Wert | Quelle |
|---|---|---|
| SoC | ESP32-P4NRW32, RISC-V Dual-Core (Doku: 360 MHz, Shop: 400 MHz), 16 MB Flash, 32 MB PSRAM | Q5, Q6 |
| Funk | ESP32-C6-MINI-1U (SDIO): Wi-Fi 6 2,4 GHz, Thread, Zigbee; 3D-Antenne + 2 × MMCX | Q5, Q6 |
| Display | 5″ IPS 1280 × 720, MIPI-DSI, kapazitiver Touch; Treiber ST7123 (ab 14.10.2025) bzw. ST7121 (ab 28.04.2026), davor ILI9881C + GT911 | Q5, Q6 |
| Kamera / Audio | SC2356 2 MP; ES8388 + ES7210, 2 Mikrofone, 1-W-Lautsprecher, 3,5-mm-Klinke | Q6 |
| RS485 | SIT3088, schaltbarer 120-Ω-Abschluss, Stecker HC1.25-6P, Versorgungsbereich 6–24 V | Q5, Q6 |
| USB | USB-A (Host), USB-C (USB 2.0 OTG) | Q5, Q6 |
| Erweiterung | HY2.0-4P (G53/G54), M5-Bus, GPIO_EXT 2,54-10P, microSD, Stamp-Pads (Cat-M, NB-IoT, LoRaWAN) | Q5, Q6 |
| Ethernet | nicht vorhanden | Q5, Q6 |
| Akku | NP-F550, 7,4 V, 2000 mAh (14,8 Wh), ca. 6 h; Laden (IP2326) nur im eingeschalteten Zustand; Schutzabschaltung unter 6 V; INA226-Überwachung | Q5, Q6 |
| Montage | 1/4″-20-Stativmutter seitlich; keine Wandhalterung | Q6 |
| Umgebung | 0–40 °C | Q5, Q6 |
| Maße | 128 × 80 × 12 mm (ohne Akku), 26,7 mm Tiefe mit Akku | Q6 |
| Preis | K145 mit NP-F550: USD 60,00 (M5Stack-Shop, 13.11.2026) | Q6 |

Hinweise für den Dauerbetrieb:
- Vor dem Trennen der Versorgung herunterfahren. Nach einem harten Stromverlust 5 s warten, bevor neu gestartet wird. [Q5]
- Für eine fest montierte Wandeinheit ist die Variante **ohne Akku** (C145) mit fester Versorgung über USB-C oder RS485 (6–24 V) naheliegend. Ein Li-Ionen-Akku im Dauerbetrieb an einem warmen Ort ist zu vermeiden **(Annahme)**.

### Firmware-Support-Matrix

| Plattform | ESPHome | ESP-IDF | Arduino | UiFlow2 | PlatformIO | LVGL |
|---|---|---|---|---|---|---|
| StamPLC (ESP32-S3) | ja, ESP32-S3; AW9523, LM75B und RX8130 nur über externe Community-Komponenten [Q13] | ja (M5StamPLC-UserDemo) [Q1] | ja (M5StamPLC-Library) [Q1] | ja [Q1] | ja [Q1] | (nicht relevant, 1,14″-LCD) |
| PoE-Modul (W5500) | ja, `ethernet: type: W5500` [Q12] | ja, esp_eth W5500 (Demo Stamplc-poe-demo) [Q2] | ja [Q2] | ja [Q2] | – | – |
| IO-Modul (A176) | keine native Komponente bekannt **(offen)** | per I2C-Register | ja (laut Shop) [Q3] | ja [Q3] | – | – |
| Tab5 (ESP32-P4) | ja, **nur ESP-IDF** [Q7]; WLAN über `esp32_hosted` (C6, SDIO) [Q8]; Display `mipi_dsi` Modell `M5STACK-TAB5`/`-ST7123`/`-ST7121` [Q9] | ja, BSP `espressif/m5stack_tab5`, M5Tab5-UserDemo [Q5] | ja, M5Unified ≥ 0.2.23, M5GFX ≥ 0.2.30 [Q5] | ja [Q5] | ja [Q5] | ESPHome-LVGL (LVGL 9, PPA-Rotation auf dem P4) [Q10]; in IDF über den BSP |

Details zur ESPHome-Konfiguration des StamPLC aus dem Community-Projekt [Q13]:
- Framework esp-idf, ESP32-S3 mit 8 MB Flash.
- I2C: SDA GPIO13, SCL GPIO15. SPI: CLK 7, MOSI 8, MISO 9.
- RS485-UART: TX GPIO0, RX GPIO39.
- Relais auf AW9523-Pins 0–3, Eingänge auf Pins 4–7 und 12–15.
- GPIO3 muss beim Booten per `power_supply` auf HIGH gehalten werden.
- Display über `mipi_spi` ST7789V; INA226 mit Shunt 0,01 Ω.

ESPHome mit `esp32_hosted` auf dem Tab5: Bei großen LVGL-Oberflächen wird `use_psram` empfohlen. [Q8]

## Offene Punkte / Unsicherheiten

1. **Basisrelais StamPLC:** Kontaktart (Schließer oder Wechsler), Relaistyp und Lebensdauer sind nicht dokumentiert. Das Schaltbild-PDF ließ sich nicht auswerten. → Schaltbild manuell prüfen oder Relais am Gerät ablesen.
2. **RS485/CAN am StamPLC:** galvanische Trennung und Abschlusswiderstand sind nicht dokumentiert. SIT3088 und SIT1044 sind übliche **nicht isolierte** Transceiver **(Annahme, gegen Datenblatt prüfen)**. Bei 5 m Leitung zum Ofen bzw. zu den Sensoren eventuell einen externen Abschluss bzw. Trenner vorsehen.
3. **Reset-/INT-Konflikt PoE-Modul:** Die Workarounds stammen aus der M5Stack-Doku [Q2], sind aber für ESPHome (Zusammenspiel mit `power_supply` auf GPIO3 aus Q13) **nicht verifiziert**.
4. **ESPHome AW9523:** Nur als externe Community-Komponente verfügbar (20 Stars, Einzelentwickler) → Wartungsrisiko. Alternativen: eigene Komponente, Arduino/IDF oder Werks-Firmware als Modbus-Slave.
5. **IO-Modul in ESPHome:** Unterstützung ist unklar.
6. **Home-Assistant-Seite von M5Stack zum StamPLC** (docs.m5stack.com/en/homeassistant/devices/stamplc) konnte nicht geladen werden.
7. **EUR-Preise:** Berrybase (.at/.de) liefert bei automatischem Abruf nur den Cookie-Banner. Die Preise oben sind USD vom M5Stack-Shop ohne Versand, Zoll und Einfuhrumsatzsteuer → manuell auf berrybase.at prüfen.
8. **Tab5-Taktfrequenz:** Die Doku nennt 360 MHz, der Shop 400 MHz. Für das Projekt nicht relevant.
9. **Temperaturbereich 0–40 °C** bei beiden Geräten: gilt für die Werkstatt bzw. den Vorraum, nicht für die Kabine.

## Quellen

- [Q1] M5Stack Docs – StamPLC: https://docs.m5stack.com/en/core/StamPLC · Shop: https://shop.m5stack.com/products/m5stamp-plc-controller-with-m5stamps3 · Arduino-Lib: https://github.com/m5stack/M5StamPLC · Werks-Firmware: https://github.com/m5stack/M5StamPLC-UserDemo
- [Q2] M5Stack Docs – StamPLC PoE: https://docs.m5stack.com/en/stamplc/StamPLC_PoE · Shop: https://shop.m5stack.com/products/poe-module-for-stamplc-w5500 · Demo: https://github.com/Ocean-lhy/Stamplc-poe-demo
- [Q3] M5Stack Shop – IO Module for StamPLC: https://shop.m5stack.com/products/io-module-for-stamplc-ina226aidgsr
- [Q4] M5Stack Shop – AC/DC Power Supply Module for StamPLC: https://shop.m5stack.com/products/ac-dc-power-supply-module-for-stamplc-12v-1a
- [Q5] M5Stack Docs – Tab5: https://docs.m5stack.com/en/core/Tab5 · Werks-Firmware: https://github.com/m5stack/M5Tab5-UserDemo
- [Q6] M5Stack Shop – Tab5 IoT Development Kit: https://shop.m5stack.com/products/m5stack-tab5-iot-development-kit-esp32-p4
- [Q7] ESPHome – ESP32 Platform: https://esphome.io/components/esp32/
- [Q8] ESPHome – ESP32 Hosted: https://esphome.io/components/esp32_hosted/
- [Q9] ESPHome – MIPI DSI Display: https://esphome.io/components/display/mipi_dsi/
- [Q10] ESPHome – LVGL: https://esphome.io/components/lvgl/
- [Q11] ESPHome – Komponentenliste (PI4IOE5V6408 vorhanden, AW9523 nicht): https://esphome.io/components/
- [Q12] ESPHome – Ethernet: https://esphome.io/components/ethernet/
- [Q13] Community – Beormund/esphome-m5stamplc: https://github.com/Beormund/esphome-m5stamplc
