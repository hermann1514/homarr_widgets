# Claude Usage

Zeigt den Verbrauch des Claude-Abos (Pro/Max): Session-Limit (5 h), Wochenlimit und Limit pro Modell. Die Daten kommen aus Home Assistant, von der Integration [hass-claude-usage](https://github.com/trickv/hass-claude-usage). Das Widget ist angelehnt an die HA-Karte [claude-usage-card](https://github.com/darki79/claude-usage-card).

**Datei:** `claude-usage.json` · **Version:** 1.0.0 · [Changelog](CHANGELOG.md)

## Funktionen

- Je ein Balken für das **Session-Limit (5 h)**, das **Wochenlimit aller Modelle** und das **Wochenlimit eines Modells** (z. B. Sonnet).
- Farbe nach Auslastung: grün, ab 75 % orange, ab 90 % rot.
- Dünner grauer Balken darunter: Anteil des Zeitfensters, der schon vergangen ist. Das Badge daneben vergleicht damit den Verbrauch, z. B. *+6 % über Tempo*. Liegt der Verbrauch über dem Zeitbalken, ist das Limit vor dem Reset aufgebraucht, wenn es so weitergeht.
- Countdown bis zum Reset (*Reset in 2 h 14 min*) und Zeitpunkt des Resets (Europe/Berlin)
- Balken **Extra Usage**, nur wenn Extra Usage im Konto aktiviert ist
- Hinweis, wenn die Integration einen API-Fehler meldet
- Button zum Neuladen

Forecast und Verlaufsgrafik der HA-Karte fehlen, weil Homarr keinen Sensor-Verlauf kennt.

## Voraussetzungen

- Homarr 2.x
- Home Assistant mit der Integration **hass-claude-usage**. Sie meldet sich per OAuth bei Anthropic an und erneuert das Token selbst.
- Home Assistant ist von Homarr aus erreichbar.

## Einrichtung

1. **Integration in Home Assistant installieren**
   1. In HACS *Benutzerdefinierte Repositories* das Repo `https://github.com/trickv/hass-claude-usage` als *Integration* hinzufügen und **Claude Usage** installieren. Danach Home Assistant neu starten.
   2. Unter *Einstellungen → Geräte & Dienste → Integration hinzufügen* **Claude Usage** wählen. Den angezeigten Link öffnen, bei Claude anmelden und den Code einfügen.
   3. Das Abfrageintervall auf 300 Sekunden lassen. Anthropic sperrt die Usage-API bei zu häufigen Abfragen für etwa 24 Stunden.
2. **Präfix ablesen:** Unter *Einstellungen → Entitäten* nach `claude` suchen. Die Entität zum Session-Limit heißt z. B. `sensor.claude_usage_my_account_session_limit_usage`. Das Präfix ist der Teil zwischen `sensor.` und `_session_limit_usage`, hier `claude_usage_my_account`.
3. **Token erstellen:** In Home Assistant unten links auf das eigene Profil klicken, dann *Sicherheit → Langlebige Zugangs-Token → Token erstellen*.
4. In Homarr *Benutzerdefinierte Widgets → Import* wählen und die JSON-Datei unverändert hochladen.
5. Über *⋯ → Quellen konfigurieren* folgende Werte eintragen:

   | Feld | Wert |
   |---|---|
   | Adresse | Adresse von Home Assistant ohne `/` am Ende, z. B. `https://homeassistant.example.com` oder `http://192.168.1.10:8123` |
   | Authentifizierung | *Bearer* (Voreinstellung) |
   | Token | das Token aus Schritt 3 |

6. Das Widget zu einem Board hinzufügen und in den Widget-Einstellungen die Optionen setzen:

   | Option | Bedeutung | Standard |
   |---|---|---|
   | Präfix der Sensoren | Präfix aus Schritt 2 | `claude_usage_my_account` |
   | Modell-Limit | Modellteil des Wochenlimits pro Modell, z. B. `sonnet` aus `…_weekly_sonnet_limit_usage`. Leer lassen, um die Zeile auszublenden. | `sonnet` |

## Anpassen

| Was | Wo in der JSON-Datei | Standard |
|---|---|---|
| Schwellen für Orange und Rot | `template`: `l.pct >= 90 ? "red" : l.pct >= 75 ? "orange"` (zweimal) | 75 % und 90 % |
| Zeitzone des Reset-Zeitpunkts | `template`: `Date.toLocaleString(…, "de-DE", "Europe/Berlin")` | `Europe/Berlin` |
| Zwischenspeicher der Antworten | `requests.*.cacheSeconds` | `60` Sekunden |

## Verwendete API-Endpunkte

Alle Abfragen gehen an die REST-API von Home Assistant. `<präfix>` ist die Option *Präfix der Sensoren*, `<modell>` die Option *Modell-Limit*.

| Request | Methode | Pfad |
|---|---|---|
| `session` | GET | `/api/states/sensor.<präfix>_session_limit_usage` |
| `weekly` | GET | `/api/states/sensor.<präfix>_weekly_all_models_limit_usage` |
| `model` | GET | `/api/states/sensor.<präfix>_weekly_<modell>_limit_usage` |
| `extra` | GET | `/api/states/sensor.<präfix>_extra_usage` |
| `apiError` | GET | `/api/states/sensor.<präfix>_api_error` |

## Fehlerbehebung

- **„Sensor … nicht gefunden – Präfix prüfen“:** Das Präfix stimmt nicht, oder die Entität wurde in Home Assistant umbenannt.
- **Fehler 401:** Das Token in *Quellen konfigurieren* ist falsch oder wurde gelöscht.
- **Zeile für das Modell fehlt:** Für dieses Modell gibt es kein eigenes Limit, oder der Modellteil ist falsch geschrieben. Unter *Einstellungen → Entitäten* nach `weekly_` suchen.

---

[← Zurück zur Übersicht](../../README.md) · Lizenz: siehe [LICENSE](../../LICENSE)
