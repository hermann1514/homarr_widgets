# Changelog – Nextcloud News

Alle nennenswerten Änderungen an diesem Widget werden in dieser Datei dokumentiert.

Das Format orientiert sich an [Keep a Changelog](https://keepachangelog.com/de/1.1.0/),
die Versionierung an [Semantic Versioning](https://semver.org/lang/de/).

## [1.2.1] – 2026-11-05

### Behoben
- Button **„Angezeigte gelesen“** war ausgegraut, weil Homarr keine Array-Parameter (`itemIds`) akzeptiert. `markShownRead` nutzt jetzt `POST /api/v1-3/items/read` mit einem einzelnen Wert `newestItemId` im Body. Der vorherige Versuch mit `PUT` auf `v1-2` endete mit Fehler 405.

### Geändert
- Die Liste zeigt jetzt die **ältesten** ungelesenen Artikel zuerst (`oldestFirst: true`). Nur so trifft `newestItemId` genau die angezeigten Artikel.

## [1.2.0] – 2026-11-05

### Geändert
- Der Button im Kopfbereich heißt jetzt **„Angezeigte gelesen“** und markiert nur noch die im Widget angezeigten Artikel als gelesen (Request `markShownRead`, `POST /items/read/multiple`). Bisher markierte **„Alle gelesen“** alle ungelesenen Artikel in Nextcloud.

## [1.1.0] – 2026-11-03

### Hinzugefügt
- Button **„Gelesen“** an jedem Eintrag: markiert den Artikel in Nextcloud als gelesen (Request `markRead`). Die Liste lädt danach neu, und der nächste ungelesene Artikel rückt nach.
- Button **„Alle gelesen“** im Kopfbereich: markiert nach einer Rückfrage alle ungelesenen Artikel in Nextcloud als gelesen (Request `markAllRead`).
- Freundlicher Hinweis „Keine ungelesenen Einträge 🎉“, wenn alles gelesen ist.
- `rel="noopener noreferrer"` an den Artikel-Links, damit geöffnete Seiten nicht auf den Homarr-Tab zugreifen können.
- Eigene [README](README.md) mit Funktionsübersicht, Voraussetzungen, Einrichtung und Anpassungsmöglichkeiten.

### Geändert
- Es werden jetzt **20** statt 10 Einträge angezeigt.
- Artikeltitel sind **blau** statt in der (roten) Primärfarbe des Homarr-Themes. Unterstrichen werden sie nur beim Darüberfahren mit der Maus.
- Der Untertitel lautet „Ungelesene Einträge“ statt „Letzte 10 Einträge“, denn das Widget lädt nur ungelesene Artikel.
- Kann die Feed-Liste nicht geladen werden, erscheinen die Artikel trotzdem (mit „Feed“ als Ersatzname) statt einer Fehlermeldung.
- Die Basis-URL in der JSON-Datei ist jetzt der Platzhalter `https://nextcloud.example.com`. Die eigene Nextcloud-Adresse wird in Homarr unter *Quellen konfigurieren* eingetragen.
- Datei umbenannt von `Nextcloud News.json` nach `widgets/nextcloud-news/nextcloud-news.json`.

### Entfernt
- Badge „NEU“: Es werden nur ungelesene Artikel geladen, das Badge stand also ohnehin an jedem Eintrag.
- Doppelte Begrenzung der Eintragsanzahl im Template. Die Anzahl steht nur noch in `batchSize`.

### Behoben
- Importfehler `UNSUPPORTED_BLOCK_STATEMENT: 'NewExpression' is not allowed in safe expressions`: Die Datumsanzeige mit `new Date(...)` wurde entfernt, weil Homarrs Template-Runtime keine `new`-Ausdrücke erlaubt.

## [1.0.0] – 2026-11-03

### Hinzugefügt
- Erste Version: zeigt die 10 neuesten ungelesenen Artikel aus der Nextcloud-News-App mit Feed-Name, Autor und ★-Markierung.
