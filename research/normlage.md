# Normlage: Anforderungen an eine Saunasteuerung mit Fernstart (privat, Österreich)

Ticket: [#9](https://github.com/zisami/sauna-O-matic/issues/9)

**Hinweis:** Das ist eine technische Auswertung frei verfügbarer Normauszüge, keine Rechtsberatung. Die Normtexte selbst (EN 60335-2-53, OVE E 8101 Teil 7-703) sind kostenpflichtig. Verfügbar waren nur Vorschauen, Änderung A2:2023 im Volltext (4 Seiten), das Harvia-Handbuch (Fenix/Xenio) und der Anwendungsbereich plus Vorwort der OVE E 8101:2025. Alles, was nicht aus diesen Quellen belegt ist, ist als *(Schluss)* oder *(offen)* markiert.

## Kurzantwort

- Die wichtigste Norm ist EN 60335-2-53 (Saunaheizgeräte). Sie regelt das Produkt "Ofen plus Steuerung", nicht den Selbstbau. Sie liefert aber die beste Messlatte, wie eine Steuerung mit Fernstart aussehen muss.
- **Fernstart ist erlaubt**, aber nur mit festen Bedingungen: zwei unabhängige Handlungen zum Scharfschalten, sichtbare Anzeige, Türkontakt (außer der Ofen besteht den Abdeck-Test 19.101/19.102), Betriebszeit höchstens 6 h, kein automatischer Neustart.
- **Hardware-Pflicht** (aus Norm und Vorgabe "Sicherheitskette hart verdrahtet"): Übertemperaturschutz, Trenner, Türkontakt-Abschaltung. **Software darf**: Temperaturregelung, Zeitpläne, Aufguss, Licht, Lüfter, solange keine dieser Funktionen die Sicherheitskette überbrücken kann.
- Für die geplante Steuerung gilt: sie ist keine von Harvia freigegebene Steuerung. Die Prüfung und Garantie des Ofens decken sie nicht ab. Die Verantwortung liegt beim Errichter und bei der abnehmenden Elektrofachkraft.

## Anforderungsliste

Spalte "Art": **HW** = Hardware-Pflicht, **SW** = Software zulässig, **E** = Empfehlung.

| Nr. | Anforderung | Quelle | Art | Folge für die Steuerung |
|---|---|---|---|---|
| N1 | Betriebszeit im Haushalt höchstens 6 h, automatischer Neustart nicht erlaubt. Gerät braucht Zeitschaltung oder programmierbaren Timer. | EN 60335-2-53 22.103 (A2:2023) | HW oder SW *(Schluss)* | Saunaabend-Ablauf muss in jedem Fall spätestens nach 6 h stromlos enden und darf sich nicht selbst neu starten. **E:** zusätzlich ein unabhängiger Hardware-Timer (z. B. Zeitrelais im Schützkreis), falls die Software als Sicherheitsfunktion gelten soll. |
| N2 | "Programmierbarer Timer" ist definiert als Timer, der nach Ablauf mit einem weiteren Zyklus fortfährt. | 3.110 (A2) | Definition | Zeitpläne, die Zyklen aneinanderreihen, sind im Haushalt nicht zulässig, wenn sie ohne neues Einschalten weiterheizen. |
| N3 | Fernbetrieb (Standby-Modus für Fernbetrieb) und verzögertes Einschalten brauchen (a) eine sichtbare Anzeige, dass der Modus aktiv ist, und (b) **zwei unabhängige Handlungen** zur Aktivierung. | 22.110 (A2) | HW oder SW *(Schluss)* | Remote-Start nur zweistufig: erst "Fernstart freigeben" (zweite, getrennte Bestätigung), dann Heizen. Anzeige auf Tab5 und besser zusätzlich eine echte Lampe. |
| N4 | Heizbetrieb muss angezeigt werden (Kontrolllampe, Symbol oder Display). | 22.110 (A2) | SW | Tab5-Symbol genügt dem Wortlaut. **E:** Lampe am Verteiler oder an der Kabinentür, weil die Anzeige auch bei Ausfall des Tab5 stimmen sollte. |
| N5 | Bei Standby für Fernbetrieb muss die Kabinentür einen Türschalter haben, der den Fernbetrieb abschaltet, wenn die Tür geöffnet wird. Ausnahme: der Ofen erfüllt die Tests 19.101 oder 19.102 (Abdeck-/Brandtest). | 7.12.1 und 22.108 (A2) | HW *(Schluss, ob Pflicht, hängt vom Ofen ab)* | **E:** Türkontakt immer einbauen, in die Sicherheitskette (Fernbetrieb fällt bei Türöffnung ab). Glastür: Magnetkontakt an der Zarge, außerhalb der Heißzone. Zu klären, ob die PO11 19.101/19.102 besteht (offen). |
| N6 | Anleitung muss besagen, dass die Kabine vor dem Einschalten oder vor dem Neustart eines Timers zu prüfen ist. | 7.12 (A2), Harvia-Handbuch 2.3 | organisatorisch | Hinweistext in Web-UI/Tab5 beim Freigeben des Fernstarts. |
| N7 | Die Anleitung des Ofens nennt die zulässigen Steuerungen und Steuerplatinen (Typ/Modell). Der Ofen wird mit diesen geprüft. | 5.9 und 7.12.1 (A2), 3.102, 3.109 | Konformität | Eine Eigenbau-Steuerung ist keine genannte Steuerung. Die Prüfabdeckung des Ofens gilt nicht automatisch. Siehe Abschnitt "Folgen für den Selbstbau". |
| N8 | Harvia-Steuerungen dürfen nur Öfen steuern, die den Test 19.101 und 19.102 bestehen, sonst ist eine Schutzvorkehrung nötig (Sicherheits- oder Türschalter). | Harvia-Handbuch Fenix/Xenio, 3.3 | HW | Gleiche Logik wie N5, dort ausdrücklich genannt. |
| N9 | Allpolig trennende Trenneinrichtung (Überspannungskategorie III) vor Ort. | Harvia-Handbuch 2.2 und 6 | HW | Hauptschalter 3P+N im Verteiler vor Schützen und Steuerung. Passt zur ohnehin geplanten Verteilung. |
| N10 | Fühlerleitung getrennt von Netz- und Steuerleitungen verlegen. | Harvia-Handbuch 6 | HW | Für den Fühler WX232 (Messkreis), siehe Ticket Harvia PO11. |
| N11 | 6 Luftwechsel je Stunde in der Kabine. | Harvia-Handbuch 3.3 | E / Auslegung | Mindest-Lüftungsleistung als Vorgabe an die Lüfterregelung (Ticket Lüfterregelung). |
| N12 | Ohne Timer/Dauerbetrieb nur beaufsichtigt (öffentliche Saunen). "Beaufsichtigt" heißt: anwesend oder anwesend plus Fernansicht der Betriebsdaten. | 22.103 und 3.111 (A2) | nicht anwendbar | Privat: es gilt der Timer-Weg (N1). Fernansicht allein macht den Betrieb im Haushalt nicht "beaufsichtigt". |
| N13 | RCD vom Typ AC dürfen im Anwendungsbereich der OVE E 8101:2025 nicht mehr verwendet werden (neu in Teil 5-53). | OVE E 8101:2025 Vorwort | HW | Vorhandenen FI prüfen: mindestens Typ A. Hängt von Stellglied ab (siehe Ticket Leistungsstellung). |
| N14 | Die OVE E 8101 gilt für alle festen Verdrahtungen und Leitungsanlagen, auch für Steuerung und Meldung, aber nicht für die innere Verdrahtung von Geräten. Sie gilt nicht für "elektrische Ausrüstung von Maschinen". | OVE E 8101:2025 Anwendungsbereich 11.2 c) bis e), 11.3 k) | Geltung | Die feste Verdrahtung im Verteiler und zur Kabine fällt darunter, eine Sauna ist nach dem Wortlaut keine Maschine *(Schluss)*. Teil 7-703 "Räume und Kabinen mit Saunaheizgeräten" ist einschlägig (Inhalt nicht frei verfügbar, siehe offen). |
| N15 | Prüfung (Teil 6, Erst- und wiederkehrende Prüfung) und Dokumentation der Anlage (132.13) sind Teil der Norm. | OVE E 8101:2025 Inhaltsverzeichnis | organisatorisch | Stromlaufplan und Dokumentation sind Teil der Spezifikation. Abnahme durch Elektrofachkraft einplanen. |
| N16 | Software als Teil einer Sicherheitsfunktion wird nach Anhang R (EN 60335-1) bewertet. Inhalt nicht verfügbar. | EN 60335-2-53 Inhaltsverzeichnis (Anhang R) | *(offen)* | Bestätigt die Vorgabe: Sicherheitsfunktionen nicht in die Software legen, dann ist Anhang R gegenstandslos. |

## Folgen für die Funktionen

- **Fernstart / Home Assistant:** erlaubt, wenn N3, N5, N6 erfüllt sind. Konkret: Freigabe-Schritt mit Bestätigung, Anzeige, Türkontakt-Abfall, Zeitbegrenzung. Reines "Heizen per Knopfdruck aus dem Büro" ohne diese Elemente ist nicht normkonform.
- **Zeitplan / Saunaabend:** Der Ablauf braucht ein festes Ende und höchstens 6 h Heizzeit (N1, N2). Ein vorprogrammierter Start (verzögertes Einschalten) fällt unter N3.
- **Anwesenheit:** Keine Normpflicht im Privathaushalt. Kann als zusätzliche Abschaltlogik dienen (Empfehlung), ersetzt aber weder Timer noch Türkontakt (N12).
- **Automatischer Aufguss:** In den verfügbaren Quellen ist keine Anforderung für Wasserdosierung auf die Steine zu finden. Die Norm behandelt Saunaöfen mit Verdampfer/Befeuchter (dort wird zusätzlich 60335-2-98 "soweit sinnvoll" herangezogen), nicht Wasser auf den Steinen eines normalen Ofens. *(offen)* Konsequenz für die Spezifikation: Dosiermenge begrenzen, nur bei laufendem Ofen, nie bei ausgelöstem Überhitzungsschutz (Empfehlung).
- **Wacheln:** Keine Norm gefunden *(offen)*. Mechanik absichern (Berührschutz, Abschaltung bei Türöffnung) ist Empfehlung.
- **Lüfter:** siehe N11. Lüfter-Ausfall darf die Sicherheitskette nicht beeinflussen.

## Folgen für den Selbstbau

- *(Schluss)* Die EN 60335-2-53 prüft das in Verkehr gebrachte Gerät. Eine für den Eigengebrauch gebaute Steuerung wird nicht in Verkehr gebracht, es entfällt eine CE-Kennzeichnung. Die Anforderungen gelten dann nicht rechtlich, aber sinngemäß als Stand der Technik und als Maßstab bei Schaden und Versicherung.
- *(Schluss)* Ofen und Eigenbau-Steuerung bilden eine Kombination, die Harvia nicht geprüft hat (N7). Garantie und Haftung des Herstellers bleiben dann unsicher. Das Ofen-Handbuch listet C150, C150VKK, C260, Griffin. Sogar die gängigen CS170/CX170 stehen nicht in dieser Liste (siehe Ticket Harvia PO11).
- *(offen)* Anforderungen des österreichischen Elektrotechnikrechts (wer darf was errichten, Abnahme, Befund). Nicht recherchiert, mit Elektrofachkraft klären. Das ist ohnehin Pflicht für den Anschluss mit 400 V.
- *(offen)* Anforderungen an den Verteilerkasten als Schaltgerätekombination (Norm für Verteiler). Nicht recherchiert.
- **E:** Die Elektrofachkraft verdrahtet und nimmt ab: Verteiler, Trenner, FI, Sicherheitskette, Ofenanschluss. Der Eigenbau liefert Logik (24 V/SELV-Seite), Software und Gehäuse.

## Offene Punkte

1. Besteht die PO11 die Tests 19.101/19.102? Dann entfällt die Türkontakt-Pflicht für Fernbetrieb (7.12.1, 22.108). Quelle: Typenschild, Konformitätserklärung oder Harvia-Support. Bis zur Klärung Türkontakt vorsehen.
2. Textstände nicht verfügbar: 19.101/19.102 im Wortlaut, 22.108 (elektronische Türverriegelung), Anhang R, OVE E 8101 Teil 7-703 (u. a. Zonen, Leitungstemperaturen, Schutz durch FI). Kauf oder Einsicht über die Elektrofachkraft.
3. Zu prüfen: Typ des vorhandenen FI (N13).
4. Österreichisches Elektrotechnikrecht und Verteilernorm, siehe oben.
5. Wasserdosierung und mechanisches Wacheln: Risikobewertung selbst nötig, weil keine Norm gefunden wurde.

## Quellen

- EN 60335-2-53:2011/A2:2023, Vorschau mit Volltext der Änderung (iTeh): https://cdn.standards.iteh.ai/samples/73717/f060170943b64e5c8d85460a7a798619/SIST-EN-60335-2-53-2012-A2-2023.pdf
- IEC 60335-2-53:2011+AMD1:2017+AMD2:2021 CSV, Vorschau (nur Inhaltsverzeichnis, Vorwort, Anwendungsbereich): https://cdn.standards.iteh.ai/samples/18045/a2ee263a9cb24e29b5a757ee14b3c379/IEC-60335-2-53-2011.pdf
- OVE E 8101:2025-10-01, Anwendungsbereich, Inhaltsverzeichnis, Vorwort (ove.at)
- Harvia Fenix / Xenio Installations- und Bedienungsanleitung (Y05-1462)
- Hinweis: Ausgabe 5.0 der IEC 60335-2-53 (2026) ist angekündigt; diese Auswertung kennt nur Ausgabe 4.2.
