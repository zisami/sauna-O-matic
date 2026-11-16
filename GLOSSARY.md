# Glossar

**Aufguss**: Einzelnes Ereignis, bei dem Wasser (optional mit Duft) auf die Saunasteine gegeben wird, optional gefolgt von Wacheln.

**Aufguss-Schema**: Gespeicherte Parameter eines Aufgusses: Wassermenge, Duft und Dosierung, Wachelprofil, zeitlicher Ablauf.

**Wacheln**: Verteilen des Aufgussdampfes in der Kabine durch bewegte Luft.

**Saunagang**: Phase mit Solltemperatur und Dauer, in der Aufgüsse nach Aufguss-Schema stattfinden.

**Saunaabend**: Gespeicherte Abfolge aus Aufheizen, Saunagängen und Ruhephasen bis zum Abschalten.

**Sicherheitskette**: Fest verdrahtete Abschaltung des Ofens (STB, Not-Aus), die unabhängig von jeder Software wirkt. Die Steuerung kann sie nur beobachten, nie überbrücken oder zurücksetzen.

**STB** (Sicherheitstemperaturbegrenzer): Bimetall-Schalter über dem Ofen, der bei Übertemperatur auslöst und nur von Hand zurückgesetzt werden kann.

**Hauptschalteinheit**: Verteilerkasten mit der Steuerung, den Leistungsschaltern und allen festen Verkabelungen.

**Bedieneinheit**: Rolle eines separaten Bediengeräts außerhalb der Kabine (z. B. Tab5, Dial, Tough); es können mehrere sein. Eine Bedieneinheit ist entweder *lokal* (fest in Kabinennähe) oder *fern*.

**Scharfschalten**: Lokale Handlung an einer lokalen Bedieneinheit oder am Taster, ohne die kein Fernstart möglich ist.

**Fernstart**: Start eines Saunaabends über die PWA oder Home Assistant, nur nach vorherigem Scharfschalten.

**Sicherheitsschütz**: Schütz, das den Ofen allpolig trennt, sobald die Sicherheitskette öffnet. Es schaltet nur bei Start, Ende und Abschaltung, nie zur Regelung.

**Regelschütz**: Schütz, das den Ofen zur Temperaturregelung ein- und ausschaltet. Es liegt in Reihe zum Sicherheitsschütz; ein Fehler am Regelschütz darf die Sicherheitskette nicht aushebeln.
