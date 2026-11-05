# Nextcloud News

<img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/svg/nextcloud.svg" alt="Nextcloud" width="48" align="right" />

Zeigt die **20 neuesten ungelesenen Artikel** aus der [Nextcloud-News-App](https://apps.nextcloud.com/apps/news) auf deinem Homarr-Board.

**Datei:** [`nextcloud-news.json`](nextcloud-news.json) · **Version:** 1.2.0 · [Changelog](CHANGELOG.md)

## Funktionen

- **Artikelliste:** Jeder Eintrag zeigt Feed-Name, Titel und Autor. Favoriten haben ein ★.
- **Artikel öffnen:** Ein Klick auf den Titel öffnet den Artikel in einem neuen Tab.
- **„Gelesen“** an jedem Eintrag: markiert den Artikel in Nextcloud als gelesen. Danach rückt automatisch der nächste ungelesene Artikel nach.
- **„Angezeigte gelesen“** im Kopfbereich: markiert nach einer Rückfrage **nur die angezeigten** Artikel in Nextcloud als gelesen. Weitere ungelesene Artikel rücken danach nach.
- **Aktualisieren** über den Button im Kopfbereich.
- **Robuste Anzeige:** Ladeanimation, Fehlermeldung bei Verbindungsproblemen und ein Hinweis, wenn alles gelesen ist.

```text
┌─────────────────────────────────────────────┐
│ ☁ Nextcloud News  [✓ Angezeigte gelesen] ⟳ │
│   Ungelesene Einträge                       │
├─────────────────────────────────────────────┤
│ heise online                   ★ [✓ Gelesen]│
│ Titel des Artikels …                        │
│ Autor                                       │
├─────────────────────────────────────────────┤
│ Golem.de                         [✓ Gelesen]│
│ Noch ein Artikel …                          │
└─────────────────────────────────────────────┘
```

## Voraussetzungen

- **Homarr 2.x** mit Unterstützung für Custom Widgets
- **Nextcloud** mit installierter **News-App**
- Ein Nextcloud-**App-Passwort**: In Nextcloud unter *Einstellungen → Sicherheit → Neues App-Passwort erstellen*. Verwende es statt deines normalen Passworts, dann kannst du es jederzeit widerrufen.

## Einrichtung

1. **Importieren:** In Homarr *Benutzerdefinierte Widgets → Import* wählen und [`nextcloud-news.json`](nextcloud-news.json) unverändert hochladen.
2. **Quelle einrichten:** Beim Widget *⋯ → Quellen konfigurieren* öffnen und eintragen:
   - **Adresse** deiner Nextcloud, ohne `/` am Ende, z. B. `https://cloud.example.com`
   - **Benutzername** und **App-Passwort**
3. **Aufs Board legen:** Das Widget wie jedes andere zu einem Board hinzufügen.

> **Warum keine Einstellung pro Kachel?** Aus Sicherheitsgründen dürfen Widget-Einstellungen bei Homarr eine Anfrage nicht an eine andere Adresse schicken. Die Adresse legst du deshalb einmal über *Quellen konfigurieren* fest, und sie gilt für alle Kacheln dieses Widgets. In der JSON-Datei steht nur der Platzhalter `https://nextcloud.example.com`.

## Anpassen

| Was | Wo in der JSON-Datei | Standard |
|---|---|---|
| Anzahl der Einträge | `requests.items.query.batchSize` | `20` |
| Auch gelesene Artikel anzeigen | `requests.items.query.getRead` auf `true` (dann den Untertitel im `template` anpassen) | `false` |
| Farbe der Titel | `c="blue.4"` am `Anchor` im `template` (beliebige [Mantine-Farbe](https://mantine.dev/theming/colors/)) | `blue.4` |
| Wer markieren darf | `permission` bei `markRead` / `markShownRead`. `view` erlaubt es allen Board-Nutzern; ohne den Eintrag sind Bearbeitungsrechte nötig | `view` |

## Gut zu wissen

- **Kein automatisches Markieren beim Scrollen:** Homarrs Widget-Runtime erlaubt aus Sicherheitsgründen keine Scroll- oder Sichtbarkeits-Events. Markieren geht deshalb nur über die Buttons.
- **Kein Veröffentlichungsdatum:** Die Template-Runtime erlaubt keine `new Date(...)`-Ausdrücke.
- **Limit für Aktionen:** Homarr erlaubt höchstens 10 Aktionen pro Minute und Nutzer.
- **POST oder PUT:** Die Buttons nutzen `POST` auf die Nextcloud-News-API `v1-3`. Meldet Homarr beim Markieren einen Fehler 405, stelle `method` bei `markRead` und `markShownRead` auf `PUT` um.

## Verwendete API-Endpunkte

| Request | Methode | Pfad |
|---|---|---|
| `items` | GET | `/index.php/apps/news/api/v1-3/items` |
| `feeds` | GET | `/index.php/apps/news/api/v1-3/feeds` |
| `markRead` | POST | `/index.php/apps/news/api/v1-3/items/{id}/read` |
| `markShownRead` | POST | `/index.php/apps/news/api/v1-3/items/read/multiple` |

---

[← Zurück zur Übersicht](../../README.md) · Lizenz: siehe [LICENSE](../../LICENSE)
