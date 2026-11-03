# Kopia Info

Zeigt den Status eines [Kopia](https://kopia.io)-Backup-Servers: Verbindung zum Repository und pro Backup-Quelle den letzten und nächsten Snapshot, die Größe, den Zeitplan und Fehler.

**Datei:** `kopia-info.json` · **Version:** 1.0.0 · [Changelog](CHANGELOG.md)

## Funktionen

- Kopfzeile mit `benutzer@host` des Kopia-Servers und Button zum Neuladen
- Badge **CONNECTED/OFFLINE** für das Repository und Anzahl der Backup-Quellen
- Eine Karte pro Quelle mit folgendem Inhalt:
  - Pfad und Ergebnis des letzten Snapshots: **OK**, **n Fehler** oder **KEIN SNAPSHOT**
  - `benutzer@host` der Quelle und aktueller Status (`IDLE`, `UPLOADING`, `FAILED`, …)
  - Größe des letzten Snapshots in MiB, GiB oder TiB
  - Zeitplan: Uhrzeiten, Intervall („alle 6 h“), Cron-Ausdruck oder „manuell“
  - Zeitpunkt des letzten und des nächsten Snapshots in Ortszeit (Europe/Berlin)

## Voraussetzungen

- Homarr 2.x
- Kopia läuft im Server-Modus (`kopia server start …`) und ist von Homarr aus erreichbar.

## Einrichtung

1. In Homarr *Benutzerdefinierte Widgets → Import* wählen und die JSON-Datei unverändert hochladen.
2. Über *⋯ → Quellen konfigurieren* die Adresse des Kopia-Servers eintragen, z. B. `https://192.168.1.10:51515`.
3. Dort auch die **Authentifizierung** wählen:
   - **Mit Passwort** (Voreinstellung *Basic*): den Benutzernamen und das Passwort des Kopia-Servers eintragen,
     also die Werte aus `--server-username` (Standard `kopia`) und `--server-password`.
   - **Ohne Passwort** (Kopia mit `--without-password`): Authentifizierung auf *None* stellen.
     Lässt Homarr das nicht zu, *Basic* behalten und beliebige Zugangsdaten eintragen.
     Kopia wertet sie ohne Passwortschutz nicht aus.
4. Das Widget zu einem Board hinzufügen.

**Hinweise zum Kopia-Server**

- Ohne TLS (`http://…`) muss Kopia mit `--insecure` laufen. Die Zugangsdaten gehen dann unverschlüsselt durchs Netz.
- Antwortet Kopia mit *403* bzw. einem CSRF-Fehler, den Server zusätzlich mit `--disable-csrf-token-checks` starten.
  Die Web-Oberfläche von Kopia verlangt sonst ein Token, das Homarr nicht mitschickt.

## Anpassen

| Was | Wo in der JSON-Datei | Standard |
|---|---|---|
| Zeitzone der Zeitangaben | `template`: `Date.toLocaleString(…, "de-DE", "Europe/Berlin")` (2×) | `Europe/Berlin` |
| Grenze TiB/GiB/MiB | `template`: `1099511627776` (1 TiB), `1073741824` (1 GiB) | – |
| Farbe des Status-Badges | `template`: `item.status === "UPLOADING" ? "blue" : …` | blau/rot/grau |

## Verwendete API-Endpunkte

| Request | Methode | Pfad |
|---|---|---|
| `sources` | GET | `/api/v1/sources` |
| `repo` | GET | `/api/v1/repo/status` |

---

[← Zurück zur Übersicht](../../README.md) · Lizenz: siehe [LICENSE](../../LICENSE)
