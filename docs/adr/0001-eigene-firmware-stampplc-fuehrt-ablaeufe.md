# Eigene Firmware, StampPLC führt alle Abläufe

Der StampPLC ist die einzige Instanz, die Saunaabend, Saunagang, Aufguss-Schema und Regelung ausführt und deren Daten speichert. Er läuft mit eigener Firmware (ESP-IDF/PlatformIO, C++) statt ESPHome und bietet eine versionierte API (REST + WebSocket), die PWA und alle Bedieneinheiten nutzen; Home Assistant hängt nur über einen MQTT-Adapter (Discovery) daran.

ESPHome hätte Modbus, HA-Anbindung und OTA fertig geliefert, aber die Ablauf-Engine mit gespeicherten Schemata, die Sicherheitsfunktionen (Wiederanlaufsperre, Schütz-Überwachung) und mehrere gleichberechtigte Bedieneinheiten wären in YAML/Lambdas schwer test- und wartbar. Autarkie des StampPLC stellt sicher, dass der Ausfall von Bedieneinheit, HA oder Netz keinen Ablauf beeinflusst.

Siehe [Kommunikation und Firmware-Basis](https://github.com/zisami/sauna-O-matic/issues/20).
