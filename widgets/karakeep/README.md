# Karakeep

Zeigt die neuesten Lesezeichen aus [Karakeep](https://karakeep.app) (früher Hoarder).

**Datei:** `karakeep.json` · **Version:** 1.0.0 · [Changelog](CHANGELOG.md)

## Funktionen

- Die 5 neuesten nicht archivierten Lesezeichen als Karte mit Vorschaubild oder Favicon, Titel, Beschreibung und Datum (Europe/Berlin)
- Ein Klick auf einen Link öffnet die Seite in einem neuen Tab.
- Notizen zeigen ihren Text als Vorschau, Dateien ihren Dateinamen. Beide sind nicht verlinkt.
- Button zum Neuladen

## Voraussetzungen

- Homarr 2.x
- Karakeep ist von Homarr aus erreichbar.

## Einrichtung

1. In Karakeep unter *Einstellungen → API-Schlüssel* einen neuen Schlüssel erstellen.
2. In Homarr *Benutzerdefinierte Widgets → Import* wählen und die JSON-Datei unverändert hochladen.
3. Über *⋯ → Quellen konfigurieren* folgende Werte eintragen:

   | Feld | Wert |
   |---|---|
   | Adresse | Adresse von Karakeep ohne `/` am Ende, z. B. `https://karakeep.example.com` |
   | Authentifizierung | *Bearer* (Voreinstellung) |
   | Token | der API-Schlüssel aus Schritt 1 |

4. Das Widget zu einem Board hinzufügen.

## Anpassen

| Was | Wo in der JSON-Datei | Standard |
|---|---|---|
| Anzahl der Lesezeichen | `requests.items.query.limit` | `5` |
| Archivierte mit anzeigen | `requests.items.query.archived` entfernen | nur nicht archivierte |
| Zeitzone des Datums | `template`: `Date.toLocaleString(…, "de-DE", "Europe/Berlin")` | `Europe/Berlin` |

## Verwendete API-Endpunkte

| Request | Methode | Pfad |
|---|---|---|
| `items` | GET | `/api/v1/bookmarks` |

---

[← Zurück zur Übersicht](../../README.md) · Lizenz: siehe [LICENSE](../../LICENSE)
