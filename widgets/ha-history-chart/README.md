# HA Verlaufsdiagramm

Zeigt den Verlauf beliebiger Home-Assistant-Entitäten als Diagramm: Durchsatz, Temperatur, Leistung, Luftfeuchte usw. Welche Entitäten erscheinen und wie sie aussehen, legst du allein in den Widget-Einstellungen fest.

**Datei:** `ha-history-chart.homarr-widget.json` · **Version:** 1.0.0 · [Changelog](CHANGELOG.md)

> **Stand:** In Homarr getestet mit der Voreinstellung (Beispiel 1, 24 Stunden). Andere Zeiträume und Serienzahlen sind noch nicht in einer laufenden Instanz geprüft.

## Funktionen

- Bis zu 8 Serien pro Widget, als Linie, Fläche, Stufe oder Balken.
- Zeitraum 1 Stunde bis 30 Tage. Die Auflösung (5 Minuten, 1 Stunde, 1 Tag) wählt das Widget selbst oder du gibst sie vor.
- Pro Serie: Name, Farbe, Umrechnung mit Faktor und Offset, Einheit, Nachkommastellen, Linienstil, Füllung, zweite Y-Achse.
- **Referenzlinie:** zeichnet den aktuellen Wert einer Entität als waagerechte gestrichelte Linie, z. B. die maximale Leitungskapazität.
- Legende mit aktuellem Wert, Minimum, Durchschnitt und Maximum pro Serie.
- Fehlende Werte erscheinen als Lücke, nicht als Null.
- Farben und Hintergrund kommen aus dem Homarr-Theme (hell und dunkel).

## Voraussetzungen

- Homarr 2.x
- Home Assistant mit aktivem Recorder (Standard) und der Aktion `recorder.get_statistics`. Prüfen: *Entwicklerwerkzeuge → Aktionen*.
- Die gezeichneten Entitäten brauchen eine **Langzeitstatistik**, also das Attribut `state_class` (`measurement`, `total` oder `total_increasing`). Prüfen: *Entwicklerwerkzeuge → Statistik*. Entitäten ohne Statistik gehen nur als Referenzlinie.

## Einrichtung

1. **Token erstellen:** In Home Assistant unten links auf das eigene Profil klicken, dann *Sicherheit → Langlebige Zugangs-Token → Token erstellen*.
2. In Homarr *Benutzerdefinierte Widgets → Import* wählen und die JSON-Datei unverändert hochladen.
3. Über *⋯ → Quellen konfigurieren* folgende Werte eintragen:

   | Feld | Wert |
   |---|---|
   | Adresse | Adresse von Home Assistant ohne `/` am Ende, z. B. `https://homeassistant.example.com` |
   | Authentifizierung | *Bearer* (Voreinstellung) |
   | Token | das Token aus Schritt 1 |

4. Das Widget zu einem Board hinzufügen und die Einstellungen anpassen. Die Voreinstellung ist [Beispiel 1](#beispiel-1--fritzbox-durchsatz).

Das Token steht nicht in der JSON-Datei. Homarr speichert es verschlüsselt.

**Warum nicht die vorhandene Home-Assistant-Integration von Homarr?** Über eine Integration darf ein Custom Widget nur GET-Abfragen automatisch ausführen. Die Statistik-Abfrage ist ein POST. Deshalb braucht das Widget eine eigene Quelle mit Token.

## Einstellungen

### Allgemein und Daten

| Einstellung | Bedeutung | Standard |
|---|---|---|
| Titel | Überschrift des Widgets | `FRITZ!Box Durchsatz` |
| Icon | Icon neben dem Titel, leer = keines | leer |
| Serien (JSON) | Liste der Serien, siehe [unten](#serien) | Beispiel 1 |
| Zeitraum | 1 h, 6 h, 24 h, 48 h, 7 Tage, 30 Tage | 24 Stunden |
| Auflösung | Länge einer Periode: Automatisch, 5 Minuten, 1 Stunde, 1 Tag | Automatisch |
| Aggregation pro Periode | Mittelwert, Minimum, Maximum, Zählerstand, Zuwachs je Periode | Mittelwert |
| Aktualisierung (Sekunden) | Abstand der Abfragen. Die Statistik fragt das Widget höchstens alle 30 Sekunden ab. | `60` |
| Zeitzone | Für die Zeitachse und die Tagesgrenzen | `Europe/Berlin` |

**Aggregation:** *Mittelwert*, *Minimum* und *Maximum* gibt es für Messwerte (`state_class: measurement`). *Zählerstand* und *Zuwachs je Periode* gibt es nur für Zähler (`total`, `total_increasing`), z. B. Energie in kWh.

**Auflösung „Automatisch“:** Homarr begrenzt den Rechenaufwand eines Widgets. Das Widget wählt deshalb die feinste Auflösung, die sicher darunter bleibt. Sie hängt von der Zahl der gezeichneten Serien ab, Referenzlinien zählen nicht mit:

| Zeitraum | 5 Minuten | 1 Stunde | 1 Tag |
|---|---|---|---|
| 1 Stunde | 1–8 Serien | | |
| 6 Stunden | 1–6 Serien | 7–8 Serien | |
| 24 Stunden | 1–2 Serien | 3–8 Serien | |
| 48 Stunden | | 1–8 Serien | |
| 7 Tage | | 1–3 Serien | 4–8 Serien |
| 30 Tage | | | 1–8 Serien |

Eine feinere Auflösung lässt sich erzwingen. Meldet das Widget dann einen Fehler zum *operation limit*, ist sie für diese Kombination zu fein. Home Assistant bewahrt 5-Minuten-Werte außerdem nur etwa 10 Tage auf.

### Darstellung, Legende und Achsen

| Einstellung | Bedeutung | Standard |
|---|---|---|
| Diagrammtyp | *Linie*: Füllung nur für Serien mit `"fill": true`. *Fläche*: alle Serien gefüllt, außer `"fill": false`. *Stufe*: Treppenlinie. *Balken*. | Linie |
| Linienstärke | Gilt für alle Serien | `2` |
| Höhe des Diagramms (px) | Feste Höhe. Die Breite passt sich dem Widget an. | `220` |
| Raster anzeigen | Rasterlinien ein oder aus | an |
| Tooltip beim Hovern | Werte der Periode unter dem Mauszeiger | an |
| Legende anzeigen | Zeile unter dem Diagramm | an |
| Statistik in der Legende | Auswahl aus Aktuell, Minimum, Durchschnitt, Maximum | Aktuell, Durchschnitt, Maximum |
| Einheit der Y-Achse | Text hinter den Achsenwerten, kurz halten oder leer lassen | leer |
| Y-Minimum, Y-Maximum | Feste Grenzen, leer = automatisch | leer |
| Einheit, Minimum, Maximum der zweiten Y-Achse | Wie oben, für Serien mit `"axis": "secondary"` | leer |

Minimum und Maximum in der Legende sind die echten Extremwerte im Zeitraum, nicht die der gemittelten Perioden.

### Serien

Homarr kennt kein Listen-Feld. Die Serien stehen deshalb als JSON-Liste in einem Textfeld. Jeder Eintrag ist ein Objekt:

| Schlüssel | Bedeutung | Standard |
|---|---|---|
| `entity_id` | **Pflicht.** Entität in Home Assistant, z. B. `sensor.aussen_temperatur` | |
| `name` | Anzeigename | `friendly_name` aus Home Assistant |
| `color` | Farbe aus dem Mantine-Theme (`blue.6`, `green.3`, `orange.6` …) oder CSS-Farbe (`#ff8800`) | je Position eine andere Farbe |
| `factor` | Umrechnung: angezeigter Wert = Wert × `factor` + `offset` | `1` |
| `offset` | siehe `factor` | `0` |
| `unit` | Einheit in Legende und Tooltip | Einheit der Achse, sonst die aus Home Assistant |
| `decimals` | Nachkommastellen, 0 bis 6 | `2` |
| `dash` | Linienstil: `solid`, `dashed`, `dotted` | `solid` |
| `fill` | `true` oder `false`, siehe Diagrammtyp | |
| `axis` | `primary` oder `secondary` (zweite Y-Achse rechts) | `primary` |
| `reference` | `true`: nur den aktuellen Wert als waagerechte gestrichelte Linie zeichnen | `false` |

Gängige Faktoren: kB/s → Mbit/s = `0.008`, kbit/s → Mbit/s = `0.001`, W → kW = `0.001`.

Ist die Liste fehlerhaft, zeigt das Widget statt des Diagramms eine Meldung mit dem betroffenen Eintrag.

## Beispiele

### Beispiel 1 – FRITZ!Box-Durchsatz

Voreinstellung des Widgets. Zeitraum *24 Stunden*, Aggregation *Mittelwert*, Legende mit *Aktuell*, *Durchschnitt* und *Maximum*.

```json
[
  { "entity_id": "sensor.fritz_box_7590_download_throughput", "name": "Download", "color": "blue.6", "factor": 0.008, "unit": "Mbit/s", "decimals": 2, "fill": true },
  { "entity_id": "sensor.fritz_box_7590_upload_throughput", "name": "Upload", "color": "green.6", "factor": 0.008, "unit": "Mbit/s", "decimals": 2, "fill": true },
  { "entity_id": "sensor.fritz_box_7590_max_connection_download_throughput", "name": "Max. Download", "color": "blue.3", "factor": 0.001, "unit": "Mbit/s", "decimals": 1, "reference": true },
  { "entity_id": "sensor.fritz_box_7590_max_connection_upload_throughput", "name": "Max. Upload", "color": "green.3", "factor": 0.001, "unit": "Mbit/s", "decimals": 1, "reference": true }
]
```

Die Referenzlinien dehnen die Y-Achse bis zur Leitungskapazität. Liegt der Durchsatz weit darunter, wird die Kurve flach. Dann die beiden Referenz-Einträge entfernen oder ein *Y-Maximum* setzen.

### Beispiel 2 – zwei Temperaturen auf einer Achse

Titel `Temperatur`, Zeitraum *7 Tage*, Einheit der Y-Achse `°C`, Legende mit *Aktuell*, *Minimum* und *Maximum*.

```json
[
  { "entity_id": "sensor.wohnzimmer_temperatur", "name": "Wohnzimmer", "color": "orange.6", "decimals": 1 },
  { "entity_id": "sensor.aussen_temperatur", "name": "Außen", "color": "cyan.6", "decimals": 1, "dash": "dashed" }
]
```

### Beispiel 3 – Leistung und Batterie-Ladestand auf zwei Achsen

Titel `PV und Speicher`, Zeitraum *24 Stunden*, Einheit der Y-Achse `W`, zweite Y-Achse: Einheit `%`, Minimum `0`, Maximum `100`.

```json
[
  { "entity_id": "sensor.pv_leistung", "name": "PV-Leistung", "color": "yellow.6", "unit": "W", "decimals": 0, "fill": true },
  { "entity_id": "sensor.batterie_ladestand", "name": "Batterie", "color": "grape.6", "unit": "%", "decimals": 0, "axis": "secondary", "dash": "dashed" }
]
```

Die Namen der Entitäten in Beispiel 2 und 3 sind Platzhalter.

## Grenzen

Diese Punkte gibt die Runtime von Homarr vor:

- **Höchstens 8 Serien.** Abfragen dürfen keine Listen als Parameter enthalten, deshalb hat die Abfrage 8 feste Plätze.
- **Auflösung nur 5 Minuten, 1 Stunde oder 1 Tag.** Das sind die Perioden der Statistik von Home Assistant. Rohdaten aus `/api/history` lassen sich wegen des Rechenlimits nicht im Widget zusammenfassen.
- **Feste Höhe.** Ein Widget kann seine eigene Größe nicht abfragen.
- **Linienstärke nur für alle Serien gemeinsam.**
- **Tooltip ohne eigene Formatierung:** Zahlen erscheinen mit Punkt statt Komma.
- **Zeitachse:** Uhrzeit bis 24 Stunden, Wochentag und Uhrzeit bei Stundenwerten über längere Zeiträume, Wochentag und Datum bei Tageswerten.
- Bei Stundenwerten endet das Diagramm mit der letzten vollen Stunde, bei 5-Minuten-Werten mit den letzten vollen 5 Minuten.

## Verwendete API-Endpunkte

Alle Abfragen gehen an die REST-API von Home Assistant.

| Request | Methode | Pfad | Zweck |
|---|---|---|---|
| `stats` | POST | `/api/services/recorder/get_statistics?return_response` | Perioden für das Diagramm |
| `summary` | POST | `/api/services/recorder/get_statistics?return_response` | Gröbere Perioden für Minimum, Durchschnitt und Maximum der Legende. Nur bei mehr als 24 Perioden. |
| `current` | GET | `/api/history/period/<start>?filter_entity_id=<ids>&minimal_response` | Aktueller Wert, Name und Einheit aller Serien in einer Abfrage |

`recorder.get_statistics` liest nur und ändert nichts in Home Assistant.

## Fehlerbehebung

- **„Home Assistant lehnt das Token ab (401)“:** Das Token unter *Quellen konfigurieren* ist falsch oder wurde gelöscht.
- **„Home Assistant ist nicht erreichbar“:** Adresse unter *Quellen konfigurieren* prüfen. Homarr muss Home Assistant im Netzwerk erreichen.
- **„Entität nicht gefunden“:** Die `entity_id` gibt es in Home Assistant nicht. Schreibweise unter *Einstellungen → Entitäten* prüfen.
- **„Keine Statistik für …“:** Die Entität hat keine `state_class`, oder die Aggregation passt nicht (z. B. *Mittelwert* für einen Zähler). Als Referenzlinie lässt sie sich trotzdem anzeigen.
- **Fehler zum *operation limit* oder *collection limit*:** Die Auflösung ist für Zeitraum und Serienzahl zu fein. Auf *Automatisch* stellen oder eine gröbere wählen.
- **Legende zeigt „–“:** Für den Zeitraum liegt noch keine Statistik vor, z. B. bei einer neuen Entität in der ersten Stunde.

---

[← Zurück zur Übersicht](../../README.md) · Lizenz: siehe [LICENSE](../../LICENSE)
