# Homarr Widgets

Eigene Widgets für das Dashboard [Homarr](https://homarr.dev), im Format `homarr-custom-widget-v2` (Homarr 2.x).

## Widgets

| Widget | Beschreibung | Version |
|---|---|---|
| [Nextcloud News](widgets/nextcloud-news/) | Ungelesene Artikel aus Nextcloud News, direkt als gelesen markierbar | 1.1.0 |
| [Kopia Info](widgets/kopia-info/) | Status des Kopia-Backup-Servers: Repository, Quellen, letzter und nächster Snapshot | 1.0.0 |

Jedes Widget hat einen eigenen Ordner mit README (Funktionen, Einrichtung, Anpassen) und Changelog.

## Widget importieren

1. In Homarr *Benutzerdefinierte Widgets → Import* wählen und die `.json`-Datei aus dem Widget-Ordner unverändert hochladen.
2. Beim Widget über *⋯ → Quellen konfigurieren* die **Adresse deines Dienstes** und die **Zugangsdaten** eintragen. Die Zugangsdaten speichert Homarr verschlüsselt, sie sind nicht Teil der JSON-Datei.
3. Das Widget zu einem Board hinzufügen.

In den JSON-Dateien stehen nur Platzhalter-Adressen wie `https://service.example.com`. Die Adresse in den Widget-Einstellungen festzulegen erlaubt Homarr aus Sicherheitsgründen nicht.

Details stehen jeweils in der README des Widgets.

## Repo-Aufbau

```text
homarr_widgets/
├── README.md            ← diese Übersicht
├── CHANGELOG.md         ← Änderungen am Repo selbst
├── _template/           ← Vorlage für neue Widgets
└── widgets/
    └── <widget-name>/
        ├── <widget-name>.json
        ├── README.md
        └── CHANGELOG.md
```

## Neues Widget anlegen

1. Den Ordner [`_template/`](_template/) nach `widgets/<widget-name>/` kopieren. Den Namen in Kleinbuchstaben mit Bindestrichen schreiben, z. B. `kopia-backups`.
2. `widget.json` in `<widget-name>.json` umbenennen und in Homarr aufbauen. Oder ein bestehendes Widget in Homarr über *⋯ → Exportieren* sichern.
3. **Vor dem Commit die `baseUrl` durch einen Platzhalter ersetzen**, z. B. `https://service.example.com`. Das Repo ist öffentlich, und die echte Adresse wird ohnehin in Homarr unter *Quellen konfigurieren* eingetragen.
4. README und CHANGELOG im Widget-Ordner ausfüllen und das Widget oben in die Tabelle eintragen.
5. Optional eine Version taggen: `git tag <widget-name>-v1.0.0`.

## Lizenz

Siehe [LICENSE](LICENSE).
