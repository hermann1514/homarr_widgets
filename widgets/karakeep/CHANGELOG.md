# Changelog – Karakeep

Alle nennenswerten Änderungen an diesem Widget werden in dieser Datei dokumentiert.

Das Format orientiert sich an [Keep a Changelog](https://keepachangelog.com/de/1.1.0/),
die Versionierung an [Semantic Versioning](https://semver.org/lang/de/).

## [1.1.0] – 2026-11-13

### Geändert
- Die Lesezeichen werden in einem eigenen scrollbaren Bereich (`ScrollArea`) angezeigt. Die Überschrift bleibt immer sichtbar.
- Es werden jetzt die 20 neuesten Lesezeichen geladen (vorher 5).

## [1.0.0] – 2026-11-04

### Hinzugefügt
- Erste Version: die 5 neuesten Lesezeichen mit Vorschaubild, Titel, Beschreibung und Datum.
- Eigene [README](README.md) mit Einrichtung des API-Schlüssels.

### Geändert (gegenüber dem ersten Homarr-Export)
- Die Basis-URL ist jetzt der Platzhalter `https://karakeep.example.com`. Die feste Adresse im Template ist entfernt.
- Archivierte Lesezeichen werden nicht mehr angezeigt.
- Notizen zeigen ihren Text und Dateien ihren Dateinamen. Bisher stand dort „-“, und der Eintrag verlinkte auf die Startseite.
- Lange Titel werden korrekt gekürzt.
- Links öffnen mit `rel="noopener noreferrer"` und ohne Unterstreichung der ganzen Karte.
- Das Datum erscheint in Ortszeit (Europe/Berlin) statt aus der UTC-Zeit abgeschnitten.
