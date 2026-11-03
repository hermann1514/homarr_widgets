# Changelog – Kopia Info

Alle nennenswerten Änderungen an diesem Widget werden in dieser Datei dokumentiert.

Das Format orientiert sich an [Keep a Changelog](https://keepachangelog.com/de/1.1.0/),
die Versionierung an [Semantic Versioning](https://semver.org/lang/de/).

## [1.0.0] – 2026-11-03

### Hinzugefügt
- Erste Version: Repository-Status, Anzahl der Quellen und pro Backup-Quelle eine Karte mit Pfad, Fehleranzahl, Status, Größe, Zeitplan sowie letztem und nächstem Snapshot.
- Eigene [README](README.md) mit Einrichtung für Kopia mit und ohne Passwort.

### Geändert (gegenüber dem ersten Homarr-Export)
- Datei umbenannt von `Kopia Backups.json` nach `widgets/kopia-info/kopia-info.json`.
- Die Basis-URL ist jetzt der Platzhalter `https://kopia.example.com`.
- Die Authentifizierung ist auf *Basic* voreingestellt und lässt sich unter *Quellen konfigurieren* anpassen.
- Der Zeitplan zeigt neben Uhrzeiten auch Intervalle („alle 6 h“) und Cron-Ausdrücke. Bisher stand bei diesen Quellen fälschlich „manuell“.
- Letzter und nächster Snapshot erscheinen einheitlich in Ortszeit (Europe/Berlin). Bisher war ein Wert mit „UTC“ gekennzeichnet, der andere nicht.
- Größen erscheinen je nach Wert in MiB, GiB oder TiB und mit Dezimalkomma. Bisher wurde alles unter 1 GiB als „0.0 GiB“ angezeigt.
