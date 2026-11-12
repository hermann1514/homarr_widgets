# Homarr Widgets

Eigene Widgets für das Dashboard [Homarr](https://homarr.dev), im Format `homarr-custom-widget-v2` (Homarr 2.x).

## Enthaltene Widgets

| Widget | Was es zeigt | Version |
|---|---|---|
| [Nextcloud News](widgets/nextcloud-news/) | Ungelesene Artikel aus Nextcloud News, direkt als gelesen markierbar | 1.2.1 |
| [Kopia Info](widgets/kopia-info/) | Status des Kopia-Backup-Servers: Repository, Quellen, letzter und nächster Snapshot | 1.0.0 |
| [Karakeep](widgets/karakeep/) | Die neuesten Lesezeichen aus Karakeep | 1.0.0 |
| [Claude Usage](widgets/claude-usage/) | Verbrauch des Claude-Abos: Session-, Wochen- und Modell-Limit, über Home Assistant | 1.0.0 |
| [HA Verlaufsdiagramm](widgets/ha-history-chart/) | Verlauf beliebiger Home-Assistant-Entitäten als Diagramm | 1.0.0 |

### Nextcloud News

- Die 20 neuesten ungelesenen Artikel mit Feed-Name, Titel und Autor. Favoriten haben ein ★.
- Button **Gelesen** an jedem Artikel und **Angezeigte gelesen** für alle im Widget angezeigten Artikel.
- Ein Klick auf den Titel öffnet den Artikel in einem neuen Tab.

Mehr Details und Anpassungen: [README des Widgets](widgets/nextcloud-news/README.md)

### Kopia Info

- Badge **CONNECTED/OFFLINE** für das Repository und Anzahl der Backup-Quellen.
- Eine Karte pro Quelle: Ergebnis des letzten Snapshots, Status, Größe und Zeitplan.
- Zeitpunkt des letzten und des nächsten Snapshots in Ortszeit.

Mehr Details und Anpassungen: [README des Widgets](widgets/kopia-info/README.md)

### Karakeep

- Die 5 neuesten nicht archivierten Lesezeichen mit Vorschaubild, Titel, Beschreibung und Datum.
- Ein Klick öffnet den Link in einem neuen Tab. Notizen zeigen ihren Text als Vorschau.

Mehr Details und Anpassungen: [README des Widgets](widgets/karakeep/README.md)

### Claude Usage

- Balken für das Session-Limit (5 h), das Wochenlimit aller Modelle und das Wochenlimit eines Modells, mit Countdown bis zum Reset.
- Tempo-Vergleich: Verbrauch gegenüber der schon vergangenen Zeit des Zeitfensters.
- Extra Usage, wenn aktiviert, und Hinweis bei API-Fehlern.

Mehr Details und Anpassungen: [README des Widgets](widgets/claude-usage/README.md)

### HA Verlaufsdiagramm

- Bis zu 8 Serien als Linie, Fläche, Stufe oder Balken, über 1 Stunde bis 30 Tage.
- Pro Serie Umrechnung mit Faktor und Offset, eigene Einheit, zweite Y-Achse und Referenzlinien.
- Legende mit aktuellem Wert, Minimum, Durchschnitt und Maximum.

Mehr Details, alle Einstellungen und Beispiele: [README des Widgets](widgets/ha-history-chart/README.md)

## Widget importieren

1. In Homarr *Benutzerdefinierte Widgets → Import* wählen und die `.json`-Datei aus dem Widget-Ordner unverändert hochladen.
2. Beim Widget über *⋯ → Quellen konfigurieren* die **Adresse des Dienstes** und die **Zugangsdaten** eintragen.
3. Das Widget zu einem Board hinzufügen.

Homarr speichert die Zugangsdaten verschlüsselt. Sie sind nicht Teil der JSON-Datei. In den JSON-Dateien stehen nur Platzhalter-Adressen wie `https://kopia.example.com`. Aus Sicherheitsgründen lässt Homarr die Adresse nicht in den Widget-Einstellungen festlegen, sondern nur unter *Quellen konfigurieren*.

## Konfiguration der Dienste

Damit ein Widget Daten bekommt, muss der Dienst dahinter von Homarr aus erreichbar sein und die Abfrage zulassen.

### Kopia

Kopia muss im **Server-Modus** laufen. Das Widget fragt die API ab, die auch die Web-Oberfläche von Kopia nutzt. Es meldet sich mit demselben Benutzer und Passwort an wie die Web-Oberfläche.

**Server mit Benutzer und Passwort starten**

```sh
kopia server start \
  --address=0.0.0.0:51515 \
  --tls-generate-cert \
  --server-username=kopia \
  --server-password=GEHEIM \
  --disable-csrf-token-checks
```

- `--server-username` und `--server-password` legen die Zugangsdaten fest. Ohne `--server-username` heißt der Benutzer `kopia`.
- `--tls-generate-cert` erzeugt beim ersten Start ein Zertifikat, damit die Verbindung über `https://` läuft. Bei späteren Starts das Zertifikat mit `--tls-cert-file` und `--tls-key-file` wieder angeben.
- `--disable-csrf-token-checks` ist nötig, weil die Web-Oberfläche sonst ein Token verlangt, das Homarr nicht mitschickt. Ohne das Flag antwortet Kopia mit *403*.

**Beispiel mit Docker Compose**

```yaml
services:
  kopia:
    image: kopia/kopia:latest
    command:
      - server
      - start
      - --address=0.0.0.0:51515
      - --tls-cert-file=/app/config/kopia.cert
      - --tls-key-file=/app/config/kopia.key
      - --server-username=kopia
      - --disable-csrf-token-checks
    environment:
      KOPIA_PASSWORD: "Passwort des Repositorys"
      KOPIA_SERVER_PASSWORD: "GEHEIM"
    ports:
      - "51515:51515"
    volumes:
      - ./config:/app/config
      - ./cache:/app/cache
      - ./repository:/repository
```

`KOPIA_PASSWORD` ist das Passwort des Repositorys. `KOPIA_SERVER_PASSWORD` ist das Passwort für die Anmeldung an der Web-Oberfläche, und nur das braucht das Widget.

**In Homarr unter *Quellen konfigurieren***

| Feld | Wert |
|---|---|
| Adresse | `https://<host>:51515` |
| Authentifizierung | *Basic* (Voreinstellung) |
| Benutzername | Wert von `--server-username`, Standard `kopia` |
| Passwort | Wert von `--server-password` bzw. `KOPIA_SERVER_PASSWORD` |

**Gut zu wissen**

- **Ohne Passwort:** Läuft Kopia mit `--without-password`, die Authentifizierung auf *None* stellen. Lässt Homarr das nicht zu, *Basic* behalten und beliebige Werte eintragen. Kopia prüft sie dann nicht.
- **Ohne TLS:** Für `http://` muss Kopia mit `--insecure` laufen. Benutzer und Passwort gehen dann unverschlüsselt durchs Netz. Das ist nur im eigenen Netz vertretbar.
- **Nicht verwechseln:** Benutzer, die mit `kopia server user add` angelegt werden, sind für Kopia-Clients gedacht, die ihre Backups auf diesen Server schreiben. Für das Widget gelten sie nicht. Es braucht die Zugangsdaten der Web-Oberfläche.

### Nextcloud News

**In Nextcloud vorbereiten**

1. Die App **News** aus dem Nextcloud-App-Store installieren und Feeds hinzufügen.
2. Unter *Verwaltung → Grundeinstellungen → Hintergrund-Aufgaben* **Cron** wählen und einen Cronjob einrichten. Sonst aktualisiert Nextcloud die Feeds nur selten.
3. Ein **App-Passwort** erstellen: *Einstellungen → Sicherheit → Neues App-Passwort erstellen*. Das App-Passwort lässt sich jederzeit widerrufen, ohne dass sich das normale Passwort ändert.

**In Homarr unter *Quellen konfigurieren***

| Feld | Wert |
|---|---|
| Adresse | Adresse der Nextcloud ohne `/` am Ende, z. B. `https://cloud.example.com` |
| Authentifizierung | *Basic* (Voreinstellung) |
| Benutzername | Nextcloud-Benutzername |
| Passwort | das App-Passwort |

**Gut zu wissen**

- Meldet Homarr beim Markieren einen Fehler *405*, in der JSON-Datei bei `markRead` und `markShownRead` die `method` von `POST` auf `PUT` ändern.
- Homarr erlaubt höchstens 10 Aktionen pro Minute und Nutzer. Das betrifft die Buttons zum Markieren.

### Karakeep

**In Karakeep vorbereiten:** Unter *Einstellungen → API-Schlüssel* einen neuen Schlüssel erstellen.

**In Homarr unter *Quellen konfigurieren***

| Feld | Wert |
|---|---|
| Adresse | Adresse von Karakeep ohne `/` am Ende, z. B. `https://karakeep.example.com` |
| Authentifizierung | *Bearer* (Voreinstellung) |
| Token | der API-Schlüssel |

### Claude Usage (über Home Assistant)

Homarr kann die OAuth-Tokens von Anthropic nicht selbst erneuern. Deshalb holt die Integration [hass-claude-usage](https://github.com/trickv/hass-claude-usage) in Home Assistant die Daten, und das Widget liest deren Sensoren.

**In Home Assistant vorbereiten**

1. **hass-claude-usage** über HACS installieren, die Integration *Claude Usage* hinzufügen und per OAuth bei Claude anmelden. Das Abfrageintervall auf 300 Sekunden lassen, sonst sperrt Anthropic die Usage-API für etwa 24 Stunden.
2. Unter *Profil → Sicherheit → Langlebige Zugangs-Token* ein Token erstellen.
3. Unter *Einstellungen → Entitäten* nach `claude` suchen und das Präfix der Sensoren ablesen, z. B. `claude_usage_my_account` aus `sensor.claude_usage_my_account_session_limit_usage`. Es kommt in die Widget-Option *Präfix der Sensoren*.

**In Homarr unter *Quellen konfigurieren***

| Feld | Wert |
|---|---|
| Adresse | Adresse von Home Assistant ohne `/` am Ende, z. B. `https://homeassistant.example.com` |
| Authentifizierung | *Bearer* (Voreinstellung) |
| Token | das langlebige Zugangs-Token |

### HA Verlaufsdiagramm

Das Widget liest die Langzeitstatistik von Home Assistant. Gezeichnete Entitäten brauchen deshalb eine `state_class`.

**In Home Assistant vorbereiten:** Unter *Profil → Sicherheit → Langlebige Zugangs-Token* ein Token erstellen.

**In Homarr unter *Quellen konfigurieren***

| Feld | Wert |
|---|---|
| Adresse | Adresse von Home Assistant ohne `/` am Ende, z. B. `https://homeassistant.example.com` |
| Authentifizierung | *Bearer* (Voreinstellung) |
| Token | das langlebige Zugangs-Token |

## Lizenz

Siehe [LICENSE](LICENSE).
