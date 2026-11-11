# OptoCopler4CanWmodD1

ESP8266 (Wemos D1 mini) + Optokoppler, parallel zu den Tasten der Original-Fernbedienung
einer Falmec Aura Dunstabzugshaube. Steuerung per MQTT.

## MQTT

| Topic | Payload | Wirkung |
|---|---|---|
| `OptoCoppler2MQTT/on` | `1` / `on` / `true` / `press` | Taste „Ein / Stufe +“ |
| `OptoCoppler2MQTT/off` | dto. | Taste „Aus / Stufe −“ |
| `OptoCoppler2MQTT/light` | dto. | Taste Licht |
| `OptoCoppler2MQTT/timer` | dto. | Taste Timer |
| `OptoCoppler2MQTT/<devname>/status` | `online` / `offline` (retained, LWT) | Verfügbarkeit |
| `OptoCoppler2MQTT/<devname>/info` | startup, version, ip, rssi, reconnect | Info |
| `OptoCoppler2MQTT/<devname>/last_command` | z. B. `on` oder `on (wake)` | Quittung nach ausgeführtem Tastendruck |

Befehle **ohne Retain-Flag** senden – sonst wird bei jedem Reconnect der letzte Befehl erneut „gedrückt“.

## Fehlersuche „erster Befehl geht nicht“

1. Kommt `last_command` an, aber die Haube reagiert nicht → Problem auf Fernbedienungs-/Haubenseite
   (Schlafmodus der FB, Drückdauer, Batterie, Optokoppler). → `WAKEUP_PULSE_ENABLED` bzw. `PRESS_MS` in `config.h` testen.
2. Kommt **kein** `last_command` (und im Seriellen Monitor kein „Message arrived“) → Netzwerkproblem
   (WLAN-Modem-Sleep, tote MQTT-Verbindung). Ist in v0.1.0 durch `WIFI_NONE_SLEEP` und kürzeres Keepalive adressiert.

Alle Zeiten und Optionen: siehe `config.h`.
