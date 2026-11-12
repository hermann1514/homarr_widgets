# Changelog – HA Verlaufsdiagramm

Alle nennenswerten Änderungen an diesem Widget werden in dieser Datei dokumentiert.

Das Format orientiert sich an [Keep a Changelog](https://keepachangelog.com/de/1.1.0/),
die Versionierung an [Semantic Versioning](https://semver.org/lang/de/).

## [1.0.0] – 2026-11-12

### Hinzugefügt
- Erste Version: Verlauf von bis zu 8 Home-Assistant-Entitäten als Linie, Fläche, Stufe oder Balken.
- Daten aus der Langzeitstatistik von Home Assistant (`recorder.get_statistics`), Zeiträume von 1 Stunde bis 30 Tage.
- Pro Serie: Name, Farbe, Faktor und Offset, Einheit, Nachkommastellen, Linienstil, Füllung, zweite Y-Achse, Referenzlinie.
- Legende mit aktuellem Wert, Minimum, Durchschnitt und Maximum pro Serie.
- Eigene [README](README.md) mit Einrichtung, allen Einstellungen und drei Beispielkonfigurationen.
